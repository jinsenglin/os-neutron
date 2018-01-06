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

OpenStack Neutron ML2 Deep Dive

https://www.ibm.com/developerworks/cn/cloud/library/cl-cn-openstackneutronml2/index.html

图 1. ML2 与 Neutron L2 agents

![scenario](https://www.ibm.com/developerworks/cn/cloud/library/cl-cn-openstackneutronml2/img001.png)

图 2. ML2 框架

![scenario](https://www.ibm.com/developerworks/cn/cloud/library/cl-cn-openstackneutronml2/img002.png)

---

OpenStack Neutron ML2

https://zhuanlan.zhihu.com/p/29833977

圖 1. Neutron Server

![scenario](https://pic3.zhimg.com/50/v2-88601bcf226283ce65980e16c082e0c8_hd.jpg)

圖 2. ML2 Core Plugin

![scenario](https://pic4.zhimg.com/50/v2-b24e7ba68f76e231566b6eae568222b7_hd.jpg)

---
