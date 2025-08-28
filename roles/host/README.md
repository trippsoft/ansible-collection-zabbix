<!-- BEGIN_ANSIBLE_DOCS -->

# Ansible Role: trippsc2.zabbix.host
Version: 1.1.17

This role configures a host within Zabbix Server.

## Requirements


## Dependencies

| Collection |
| ---------- |
| ansible.utils |
| community.general |
| community.vmware |
| community.zabbix |

## Role Arguments
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| vmware_hostname | <p>The hostname or IP address of the vCenter server or standalone ESXi host.</p><p>If *zbxhost_vmware_uuid* is not defined and *zbxhost_configure_vmware_monitoring* is `true`, this is required.</p> | str | no |  |  |
| vmware_username | <p>The username to use for connecting to the vCenter server or standalone ESXi host.</p><p>If *zbxhost_vmware_uuid* is not defined and *zbxhost_configure_vmware_monitoring* is `true`, this is required.</p> | str | no |  |  |
| vmware_password | <p>The password to use for connecting to the vCenter server or standalone ESXi host.</p><p>If *zbxhost_vmware_uuid* is not defined and *zbxhost_configure_vmware_monitoring* is `true`, this is required.</p> | str | no |  |  |
| vmware_datacenter | <p>The name of the datacenter in which the VM is located.</p><p>If using a standalone ESXi host, this should be set to `ha-datacenter`.</p><p>If *zbxhost_vmware_uuid* is not defined and *zbxhost_configure_vmware_monitoring* is `true`, this is required.</p> | str | no |  |  |
| vmware_vm_folder | <p>The folder in which the VM is located.</p><p>If *zbxhost_vmware_uuid* is not defined and *zbxhost_configure_vmware_monitoring* is `true`, this is required.</p><p>For formatting, see https://docs.ansible.com/ansible/latest/collections/community/vmware/vmware_guest_info_module.html#parameter-folder.</p> | str | no |  |  |
| vmware_vm_name | <p>The name of the VM to monitor.</p><p>If *zbxhost_vmware_uuid* is not defined and *zbxhost_configure_vmware_monitoring* is `true`, this is required.</p> | str | no |  |  |
| zabbix_host | <p>The hostname or IP address of the Zabbix server for API requests.</p> | str | yes |  |  |
| zabbix_port | <p>The port on which to connect to Zabbix server for API requests.</p> | int | yes |  |  |
| zabbix_url_path | <p>The URL path for the Zabbix API.</p><p>If using Apache for web frontend, this should be set to `zabbix` unless configured otherwise.</p><p>If using NGINX for web frontend, this should be set to an empty string unless configured otherwise.</p> | str | no |  |  |
| zabbix_ssl | <p>Whether to use SSL for API requests.</p> | bool | no |  | False |
| zabbix_validate_certs | <p>Whether to validate SSL certificates for API requests.</p><p>If not specified, the default behavior is to validate certificates.</p> | bool | no |  |  |
| zabbix_username | <p>The username to use for API requests.</p> | str | yes |  |  |
| zabbix_password | <p>The password to use for API requests.</p> | str | yes |  |  |
| zbxhost_configure_vmware_monitoring | <p>Whether to configure VMware monitoring for the host.</p> | bool | no |  | False |
| zbxagent_tls_connect | <p>The type of connection to use for connections to the Zabbix server for active checks.</p><p>This should not be supplied when not using a Zabbix agent interface.</p><p>Linux reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_agent2#tlsconnect</p><p>Windows reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_agent2_win#tlsconnect</p> | str | no | <ul><li>unencrypted</li><li>psk</li><li>cert</li></ul> | unencrypted |
| zbxagent_tls_accept | <p>The type of connection to accept for connections from the Zabbix server.</p><p>This should not be supplied when not using a Zabbix agent interface.</p><p>Linux reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_agent2#tlsaccept</p><p>Windows reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_agent2_win#tlsaccept</p> | str | no | <ul><li>unencrypted</li><li>psk</li><li>cert</li></ul> | unencrypted |
| zbxhost_name | <p>The name of the host on the Zabbix Server.</p> | str | yes |  |  |
| zbxhost_visible_name | <p>The visible name of the host on the Zabbix Server.</p><p>If not specified, the visible name will not be set on the host.</p> | str | no |  |  |
| zbxhost_groups | <p>A list of host groups to which the host on the Zabbix Server belongs.</p> | list of 'str' | yes |  |  |
| zbxhost_interfaces | <p>A list of interfaces for the host on the Zabbix Server.</p> | list of dicts of 'zbxhost_interfaces' options | yes |  |  |
| zbxhost_templates | <p>A list of templates to link to the host on the Zabbix Server.</p> | list of 'str' | yes |  |  |
| zbxhost_proxy | <p>The name of the Zabbix proxy to use for the host.</p><p>This is mutually exclusive with `zbxhost_proxy_group`.</p><p>If not specified, the host will not use a proxy.</p> | str | no |  |  |
| zbxhost_proxy_group | <p>The name of the Zabbix proxy group to use for the host.</p><p>This is mutually exclusive with `zbxhost_proxy`.</p><p>If not specified, the host will not use a proxy group.</p> | str | no |  |  |
| zbxhost_macros | <p>A dictionary of macros to set for the host on the Zabbix Server.</p> | list of dicts of 'zbxhost_macros' options | no |  | [] |
| zbxhost_vmware_uuid | <p>The UUID of the VM to monitor.</p><p>If *zbxhost_configure_vmware_monitoring* is `false`, this is ignored.</p><p>Otherwise, this value will be used to set the VM UUID host macro, instead of querying the vCenter server or standalone ESXi host.</p> | str | no |  |  |

### Options for zbxhost_interfaces
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| type | <p>Interface type to add.</p><p>Reference: https://docs.ansible.com/ansible/latest/collections/community/zabbix/zabbix_host_module.html#parameter-interfaces/type</p> | str | no | <ul><li>agent</li><li>snmp</li><li>ipmi</li><li>jmx</li></ul> | agent |
| main | <p>Whether the interface is used as default.</p><p>If multiple interfaces with the same type are provided, only one can be default.</p><p>`0` (not default), `1` (default)</p><p>Reference: https://docs.ansible.com/ansible/latest/collections/community/zabbix/zabbix_host_module.html#parameter-interfaces/main</p> | int | yes | <ul><li>0</li><li>1</li></ul> |  |
| useip | <p>Connect to host interface with IP address instead of DNS name.</p><p>`0` (don’t use ip), `1` (use ip)</p><p>Reference: https://docs.ansible.com/ansible/latest/collections/community/zabbix/zabbix_host_module.html#parameter-interfaces/useip</p> | int | yes | <ul><li>0</li><li>1</li></ul> |  |
| ip | <p>IP address used by host interface.</p><p>Required if useip=`1`.</p><p>Reference: https://docs.ansible.com/ansible/latest/collections/community/zabbix/zabbix_host_module.html#parameter-interfaces/ip</p> | str | no |  |  |
| dns | <p>DNS name of the host interface.</p><p>Required if useip=`0`.</p><p>Reference: https://docs.ansible.com/ansible/latest/collections/community/zabbix/zabbix_host_module.html#parameter-interfaces/dns</p> | str | no |  |  |
| port | <p>Port used by host interface.</p><p>If not specified, default port for each type of interface is used</p><p>`10050` if type=”agent”</p><p>`161` if type=”snmp”</p><p>`623` if type=”ipmi”</p><p>`12345` if type=”jmx”</p><p>Reference: https://docs.ansible.com/ansible/latest/collections/community/zabbix/zabbix_host_module.html#parameter-interfaces/port</p> | int | no |  |  |
| details | <p>Additional details for SNMP host interfaces.</p><p>Reference: https://docs.ansible.com/ansible/latest/collections/community/zabbix/zabbix_host_module.html#parameter-interfaces/details</p> | dict of 'details' options | no |  |  |

### Options for zbxhost_interfaces > details
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| authpassphrase | <p>SNMPv3 authentication passphrase.</p><p>Used when securitylevel=`1`(authNoPriv) or securitylevel=`2`(AuthPriv).</p><p>Reference: https://docs.ansible.com/ansible/latest/collections/community/zabbix/zabbix_host_module.html#parameter-interfaces/details/authpassphrase</p> | str | no |  |  |
| authprotocol | <p>SNMPv3 authentication protocol.</p><p>Used when securitylevel=1(authNoPriv) or securitylevel=2(AuthPriv).</p><p>`0` (MD5), `1` (SHA1), `2` (SHA224), `3` (SHA256), `4` (SHA384), `5` (SHA512)</p><p>Reference: https://docs.ansible.com/ansible/latest/collections/community/zabbix/zabbix_host_module.html#parameter-interfaces/details/authprotocol</p> | int | no | <ul><li>0</li><li>1</li><li>2</li><li>3</li><li>4</li><li>5</li></ul> | 0 |
| bulk | <p>Whether to use bulk SNMP requests.</p><p>`0` (don’t use bulk requests), `1` (use bulk requests)</p><p>Reference: https://docs.ansible.com/ansible/latest/collections/community/zabbix/zabbix_host_module.html#parameter-interfaces/details/bulk</p> | int | no | <ul><li>0</li><li>1</li></ul> | 1 |
| community | <p>SNMPv1 and SNMPv2 community string.</p><p>Required when version=`1` or version=`2`.</p><p>Reference: https://docs.ansible.com/ansible/latest/collections/community/zabbix/zabbix_host_module.html#parameter-interfaces/details/community</p> | str | no |  |  |
| contextname | <p>SNMPv3 context name.</p><p>Reference: https://docs.ansible.com/ansible/latest/collections/community/zabbix/zabbix_host_module.html#parameter-interfaces/details/contextname</p> | str | no |  |  |
| privpassphrase | <p>SNMPv3 privacy passphrase.</p><p>Used when securitylevel=`2`(AuthPriv).</p><p>Reference: https://docs.ansible.com/ansible/latest/collections/community/zabbix/zabbix_host_module.html#parameter-interfaces/details/privpassphrase</p> | str | no |  |  |
| privprotocol | <p>SNMPv3 privacy protocol.</p><p>Used when securitylevel=`2`(AuthPriv).</p><p>`0` (DES), `1` (AES128), `2` (AES192), `3` (AES256), `4` (AES192C), `5` (AES256C)</p><p>Reference: https://docs.ansible.com/ansible/latest/collections/community/zabbix/zabbix_host_module.html#parameter-interfaces/details/privprotocol</p> | int | no | <ul><li>0</li><li>1</li><li>2</li><li>3</li><li>4</li><li>5</li></ul> | 0 |
| securitylevel | <p>SNMPv3 security level.</p><p>`0` (noAuthNoPriv), `1` (authNoPriv), `2` (AuthPriv)</p><p>Reference: https://docs.ansible.com/ansible/latest/collections/community/zabbix/zabbix_host_module.html#parameter-interfaces/details/securitylevel</p> | int | no | <ul><li>0</li><li>1</li><li>2</li></ul> | 0 |
| securityname | <p>SNMPv3 security name.</p><p>Reference: https://docs.ansible.com/ansible/latest/collections/community/zabbix/zabbix_host_module.html#parameter-interfaces/details/securityname</p> | str | no |  |  |
| version | <p>SNMP version.</p><p>`1` (SNMPv1), `2` (SNMPv2c), `3` (SNMPv3)</p><p>Reference: https://docs.ansible.com/ansible/latest/collections/community/zabbix/zabbix_host_module.html#parameter-interfaces/details/version</p> | int | no | <ul><li>1</li><li>2</li><li>3</li></ul> | 2 |

### Options for zbxhost_macros
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| name | <p>The macro name.</p> | str | yes |  |  |
| type | <p>The macro type.</p> | str | no | <ul><li>text</li><li>secret</li><li>vault</li></ul> | text |
| value | <p>The macro value.</p> | str | yes |  |  |


## License
MIT

## Author and Project Information
Jim Tarpley (@trippsc2)
<!-- END_ANSIBLE_DOCS -->
