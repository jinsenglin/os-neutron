REF https://docs.openstack.org/oslo.service/latest/index.html

oslo.service provides a framework for defining new long-running services using the patterns established by other OpenStack applications. It also includes utilities long-running applications might need for working with SSL or WSGI, performing periodic operations, interacting with systemd, etc.

---

REF https://docs.openstack.org/oslo.service/latest/user/usage.html#migrating-to-oslo-service

The oslo.service library no longer assumes a global configuration object is available. Instead the following functions and classes have been changed to expect the consuming application to pass in an oslo.config configuration object:

* initialize_if_enabled()
* oslo_service.periodic_task.PeriodicTasks
* launch()
* oslo_service.service.ProcessLauncher
* oslo_service.service.ServiceLauncher
* is_enabled()
* wrap()

---

REF https://docs.openstack.org/oslo.service/latest/configuration/index.html#wsgi

```
DEFAULT

api_paste_config
    Type:       string
    Default:    api-paste.ini

    File name for the paste.deploy config for api service
```

---

REF https://github.com/openstack/oslo.service/blob/master/oslo_service/wsgi.py#L320

```
class Loader(object):
    """Used to load WSGI applications from paste configurations."""

    def __init__(self, conf):
        """Initialize the loader, and attempt to find the config.
        :param conf: Application config
        :returns: None
        """
        conf.register_opts(_options.wsgi_opts)
        self.config_path = None

        config_path = conf.api_paste_config
        if not os.path.isabs(config_path):
            self.config_path = conf.find_file(config_path)
        elif os.path.exists(config_path):
            self.config_path = config_path

        if not self.config_path:
            raise ConfigNotFound(path=config_path)

    def load_app(self, name):
        """Return the paste URLMap wrapped WSGI application.
        :param name: Name of the application to load.
        :returns: Paste URLMap object wrapping the requested application.
        :raises: PasteAppNotFound
        """
        try:
            LOG.debug("Loading app %(name)s from %(path)s",
                      {'name': name, 'path': self.config_path})
            return deploy.loadapp("config:%s" % self.config_path, name=name)
        except LookupError:
            LOG.exception("Couldn't lookup app: %s", name)
            raise PasteAppNotFound(name=name, path=self.config_path)
```
