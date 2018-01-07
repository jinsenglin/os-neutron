REF https://wiki.openstack.org/wiki/Neutron/ServiceTypeFramework

Those provider options should be specified in multiline form, one per provider in format:

```
<service_type>:<name>:<driver>[:default]
```

Example of neutron.conf section:

```
[service_providers] 
service_provider=LOADBALANCER:reference:quantum.services.loadbalancer.plugin_drivers.haproxy.HaproxyOnHost:default
service_provider=LOADBALANCER:net_scaler:quantum.services.loadbalancer.plugin_drivers.netscaler.NetScDriver
service_provider=VPN:strong_swan:quantum.services.vpn.drivers.StrongSwan
```

Example (LBaaS):

![Scenario](https://wiki.openstack.org/w/images/7/77/Call_dispatching_workflow.png)
