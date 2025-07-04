<!-- BEGIN_ANSIBLE_DOCS -->

# Ansible Role: trippsc2.zabbix.proxy_postgresql
Version: 1.1.16

This role installs PostgreSQL server for use by a Zabbix Proxy on a Linux system.

This role uses **trippsc2.postgresql.install** to install PostgreSQL.
The *pgsql_additional_hba_entries* variable is overridden by this role. The rest of the role variables can be set in the playbook or inventory.


## Requirements

| Platform | Versions |
| -------- | -------- |
| Debian | <ul><li>bookworm</li></ul> |
| EL | <ul><li>9</li><li>8</li></ul> |
| Ubuntu | <ul><li>noble</li><li>jammy</li></ul> |

## Dependencies
| Role |
| ---- |
| trippsc2.zabbix.repo |

| Collection |
| ---------- |
| community.hashi_vault |
| community.postgresql |
| trippsc2.hashi_vault |
| trippsc2.postgresql |

## Role Arguments
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| vault_url | <p>The URL for accessing HashiCorp Vault.</p><p>Alternatively, this can be configured through ansible.cfg or environment variables.</p> | str | no |  |  |
| vault_token | <p>The token for accessing HashiCorp Vault.</p><p>Alternatively, this (or any other authentication method) can be configured through ansible.cfg or environment variables.</p> | str | no |  |  |
| zbxproxy_major_version | <p>The major version of Zabbix to install.</p> | str | yes | <ul><li>7.2</li><li>7.0</li></ul> |  |
| zbxproxy_pgsql_standalone | <p>Whether the PostgreSQL server is separate from the Zabbix Proxy.</p><p>If set to `true`, *pgsql_listen_on_local_only* defaults to `false` (and must be `false`) and *pgsql_listen_on_all_addresses* defaults to `true`.</p><p>If set to `false`, *pgsql_listen_on_local_only* defaults to `true` and *pgsql_listen_on_all_addresses* defaults to `false`.</p> | bool | no |  | False |
| zbxproxy_pgsql_additional_hba_entries | <p>Additional host-based authentication entries to add to the pg_hba.conf file.</p><p>These entries are in addition to the default entries from the **trippsc2.postgresql.install** role and the entries added by this role.</p> | list of dicts of 'zbxproxy_pgsql_additional_hba_entries' options | no |  | [] |
| zbxproxy_ip_addresses | <p>The IP addresses of the Zabbix proxy.</p><p>If *zbxproxy_pgsql_standalone* is `true`, this is required.</p><p>A pg_hba.conf entry will be created to access the Zabbix proxy database authenticated with the Zabbix proxy user via scram-sha-256 for each IP address.</p> | list of 'str' | no |  |  |
| zbxproxy_configure_vault | <p>Whether to configure HashiCorp Vault for the Zabbix database secret.</p><p>If `true`, the Zabbix database password will be stored in Vault.</p><p>If *zabbix_database_password* is defined and an existing password is not stored in Vault, the supplied password will be stored there.</p><p>If *zabbix_database_password* is not defined, a new password will be generated and stored in Vault.</p> | bool | no |  | True |
| zbxproxy_vault_create_mount_points | <p>Whether to create HashiCorp Vault the expected mount points, if they do not exist.</p> | bool | no |  | True |
| zabbix_database_password | <p>The password for the Zabbix database user.</p><p>If *zbxproxy_configure_vault* is true and HashiCorp Vault already stores the secret, this value is ignored.</p> | str | no |  |  |
| zbxproxy_vault_database_mount_point | <p>The HashiCorp Vault mount point where the Zabbix KV2 database secret is to be stored.</p> | str | no |  |  |
| zbxproxy_vault_database_path | <p>The path in HashiCorp Vault where the Zabbix KV2 database secret is to be stored.</p> | str | no |  |  |
| zbxproxy_user | <p>The PostgreSQL user to be used by the Zabbix proxy.</p><p>A Linux user with the same name will be created, if it does not exist.</p><p>The Linux user will be used for peer authentication to the PostgreSQL server during initial configuration.</p> | str | no |  | zabbix |
| zbxproxy_group | <p>The Linux primary group of the *zbxproxy_user* user.</p> | str | no |  | zabbix |
| zbxproxy_database_name | <p>The name of the Zabbix Proxy database.</p> | str | no |  | zabbix-proxy |

### Options for zbxproxy_pgsql_additional_hba_entries
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| type | <p>The type of host-based authentication entry.</p><p>See https://www.postgresql.org/docs/current/auth-pg-hba-conf.html#AUTH-PG-HBA-CONF for more information.</p> | str | yes | <ul><li>local</li><li>host</li><li>hostssl</li><li>hostnossl</li><li>hostgssenc</li><li>hostnogssenc</li></ul> |  |
| database | <p>The name of the database to which the entry applies.</p><p>This can be `all` to apply to all databases.</p> | str | yes |  |  |
| user | <p>The name of the user to which the entry applies.</p><p>This can be `all` to apply to all users.</p> | str | yes |  |  |
| address | <p>The CIDR range to which the entry applies.</p><p>This must not be provided for `local` entries.</p> | str | no |  |  |
| auth_method | <p>The authentication method to use.</p><p>See https://www.postgresql.org/docs/current/auth-pg-hba-conf.html#AUTH-PG-HBA-CONF for more information.</p> | str | yes | <ul><li>trust</li><li>reject</li><li>scram-sha-256</li><li>md5</li><li>password</li><li>gss</li><li>ident</li><li>peer</li><li>ldap</li><li>radius</li><li>cert</li><li>pam</li></ul> |  |
| auth_options | <p>Any additional options needed for the authentication method.</p><p>For most common authentication methods, this should not be provided.</p><p>See https://www.postgresql.org/docs/current/auth-pg-hba-conf.html#AUTH-PG-HBA-CONF for more information.</p> | str | no |  |  |


## License
MIT

## Author and Project Information
Jim Tarpley (@trippsc2)
<!-- END_ANSIBLE_DOCS -->
