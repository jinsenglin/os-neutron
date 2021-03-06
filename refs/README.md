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

Neutron/ServiceTypeFramework

Currently considering model is "one service plugin - multiple drivers".

![scenario](https://wiki.openstack.org/w/images/7/77/Call_dispatching_workflow.png)

---

学习 Neutron 系列文章：Neutron 所实现的虚拟化网络

http://www.cnblogs.com/sammyliu/p/4622563.html

圖 1. 这是 RedHat 提供的一个 OpenStack Cloud network 网络架构

![scenario](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux_OpenStack_Platform/5/html/Installation_and_Configuration_Guide/images/5113.png)

圖 2. Neutron OpenvSwitch + GRE/VxLAN 虚拟网络

![scenario](https://pinrojas.files.wordpress.com/2014/07/neutron-openstack-openvswitch-router-dnsmasq-gre-kionetworks-plug-in-tap-devices-instances-nova-compute-software-defined-networks-sdn-cloud-layer-4.jpg)

圖 3. 未使用DVR

![scenario](https://wiki.openstack.org/w/images/thumb/a/a7/Architecuture-1.png/500px-Architecuture-1.png)

圖 4. 使用DVR

![scenario](https://wiki.openstack.org/w/images/thumb/9/99/Dvr-architecture.png/500px-Dvr-architecture.png)

---

https://github.com/lorin/openstack-hackspace/blob/master/under-the-hood-network.md

![scenario](https://github.com/lorin/openstack-hackspace/raw/master/network-under-hood.png)

---

http://superuser.openstack.org/articles/everything-you-need-to-know-to-get-started-with-neutron-f90e2797-26b7-4d1c-84d8-effef03f11d2/

![scenario](https://aptira.com/wp-content/uploads/2016/03/41.png)

---

Additional Posts

* neutron plugin 与 extension 编写流程 http://blog.csdn.net/u010341430/article/details/51332171
* 通过neutron的extension添加restful接口和代码跨版本迁移 http://blog.csdn.net/liu_baoqing/article/details/54947082
* openstack之neutron代码分析---(1)neutron初始化流程 http://blog.csdn.net/u013920085/article/details/50099147
* 如何区分 OpenStack Neutron Extension 和 Plugin http://www.cnblogs.com/zhutianshi/p/3902315.html
* 怎样写 OpenStack Neutron 的 Extension （一）http://www.aboutyun.com/thread-10480-1-1.html
* NEUTRON中的plugin和extension介绍及加载机制 http://blog.csdn.net/llxx1234/article/details/60590575
* openstack plugin 之（三）怎样写 OpenStack Neutron 的 Extension http://blog.csdn.net/hejin_some/article/details/72730156
