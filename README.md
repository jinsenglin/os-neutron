# Background

os-controller installation

```
apt install -y neutron-server \
               neutron-plugin-ml2
```

os-network installation

```
apt install -y neutron-plugin-ml2 \
               neutron-openvswitch-agent \
               neutron-l3-agent \
               neutron-dhcp-agent \
               neutron-metadata-agent
```

os-compute installation

```
apt install -y neutron-plugin-ml2 \
               neutron-openvswitch-agent
```

---

os-contrller update for plus lbaasv2 haproxy

```
apt-get install -y python-neutron-lbaas
```

os-network update for plus lbaasv2 haproxy

```
apt-get install -y neutron-lbaasv2-agent
```
