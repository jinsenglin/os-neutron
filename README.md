# Background

os-controller installation

```
apt install -y neutron-server \
               neutron-plugin-ml2

# Edit the /etc/neutron/neutron.conf file, [database] section
# Edit the /etc/neutron/neutron.conf file, [DEFAULT] section
# Edit the /etc/neutron/neutron.conf file, [keystone_authtoken] section
# Edit the /etc/neutron/neutron.conf file, [nova] section
# Edit the /etc/neutron/plugins/ml2/ml2_conf.ini file, [ml2] section
# Edit the /etc/neutron/plugins/ml2/ml2_conf.ini file, [ml2_type_flat] section
# Edit the /etc/neutron/plugins/ml2/ml2_conf.ini file, [ml2_type_vxlan] section
# Edit the /etc/neutron/plugins/ml2/ml2_conf.ini file, [securitygroup] section

su -s /bin/sh -c "neutron-db-manage --config-file /etc/neutron/neutron.conf --config-file /etc/neutron/plugins/ml2/ml2_conf.ini upgrade head" neutron

service neutron-server restart
```

os-network installation

```
apt install -y neutron-plugin-ml2 \
               neutron-openvswitch-agent \
               neutron-l3-agent \
               neutron-dhcp-agent \
               neutron-metadata-agent

# Edit the /etc/neutron/neutron.conf file, [database] section
# Edit the /etc/neutron/neutron.conf file, [DEFAULT] section
# Edit the /etc/neutron/neutron.conf file, [keystone_authtoken] section
# Edit the /etc/neutron/plugins/ml2/ml2_conf.ini file, [ml2] section
# Edit the /etc/neutron/plugins/ml2/ml2_conf.ini file, [ml2_type_flat] section
# Edit the /etc/neutron/plugins/ml2/ml2_conf.ini file, [ml2_type_vxlan] section
# Edit the /etc/neutron/plugins/ml2/ml2_conf.ini file, [securitygroup] section
# Edit the /etc/neutron/plugins/ml2/openvswitch_agent.ini file, [ovs] section
# Edit the /etc/neutron/plugins/ml2/openvswitch_agent.ini file, [agent] section
# Edit the /etc/neutron/plugins/ml2/openvswitch_agent.ini file, [securitygroup] section
# Edit the /etc/neutron/l3_agent.ini file, [DEFAULT] section
# Edit the /etc/neutron/dhcp_agent.ini file, [DEFAULT] section
# Edit the /etc/neutron/metadata_agent.ini file, [DEFAULT] section

service openvswitch-switch restart
service neutron-openvswitch-agent restart
service neutron-dhcp-agent restart
service neutron-metadata-agent restart
service neutron-l3-agent restart
```

os-compute installation

```
apt install -y neutron-plugin-ml2 \
               neutron-openvswitch-agent

# Edit the /etc/neutron/neutron.conf file, [database] section
# Edit the /etc/neutron/neutron.conf file, [DEFAULT] section
# Edit the /etc/neutron/neutron.conf file, [keystone_authtoken] section
# Edit the /etc/neutron/plugins/ml2/openvswitch_agent.ini file, [ovs] section
# Edit the /etc/neutron/plugins/ml2/openvswitch_agent.ini file, [agent] section
# Edit the /etc/neutron/plugins/ml2/openvswitch_agent.ini file, [securitygroup] section

service openvswitch-switch restart
service neutron-openvswitch-agent restart
```

---

os-contrller update for plus lbaasv2 haproxy

```
apt-get install -y python-neutron-lbaas

# Edit the /etc/neutron/neutron.conf file, [DEFAULT] section
# Edit the /etc/neutron/neutron_lbaas.conf file, [service_providers] section

neutron-db-manage --subproject neutron-lbaas upgrade head

service neutron-server restart
```

os-network update for plus lbaasv2 haproxy

```
apt-get install -y neutron-lbaasv2-agent

# Edit the /etc/neutron/lbaas_agent.ini file, [DEFAULT] section
# Edit the /etc/neutron/neutron_lbaas.conf file, [service_providers] section

service neutron-lbaasv2-agent start
```

---

os-controller update for plus fwaasv1

```
# Edit the /etc/neutron/neutron.conf file, [DEFAULT] section
# Edit the /etc/neutron/neutron.conf file, [service_providers] section
# Edit the /etc/neutron/neutron.conf file, [fwaas] section
# Edit the /etc/neutron/fwaas_driver.ini file, [fwaas] section
# Edit the /etc/neutron/l3_agent.ini file, [AGENT] section

neutron-db-manage --subproject neutron-fwaas upgrade head

service neutron-server restart
```

os-network update for plus fwaasv1

```
# Edit the /etc/neutron/neutron.conf file, [DEFAULT] section
# Edit the /etc/neutron/neutron.conf file, [service_providers] section
# Edit the /etc/neutron/neutron.conf file, [fwaas] section
# Edit the /etc/neutron/fwaas_driver.ini file, [fwaas] section
# Edit the /etc/neutron/l3_agent.ini file, [AGENT] section

service neutron-l3-agent restart
```

# Trace from service XXXX start

e.g. neutron-server

soft link file /etc/systemd/system/multi-user.target.wants/neutron-server.service -> /lib/systemd/system/neutron-server.service

```
# file: /lib/systemd/system/neutron-server.service

[Service]
ExecStart=/etc/init.d/neutron-server systemd-start
```

```
# file: /etc/init.d/neutron-server

PROJECT_NAME=neutron
NAME=${PROJECT_NAME}-server
[ -r /etc/default/neutron-server ] && . /etc/default/neutron-server
[ -n "$NEUTRON_PLUGIN_CONFIG" ] && DAEMON_ARGS="--config-file=$NEUTRON_PLUGIN_CONFIG"

if [ -z "${DAEMON}" ] ; then
        DAEMON=/usr/bin/${NAME}
fi

if [ -z "${CONFIG_FILE}" ] ; then
        CONFIG_FILE=/etc/${PROJECT_NAME}/${PROJECT_NAME}.conf
fi

if [ -z "${NO_OPENSTACK_CONFIG_FILE_DAEMON_ARG}" ] ; then
    DAEMON_ARGS="--config-file=${CONFIG_FILE} ${DAEMON_ARGS}"
fi

do_systemd_start() {
        exec $DAEMON $DAEMON_ARGS
}

case "$1" in
systemd-start)
        do_systemd_start
;;
esac

# file: /etc/default/neutron-server

NEUTRON_PLUGIN_CONFIG="/etc/neutron/plugins/ml2/ml2_conf.ini"
```

```
# file: /usr/bin/neurton-server

from neutron.cmd.eventlet.server import main


if __name__ == "__main__":
    sys.exit(main())

# ps aux | grep neurton-server

/usr/bin/python /usr/bin/neutron-server --config-file=/etc/neutron/neutron.conf --config-file=/etc/neutron/plugins/ml2/ml2_conf.ini --log-file=/var/log/neutron/neutron-server.log
/usr/bin/python /usr/bin/neutron-server --config-file=/etc/neutron/neutron.conf --config-file=/etc/neutron/plugins/ml2/ml2_conf.ini --log-file=/var/log/neutron/neutron-server.log
/usr/bin/python /usr/bin/neutron-server --config-file=/etc/neutron/neutron.conf --config-file=/etc/neutron/plugins/ml2/ml2_conf.ini --log-file=/var/log/neutron/neutron-server.log
/usr/bin/python /usr/bin/neutron-server --config-file=/etc/neutron/neutron.conf --config-file=/etc/neutron/plugins/ml2/ml2_conf.ini --log-file=/var/log/neutron/neutron-server.log
/usr/bin/python /usr/bin/neutron-server --config-file=/etc/neutron/neutron.conf --config-file=/etc/neutron/plugins/ml2/ml2_conf.ini --log-file=/var/log/neutron/neutron-server.log
```

```
# file: https://github.com/openstack/neutron/blob/adc344c065c4c6bb2e29e9a6c9a6618163ddfbe7/neutron/cmd/eventlet/server/__init__.py

from neutron import server
from neutron.server import wsgi_eventlet


def main():
    server.boot_server(wsgi_eventlet.eventlet_wsgi_server)
 
# file: https://github.com/openstack/neutron/blob/adc344c065c4c6bb2e29e9a6c9a6618163ddfbe7/neutron/server/__init__.py 

def boot_server(server_func):
    _init_configuration()
    try:
        server_func()
    except KeyboardInterrupt:
        pass
    except RuntimeError as e:
        sys.exit(_("ERROR: %s") % e)

# file: https://github.com/openstack/neutron/blob/adc344c065c4c6bb2e29e9a6c9a6618163ddfbe7/neutron/server/wsgi_eventlet.py

from neutron import service


def eventlet_wsgi_server():
    neutron_api = service.serve_wsgi(service.NeutronApiService)
    start_api_and_rpc_workers(neutron_api)


def start_api_and_rpc_workers(neutron_api):
    try:
        worker_launcher = service.start_all_workers()

        pool = eventlet.GreenPool()
        api_thread = pool.spawn(neutron_api.wait)
        plugin_workers_thread = pool.spawn(worker_launcher.wait)

        # api and other workers should die together. When one dies,
        # kill the other.
        api_thread.link(lambda gt: plugin_workers_thread.kill())
        plugin_workers_thread.link(lambda gt: api_thread.kill())

        pool.waitall()
    except NotImplementedError:
        LOG.info("RPC was already started in parent process by "
                 "plugin.")

        neutron_api.wait()

# file: https://github.com/openstack/neutron/blob/adc344c065c4c6bb2e29e9a6c9a6618163ddfbe7/neutron/service.py

from oslo_config import cfg
from neutron.common import config


class WsgiService(object):
    """Base class for WSGI based services.
    For each api you define, you must also define these flags:
    :<api>_listen: The address on which to listen
    :<api>_listen_port: The port on which to listen
    """

    def __init__(self, app_name):
        self.app_name = app_name
        self.wsgi_app = None

    def start(self):
        self.wsgi_app = _run_wsgi(self.app_name)

    def wait(self):
        self.wsgi_app.wait()


class NeutronApiService(WsgiService):
    """Class for neutron-api service."""
    def __init__(self, app_name):
        profiler.setup('neutron-server', cfg.CONF.host)
        super(NeutronApiService, self).__init__(app_name)

    @classmethod
    def create(cls, app_name='neutron'):
        # Setup logging early
        config.setup_logging()
        service = cls(app_name)
        return service


def serve_wsgi(cls):

    try:
        service = cls.create()
        service.start()
    except Exception:
        with excutils.save_and_reraise_exception():
            LOG.exception('Unrecoverable error: please check log '
                          'for details.')

    registry.notify(resources.PROCESS, events.BEFORE_SPAWN, service)
    return service


def start_all_workers(): https://github.com/openstack/neutron/blob/adc344c065c4c6bb2e29e9a6c9a6618163ddfbe7/neutron/service.py#L262


def _run_wsgi(app_name):
    app = config.load_paste_app(app_name)
    if not app:
        LOG.error('No known API applications configured.')
        return
    return run_wsgi_app(app)


def run_wsgi_app(app):
    server = wsgi.Server("Neutron")
    server.start(app, cfg.CONF.bind_port, cfg.CONF.bind_host,
                 workers=_get_api_workers())
    LOG.info("Neutron service started, listening on %(host)s:%(port)s",
             {'host': cfg.CONF.bind_host, 'port': cfg.CONF.bind_port})
    return server

# file: https://github.com/openstack/neutron/blob/7c1e21a3f35e80e176dfc025eb6f4a0024cb137c/neutron/common/config.py

from oslo_service import wsgi


def load_paste_app(app_name):
    """Builds and returns a WSGI app from a paste config file.
    :param app_name: Name of the application to load
    """
    loader = wsgi.Loader(cfg.CONF)
    app = loader.load_app(app_name)
    return app
```

```
# https://docs.openstack.org/oslo.service/latest/configuration/index.html#wsgi

Usage

from oslo_config import cfg
from oslo_service import service

CONF = cfg.CONF
launcher = service.launch(CONF, service, workers=2)

Configuration

[DEFAULT]
api_paste_config = api-paste.ini # Default
```

```
# file: https://github.com/openstack/neutron/blob/adc344c065c4c6bb2e29e9a6c9a6618163ddfbe7/etc/api-paste.ini

[composite:neutron]
use = egg:Paste#urlmap
/: neutronversions_composite
/v2.0: neutronapi_v2_0

[composite:neutronapi_v2_0]
use = call:neutron.auth:pipeline_factory
noauth = cors http_proxy_to_wsgi request_id catch_errors extensions neutronapiapp_v2_0
keystone = cors http_proxy_to_wsgi request_id catch_errors authtoken keystonecontext extensions neutronapiapp_v2_0

[filter:extensions]
paste.filter_factory = neutron.api.extensions:plugin_aware_extension_middleware_factory

[app:neutronapiapp_v2_0]
paste.app_factory = neutron.api.v2.router:APIRouter.factory
```

```
# ref: Ocata Neutron代碼分析（四）——api-paste.ini分析 https://hk.saowen.com/a/03fb8fc302b77a111c9fcb6859147f534cf7fd5f66c3746c844ae349cad69091

use 指定要調用的函數為 /neutron/auth.py 的 pipeline_factory。
noauth 和 keystone 作為參數 local_conf（dict）傳入該函數。
```

```
# ref: Ocata Neutron代碼分析（四）——api-paste.ini分析 https://hk.saowen.com/a/03fb8fc302b77a111c9fcb6859147f534cf7fd5f66c3746c844ae349cad69091

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

```
# ref: Ocata Neutron代碼分析（四）——api-paste.ini分析 https://hk.saowen.com/a/03fb8fc302b77a111c9fcb6859147f534cf7fd5f66c3746c844ae349cad69091

class APIRouter(base_wsgi.Router):

    @classmethod
    def factory(cls, global_config, **local_config):
        return cls(**local_config)

    def __init__(self, **local_config):
        ......

---

def plugin_aware_extension_middleware_factory(global_config, **local_config):
    """Paste factory."""
    def _factory(app):
        ext_mgr = PluginAwareExtensionManager.get_instance()
        return ExtensionMiddleware(app, ext_mgr=ext_mgr)
    return _factory

該函數中只包含一個_factory函數，函數中首先獲取了PluginAwareExtensionManager的實例，該類是一個單例模式的實現，在APIRouter初始化時初始化。接着，將ext_mgr作為參數初始化ExtensionMiddleware，ExtensionMiddleware的構造函數主要是將extensions中的resource進行map，而頂級的resource（network、subnet和port）將在APIRouter的構造函數中進行map。
```

```
# ref: http://routes.readthedocs.io/en/latest/

Routes is a Python re-implementation of the Rails routes system for mapping URLs to application actions, and conversely to generate URLs. Routes makes it easy to create pretty and concise URLs that are RESTful with little effort.
```

# Entrypoint

Tips: console script

see ENTRYPOINT.md 

# Summary 1

/etc/neutron/neutron.conf

```
[DEFAULT]
core_plugin = ml2 
service_plugins = router,fwaas,lbaasv2
```

neutron/setup.cfg

```
[entry_points]
neutron.core_plugins =
    ml2 = neutron.plugins.ml2.plugin:Ml2Plugin
neutron.service_plugins =
    router = neutron.services.l3_router.l3_router_plugin:L3RouterPlugin
```

neutron-fwaas/setup.cfg

```
[entry_points]
neutron.service_plugins =
    firewall = neutron_fwaas.services.firewall.fwaas_plugin:FirewallPlugin
```

neutron-lbaas/setup.cfg

```
[entry_points]
neutron.service_plugins =
    lbaasv2 = neutron_lbaas.services.loadbalancer.plugin:LoadBalancerPluginv2
```

# Summary 2

/etc/neutron/plugins/ml2/ml2_conf.ini

```
[ml2]
type_drivers = flat,vlan,vxlan
mechanism_drivers = openvswitch,l2population
extension_drivers = port_security
```

neutron/setup.cfg

```
[entry_points]
neutron.ml2.type_drivers =
    flat = neutron.plugins.ml2.drivers.type_flat:FlatTypeDriver
    local = neutron.plugins.ml2.drivers.type_local:LocalTypeDriver
    vlan = neutron.plugins.ml2.drivers.type_vlan:VlanTypeDriver
neutron.ml2.mechanism_drivers =
    openvswitch = neutron.plugins.ml2.drivers.openvswitch.mech_driver.mech_openvswitch:OpenvswitchMechanismDriver
    l2population = neutron.plugins.ml2.drivers.l2pop.mech_driver:L2populationMechanismDriver
neutron.ml2.extension_drivers =
    port_security = neutron.plugins.ml2.extensions.port_security:PortSecurityExtensionDriver
```

# Summary 3

Q: service plugin vs. service provider?

A: see SERVICE-TYPE-FRAMEWORK.md

service_providers example 1 (configure_neutron_fwaasv1)

```
# file: /etc/neutron/neutron.conf

[service_providers]
service_provider = FIREWALL:Iptables:neutron.agent.linux.iptables_firewall.OVSHybridIptablesFirewallDriver:default
```

service_providers example 2 (configure_neutron_lbaasv2_haproxy)

```
# file: /etc/neutron/neutron_lbaas.conf

[service_providers]
service_provider = LOADBALANCERV2:Haproxy:neutron_lbaas.drivers.haproxy.plugin_driver.HaproxyOnHostPluginDriver:default
```
