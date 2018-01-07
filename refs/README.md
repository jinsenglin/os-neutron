Neutron Internals

https://docs.openstack.org/neutron/latest/contributor/index.html#neutron-internals

---

Neutron Configuration Options

https://docs.openstack.org/neutron/latest/configuration/index.html

* neutron.conf
* ml2_conf.ini
* linuxbridge_agent.ini
* openvswitch_agent.ini
* dhcp_agent.ini
* l3_agent.ini
* metadata_agent.ini
* metering_agent.ini

https://docs.openstack.org/ocata/config-reference/networking/lbaas.html

* neutron_lbaas.conf
* lbaas_agent.ini
* services_lbaas.conf
* octavia.conf

https://docs.openstack.org/ocata/config-reference/networking/fwaas.html

* fwaas_driver.ini

---

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

ml2 mechanism driver, see neutron/neutron/plugins/ml2/drivers/, e.g.

* linuxbridge
* openvswitch 

ml2 extension driver, see neutron/neutron/extensions/ (定義api) and neutron/neutron/plugins/ml2/extensions/, e.g.

* router
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

图 3. ML2 创建 Network 流程

![scenario](https://www.ibm.com/developerworks/cn/cloud/library/cl-cn-openstackneutronml2/img003.png)

图 4. ML2 创建 Port 流程

![scenario](https://www.ibm.com/developerworks/cn/cloud/library/cl-cn-openstackneutronml2/img004.png)

---

OpenStack Neutron ML2

https://zhuanlan.zhihu.com/p/29833977

core plugin vs. service plugin

Neutron中的service plugin 都定义在 neutron/neutron/services  这个目录, e.g.

* router
* qos
* metering

圖 1. Neutron Server

![scenario](https://pic3.zhimg.com/50/v2-88601bcf226283ce65980e16c082e0c8_hd.jpg)

圖 2. ML2 Core Plugin

![scenario](https://pic4.zhimg.com/50/v2-b24e7ba68f76e231566b6eae568222b7_hd.jpg)

---

学习 Neutron 系列文章：Neutron 所实现的虚拟化网络

http://www.cnblogs.com/sammyliu/p/4622563.html

圖 1. 这是 RedHat 提供的一个 OpenStack Cloud network 网络架构

![scenario](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux_OpenStack_Platform/5/html/Installation_and_Configuration_Guide/images/5113.png)

圖 2. 未使用DVR

![scenario](https://wiki.openstack.org/w/images/thumb/a/a7/Architecuture-1.png/500px-Architecuture-1.png)

圖 3. 使用DVR

![scenario](https://wiki.openstack.org/w/images/thumb/9/99/Dvr-architecture.png/500px-Dvr-architecture.png)

