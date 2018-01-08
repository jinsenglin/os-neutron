REF https://hk.saowen.com/a/03fb8fc302b77a111c9fcb6859147f534cf7fd5f66c3746c844ae349cad69091

在Neutron API啟動過程分析中，曾分析到加載wsgi app是通過load_paste_app函數首先實例化oslo_service.wsgi.py中的Loader類，返回一個loader對象。然後再調用loader對象的load_app函數來實現的。

```
def load_paste_app(app_name):
    """Builds and returns a WSGI app from a paste config file.

    :param app_name: Name of the application to load
    """
    loader = wsgi.Loader(cfg.CONF)
    app = loader.load_app(app_name)
    return app
```

```
class Loader(object):

    def load_app(self, name):
        """Return the paste URLMap wrapped WSGI application.

        :param name: Name of the application to load.
        :returns: Paste URLMap object wrapping the requested application.
        :raises: PasteAppNotFound

        """
        try:
            LOG.debug("Loading app %(name)s from %(path)s",
                      {'name': name, 'path': self.config_path})
            return deploy.loadapp("config:%s" % self.config_path, name=name)    # 調用第三方庫paste.deploy的loadapp函數來加載wsgi_app並返回
        except LookupError:
            LOG.exception("Couldn't lookup app: %s", name)
            raise PasteAppNotFound(name=name, path=self.config_path)
```

下面分析一下paste.deploy的配置文檔（即self.config_path），該配置文檔默認為api-paste.ini。

```
[composite:neutron]
use = egg:Paste#urlmap
/: neutronversions_composite
/v2.0: neutronapi_v2_0

[composite:neutronapi_v2_0]
use = call:neutron.auth:pipeline_factory
noauth = cors http_proxy_to_wsgi request_id catch_errors extensions neutronapiapp_v2_0
keystone = cors http_proxy_to_wsgi request_id catch_errors authtoken keystonecontext extensions neutronapiapp_v2_0

[composite:neutronversions_composite]
use = call:neutron.auth:pipeline_factory
noauth = cors http_proxy_to_wsgi neutronversions
keystone = cors http_proxy_to_wsgi neutronversions

[filter:request_id]
paste.filter_factory = oslo_middleware:RequestId.factory

[filter:catch_errors]
paste.filter_factory = oslo_middleware:CatchErrors.factory

[filter:cors]
paste.filter_factory = oslo_middleware.cors:filter_factory
oslo_config_project = neutron

[filter:http_proxy_to_wsgi]
paste.filter_factory = oslo_middleware.http_proxy_to_wsgi:HTTPProxyToWSGI.factory

[filter:keystonecontext]
paste.filter_factory = neutron.auth:NeutronKeystoneContext.factory

[filter:authtoken]
paste.filter_factory = keystonemiddleware.auth_token:filter_factory

[filter:extensions]
paste.filter_factory = neutron.api.extensions:plugin_aware_extension_middleware_factory

[app:neutronversions]
paste.app_factory = neutron.api.versions:Versions.factory

[app:neutronapiapp_v2_0]
paste.app_factory = neutron.api.v2.router:APIRouter.factory

[filter:osprofiler]
paste.filter_factory = osprofiler.web:WsgiMiddleware.factory
```

這裏只分析api-paste.ini的處理流程，具體paste配置文檔的語法請參考其他文檔。
app_name為該配置文檔的入口，在NeutronApiService.create函數中傳入了app_name為neutron。這裏neutron的composite用Paste.urlmap來構造wsgi app：

* url為'/'：由neutronversions_composite處理；
* url為'/v2.0'：由neutronapi_v2_0處理。

neutronversions_composite的處理很簡單，在response的body中返回Neutron API版本的相關信息。

下面重點分析neutronapi_v2_0。

```
[composite:neutronapi_v2_0]
use = call:neutron.auth:pipeline_factory
noauth = cors http_proxy_to_wsgi request_id catch_errors extensions neutronapiapp_v2_0
keystone = cors http_proxy_to_wsgi request_id catch_errors authtoken keystonecontext extensions neutronapiapp_v2_0
```

use指定要調用的函數為/neutron/auth.py的pipeline_factory。noauth和keystone作為參數local_conf（dict）傳入該函數。

```
def pipeline_factory(loader, global_conf, **local_conf):
    """Create a paste pipeline based on the 'auth_strategy' config option."""
    pipeline = local_conf[cfg.CONF.auth_strategy]           # 配置文檔中auth_strategy設置是否需要驗證token（口令）
    pipeline = pipeline.split()                             # pipeline：str -> list
    filters = [loader.get_filter(n) for n in pipeline[:-1]] # 獲取所有的filters（排除最後一個neutronapiapp_v2_0，因為它是app），形成list
    app = loader.get_app(pipeline[-1])                      # neutronapiapp_v2_0中調用的類（APIRouter）在這一步被初始化，下一節分析
    filters.reverse()                                       # 反向filters列表，最靠近APIRouter的filter最先執行，即第一個執行的是extensions filter
    for filter in filters:
        app = filter(app)                                   # 將app作為參數依次傳入每個filter
    return app
```

配置文檔中的auth_strategy默認為keystone，即默認需要對token進行驗證。下面重點分析一下app:neutronapiapp_v2_0和filter:extensions。

1.neutronapiapp_v2_0

```
[app:neutronapiapp_v2_0]
paste.app_factory = neutron.api.v2.router:APIRouter.factory
```

直接調用/neutron/api/v2/router.py中APIRouter的factory方法：

```
class APIRouter(base_wsgi.Router):

    @classmethod
    def factory(cls, global_config, **local_config):
        return cls(**local_config)

    def __init__(self, **local_config):
        ......
```

APIRouter的factory方法是一個classmethod，直接調用APIRouter的構造函數，對APIRouter進行初始化，這部分下一節分析。

2.extensions

```
[filter:extensions]
paste.filter_factory = neutron.api.extensions:plugin_aware_extension_middleware_factory
```

extensions filter直接調用/neutron/api/extensions.py中的plugin_aware_extension_middleware_factory方法：

```
def plugin_aware_extension_middleware_factory(global_config, **local_config):
    """Paste factory."""
    def _factory(app):
        ext_mgr = PluginAwareExtensionManager.get_instance()
        return ExtensionMiddleware(app, ext_mgr=ext_mgr)
    return _factory
```

該函數中只包含一個_factory函數，函數中首先獲取了PluginAwareExtensionManager的實例，該類是一個單例模式的實現，在APIRouter初始化時初始化，在下一節進行分析。
接着，將ext_mgr作為參數初始化ExtensionMiddleware，ExtensionMiddleware的構造函數主要是將extensions中的resource進行map，而頂級的resource（network、subnet和port）將在APIRouter的構造函數中進行map，也將在下一節進行分析。
