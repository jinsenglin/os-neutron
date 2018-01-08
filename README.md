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

class NeutronApiService(WsgiService): https://github.com/openstack/neutron/blob/adc344c065c4c6bb2e29e9a6c9a6618163ddfbe7/neutron/service.py#L68

def serve_wsgi(cls): https://github.com/openstack/neutron/blob/adc344c065c4c6bb2e29e9a6c9a6618163ddfbe7/neutron/service.py#L82

def start_all_workers(): https://github.com/openstack/neutron/blob/adc344c065c4c6bb2e29e9a6c9a6618163ddfbe7/neutron/service.py#L262
```

Next ?

Tips 1: https://github.com/openstack/neutron/blob/adc344c065c4c6bb2e29e9a6c9a6618163ddfbe7/neutron/service.py#L85

Tips 1: https://github.com/openstack/neutron/blob/adc344c065c4c6bb2e29e9a6c9a6618163ddfbe7/neutron/service.py#L75

Tips 1: https://github.com/openstack/neutron/blob/adc344c065c4c6bb2e29e9a6c9a6618163ddfbe7/neutron/service.py#L78

Tips 1: https://github.com/openstack/neutron/blob/adc344c065c4c6bb2e29e9a6c9a6618163ddfbe7/neutron/service.py#L72

Tips 1: https://github.com/openstack/neutron/blob/adc344c065c4c6bb2e29e9a6c9a6618163ddfbe7/neutron/service.py#L62

Tips 1: https://github.com/openstack/neutron/blob/adc344c065c4c6bb2e29e9a6c9a6618163ddfbe7/neutron/service.py#L289

Tips 1: https://github.com/openstack/neutron/blob/7c1e21a3f35e80e176dfc025eb6f4a0024cb137c/neutron/common/config.py#L122 # loader = wsgi.Loader(cfg.CONF); app = loader.load_app(app_name) # app_name = 'neutron'

Tips 1: https://github.com/openstack/neutron/blob/7c1e21a3f35e80e176dfc025eb6f4a0024cb137c/neutron/common/config.py#L28 # from oslo_service import wsgi

Tips 1: https://github.com/openstack/neutron/blob/7c1e21a3f35e80e176dfc025eb6f4a0024cb137c/etc/api-paste.ini#L1 # [composite:neutron]

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
