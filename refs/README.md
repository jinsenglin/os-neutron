OpenStack Neutron Plugins and Agents

http://www.innervoice.in/blogs/2015/03/31/openstack-neutron-plugins-and-agents/

![scenario](https://i2.wp.com/www.innervoice.in/blogs/wp-content/uploads/2015/03/Plugin-Agents.jpg?w=807)

---

Neutron plugin体系

http://zhouyaguo.github.io/openstack/2015/11/26/neutron-plugin-arch/

ml2 type driver, see neutron/neutron/plugins/ml2/drivers/, e.g.

* flat
* vlan
* vxlan

ml2 mechanism driver, see neutron/neutron/neutron/plugins/ml2/drivers/, e.g.

* linuxbridge
* openvswitch 

ml2 extension driver, see neutron/neutron/extensions/, e.g.

* qos
* securitygroup
* metering
* dvr

透過 extensions, 提供各种除了network，subnet，port之外的其他服务

neutron-lbaas and neutron-fwaas are also ml2 extension drivers.

---
