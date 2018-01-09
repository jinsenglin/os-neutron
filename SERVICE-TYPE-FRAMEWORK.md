REF https://wiki.openstack.org/wiki/Neutron/ServiceTypeFramework

Those provider options should be specified in multiline form, one per provider in format:

```
<service_type>:<name>:<driver>[:default]

Where: - service_type is a string identifying a service, one of allowed services. In Havana the next list of services will be allowed: LOADBALANCER, FIREWALL, VPN, ROUTER.
- name - user-facing representation of the provider
- driver - driver identification. That could be a classpath, or some other identification (stevedore usage is considered)
- default option. If specified, the provider becomes default for given service type.

There could be only one default provider for a given service type, driver identification should be unique across all providers.
Name and service type must also be unique. 
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
