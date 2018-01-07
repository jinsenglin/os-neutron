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

# Entrypoint

Tips: console script

see ENTRYPOINT.md 

# SUMMARY

neutron/neutron.conf

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
