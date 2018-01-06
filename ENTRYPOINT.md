# Entrypoints

defined in the file neutron/setup.cfg

```
[entry_points]
wsgi_scripts =
    neutron-api = neutron.server:get_application
console_scripts =
    neutron-db-manage = neutron.db.migration.cli:main
    neutron-debug = neutron.debug.shell:main
    neutron-dhcp-agent = neutron.cmd.eventlet.agents.dhcp:main
    neutron-keepalived-state-change = neutron.cmd.keepalived_state_change:main
    neutron-ipset-cleanup = neutron.cmd.ipset_cleanup:main
    neutron-l3-agent = neutron.cmd.eventlet.agents.l3:main
    neutron-linuxbridge-agent = neutron.cmd.eventlet.plugins.linuxbridge_neutron_agent:main
    neutron-linuxbridge-cleanup = neutron.cmd.linuxbridge_cleanup:main
    neutron-macvtap-agent = neutron.cmd.eventlet.plugins.macvtap_neutron_agent:main
    neutron-metadata-agent = neutron.cmd.eventlet.agents.metadata:main
    neutron-netns-cleanup = neutron.cmd.netns_cleanup:main
    neutron-openvswitch-agent = neutron.cmd.eventlet.plugins.ovs_neutron_agent:main
    neutron-ovs-cleanup = neutron.cmd.ovs_cleanup:main
    neutron-pd-notify = neutron.cmd.pd_notify:main
    neutron-server = neutron.cmd.eventlet.server:main
    neutron-rpc-server = neutron.cmd.eventlet.server:main_rpc_eventlet
    neutron-rootwrap = oslo_rootwrap.cmd:main
    neutron-rootwrap-daemon = oslo_rootwrap.cmd:daemon
    neutron-usage-audit = neutron.cmd.eventlet.usage_audit:main
    neutron-metering-agent = neutron.cmd.eventlet.services.metering_agent:main
    neutron-sriov-nic-agent = neutron.cmd.eventlet.plugins.sriov_nic_neutron_agent:main
    neutron-sanity-check = neutron.cmd.sanity_check:main
neutron.core_plugins =
    ml2 = neutron.plugins.ml2.plugin:Ml2Plugin
neutron.service_plugins =
    dummy = neutron.tests.unit.dummy_plugin:DummyServicePlugin
    router = neutron.services.l3_router.l3_router_plugin:L3RouterPlugin
    metering = neutron.services.metering.metering_plugin:MeteringPlugin
    qos = neutron.services.qos.qos_plugin:QoSPlugin
    tag = neutron.services.tag.tag_plugin:TagPlugin
    flavors = neutron.services.flavors.flavors_plugin:FlavorsPlugin
    auto_allocate = neutron.services.auto_allocate.plugin:Plugin
    segments = neutron.services.segments.plugin:Plugin
    network_ip_availability = neutron.services.network_ip_availability.plugin:NetworkIPAvailabilityPlugin
    revisions = neutron.services.revisions.revision_plugin:RevisionPlugin
    timestamp = neutron.services.timestamp.timestamp_plugin:TimeStampPlugin
    trunk = neutron.services.trunk.plugin:TrunkPlugin
    loki = neutron.services.loki.loki_plugin:LokiPlugin
    logapi = neutron.services.logapi.logging_plugin:LoggingPlugin
neutron.ml2.type_drivers =
    flat = neutron.plugins.ml2.drivers.type_flat:FlatTypeDriver
    local = neutron.plugins.ml2.drivers.type_local:LocalTypeDriver
    vlan = neutron.plugins.ml2.drivers.type_vlan:VlanTypeDriver
    geneve = neutron.plugins.ml2.drivers.type_geneve:GeneveTypeDriver
    gre = neutron.plugins.ml2.drivers.type_gre:GreTypeDriver
    vxlan = neutron.plugins.ml2.drivers.type_vxlan:VxlanTypeDriver
neutron.ml2.mechanism_drivers =
    logger = neutron.tests.unit.plugins.ml2.drivers.mechanism_logger:LoggerMechanismDriver
    test = neutron.tests.unit.plugins.ml2.drivers.mechanism_test:TestMechanismDriver
    linuxbridge = neutron.plugins.ml2.drivers.linuxbridge.mech_driver.mech_linuxbridge:LinuxbridgeMechanismDriver
    macvtap = neutron.plugins.ml2.drivers.macvtap.mech_driver.mech_macvtap:MacvtapMechanismDriver
    openvswitch = neutron.plugins.ml2.drivers.openvswitch.mech_driver.mech_openvswitch:OpenvswitchMechanismDriver
    l2population = neutron.plugins.ml2.drivers.l2pop.mech_driver:L2populationMechanismDriver
    sriovnicswitch = neutron.plugins.ml2.drivers.mech_sriov.mech_driver.mech_driver:SriovNicSwitchMechanismDriver
    fake_agent = neutron.tests.unit.plugins.ml2.drivers.mech_fake_agent:FakeAgentMechanismDriver
    faulty_agent = neutron.tests.unit.plugins.ml2.drivers.mech_faulty_agent:FaultyAgentMechanismDriver
neutron.ml2.extension_drivers =
    test = neutron.tests.unit.plugins.ml2.drivers.ext_test:TestExtensionDriver
    testdb = neutron.tests.unit.plugins.ml2.drivers.ext_test:TestDBExtensionDriver
    port_security = neutron.plugins.ml2.extensions.port_security:PortSecurityExtensionDriver
    qos = neutron.plugins.ml2.extensions.qos:QosExtensionDriver
    dns = neutron.plugins.ml2.extensions.dns_integration:DNSExtensionDriverML2
    data_plane_status = neutron.plugins.ml2.extensions.data_plane_status:DataPlaneStatusExtensionDriver
    dns_domain_ports = neutron.plugins.ml2.extensions.dns_integration:DNSDomainPortsExtensionDriver
neutron.ipam_drivers =
    fake = neutron.tests.unit.ipam.fake_driver:FakeDriver
    internal = neutron.ipam.drivers.neutrondb_ipam.driver:NeutronDbPool
neutron.agent.l2.extensions =
    qos = neutron.agent.l2.extensions.qos:QosAgentExtension
    fdb = neutron.agent.l2.extensions.fdb_population:FdbPopulationAgentExtension
neutron.qos.agent_drivers =
    ovs = neutron.plugins.ml2.drivers.openvswitch.agent.extension_drivers.qos_driver:QosOVSAgentDriver
    sriov = neutron.plugins.ml2.drivers.mech_sriov.agent.extension_drivers.qos_driver:QosSRIOVAgentDriver
    linuxbridge = neutron.plugins.ml2.drivers.linuxbridge.agent.extension_drivers.qos_driver:QosLinuxbridgeAgentDriver
neutron.agent.linux.pd_drivers =
    dibbler = neutron.agent.linux.dibbler:PDDibbler
neutron.services.external_dns_drivers =
    designate = neutron.services.externaldns.drivers.designate.driver:Designate
oslo.config.opts =
    neutron = neutron.opts:list_opts
    neutron.agent = neutron.opts:list_agent_opts
    neutron.az.agent = neutron.opts:list_az_agent_opts
    neutron.base.agent = neutron.opts:list_base_agent_opts
    neutron.db = neutron.opts:list_db_opts
    neutron.dhcp.agent = neutron.opts:list_dhcp_agent_opts
    neutron.extensions = neutron.opts:list_extension_opts
    neutron.l3.agent = neutron.opts:list_l3_agent_opts
    neutron.metadata.agent = neutron.opts:list_metadata_agent_opts
    neutron.metering.agent = neutron.opts:list_metering_agent_opts
    neutron.ml2 = neutron.opts:list_ml2_conf_opts
    neutron.ml2.linuxbridge.agent = neutron.opts:list_linux_bridge_opts
    neutron.ml2.macvtap.agent = neutron.opts:list_macvtap_opts
    neutron.ml2.ovs.agent = neutron.opts:list_ovs_opts
    neutron.ml2.sriov.agent = neutron.opts:list_sriov_agent_opts
    neutron.ml2.xenapi = neutron.opts:list_xenapi_opts
    nova.auth = neutron.opts:list_auth_opts
oslo.config.opts.defaults =
    neutron = neutron.common.config:set_cors_middleware_defaults
neutron.db.alembic_migrations =
    neutron = neutron.db.migration:alembic_migrations
neutron.interface_drivers =
    ivs = neutron.agent.linux.interface:IVSInterfaceDriver
    linuxbridge = neutron.agent.linux.interface:BridgeInterfaceDriver
    null = neutron.agent.linux.interface:NullDriver
    openvswitch = neutron.agent.linux.interface:OVSInterfaceDriver
neutron.agent.firewall_drivers =
    noop = neutron.agent.firewall:NoopFirewallDriver
    iptables = neutron.agent.linux.iptables_firewall:IptablesFirewallDriver
    iptables_hybrid = neutron.agent.linux.iptables_firewall:OVSHybridIptablesFirewallDriver
    openvswitch = neutron.agent.linux.openvswitch_firewall:OVSFirewallDriver
neutron.services.metering_drivers =
    noop = neutron.services.metering.drivers.noop.noop_driver:NoopMeteringDriver
    iptables = neutron.services.metering.iptables.iptables_driver:IptablesMeteringDriver
tempest.test_plugins =
    neutron_tests = neutron.tests.tempest.plugin:NeutronTempestPlugin
```

---

defined in the neutron-lbaas/setup.cfg

```
[entry_points]
console_scripts =
    neutron-lbaasv2-agent = neutron_lbaas.cmd.lbaasv2_agent:main
loadbalancer_schedulers =
    neutron_lbaas.agent_scheduler.ChanceScheduler = neutron_lbaas.agent_scheduler:ChanceScheduler
neutron.service_plugins =
    lbaasv2 = neutron_lbaas.services.loadbalancer.plugin:LoadBalancerPluginv2
neutron.db.alembic_migrations =
    neutron-lbaas = neutron_lbaas.db.migration:alembic_migrations
neutron_lbaas.cert_manager.backend =
    barbican = neutron_lbaas.common.cert_manager.barbican_cert_manager
    local = neutron_lbaas.common.cert_manager.local_cert_manager
oslo.config.opts =
    neutron.lbaas = neutron_lbaas.opts:list_opts
    neutron.lbaas.agent = neutron_lbaas.opts:list_agent_opts
    neutron.lbaas.service = neutron_lbaas.opts:list_service_opts
neutron_lbaas.cert_manager.barbican_auth =
    barbican_acl_auth = neutron_lbaas.common.cert_manager.barbican_auth.barbican_acl:BarbicanACLAuth
tempest.test_plugins =
    neutron_lbaas = neutron_lbaas.tests.tempest.plugin:NeutronLbaasTempestPlugin

```

---

defined in the neutron-fwaas/setup.cfg

```
[entry_points]
firewall_drivers =
    # These are for backwards compat with Juno firewall service provider
    # configuration values
    neutron.services.firewall.drivers.linux.iptables_fwaas.IptablesFwaasDriver = neutron_fwaas.services.firewall.drivers.linux.iptables_fwaas:IptablesFwaasDriver
    iptables = neutron_fwaas.services.firewall.drivers.linux.iptables_fwaas:IptablesFwaasDriver
    iptables_v2 = neutron_fwaas.services.firewall.drivers.linux.iptables_fwaas_v2:IptablesFwaasDriver
neutron.service_plugins =
    firewall = neutron_fwaas.services.firewall.fwaas_plugin:FirewallPlugin
    firewall_v2 = neutron_fwaas.services.firewall.fwaas_plugin_v2:FirewallPluginV2
    neutron.services.firewall.fwaas_plugin.FirewallPlugin = neutron_fwaas.services.firewall.fwaas_plugin:FirewallPlugin

neutron.db.alembic_migrations =
    neutron-fwaas = neutron_fwaas.db.migration:alembic_migrations
tempest.test_plugins =
    neutron-fwaas = neutron_fwaas.tests.tempest_plugin.plugin:NeutronFWaaSPlugin
oslo.config.opts =
    neutron.fwaas = neutron_fwaas.opts:list_opts
    firewall.agent = neutron_fwaas.opts:list_agent_opts
neutron.agent.l3.extensions =
    fwaas = neutron_fwaas.services.firewall.agents.l3reference.firewall_l3_agent:L3WithFWaaS
    fwaas_v2 = neutron_fwaas.services.firewall.agents.l3reference.firewall_l3_agent_v2:L3WithFWaaS
neutron_fwaas.services.firewall.drivers.linux =
    conntrack = neutron_fwaas.services.firewall.drivers.linux.legacy_conntrack:ConntrackLegacy
    netlink_conntrack = neutron_fwaas.services.firewall.drivers.linux.netlink_conntrack:ConntrackNetlink
```