# Background

os-controller installation

```
apt install -y neutron-server \
               neutron-plugin-ml2

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

service openvswitch-switch restart
service neutron-openvswitch-agent restart
```

---

os-contrller update for plus lbaasv2 haproxy

```
apt-get install -y python-neutron-lbaas

neutron-db-manage --subproject neutron-lbaas upgrade head

service neutron-server restart
```

os-network update for plus lbaasv2 haproxy

```
apt-get install -y neutron-lbaasv2-agent

service neutron-lbaasv2-agent start
```

---

os-controller update for plus fwaasv1

```
neutron-db-manage --subproject neutron-fwaas upgrade head

service neutron-server restart
```

os-network update for plus fwaasv1

```
service neutron-l3-agent restart
```
