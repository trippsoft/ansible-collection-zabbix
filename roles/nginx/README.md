<!-- BEGIN_ANSIBLE_DOCS -->

# Ansible Role: trippsc2.zabbix.nginx
Version: 1.1.17

This role installs NGINX as a web frontend for Zabbix Server on a Linux system.

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
| ansible.posix |
| trippsc2.nginx |

## Role Arguments
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| zbxserver_major_version | <p>The major version of Zabbix to install.</p><p>This defaults to the latest supported version for the OS.</p> | str | yes | <ul><li>7.2</li><li>7.0</li></ul> |  |
| zbxserver_nginx_module_version | <p>The version of the NGINX module to install on EL systems.</p><p>This has no effect on other systems.</p><p>This defaults to the latest supported version for the OS.</p> | str | no | <ul><li>1.24</li><li>1.22</li><li>1.20</li></ul> |  |
| zbxserver_php_module_version | <p>The version of the PHP module to install on EL systems.</p><p>This has no effect on other systems.</p><p>This defaults to the latest supported version for the OS.</p><p>The offered module versions for each EL major version can be found here: https://access.redhat.com/support/policy/updates/rhel-app-streams-life-cycle</p> | str | no | <ul><li>8.2</li><li>8.1</li><li>8.0</li><li>7.4</li></ul> |  |
| zbxserver_configure_selinux | <p>Whether to configure SELinux for Zabbix.</p><p>For EL systems, this defaults to true.</p><p>For other systems, this defaults to false.</p> | bool | no |  |  |
| zbxserver_configure_vault | <p>Whether to configure HashiCorp Vault for Zabbix.</p> | bool | no |  | True |
| zbxserver_database_type | <p>The type of database to use for Zabbix.</p> | str | no | <ul><li>postgresql</li><li>mysql</li></ul> | postgresql |
| zbxserver_timezone | <p>The timezone to use for Zabbix in TZ format.</p><p>For more information, see: https://en.wikipedia.org/wiki/List_of_tz_database_time_zones</p> | str | no |  | America/New_York |
| zbxserver_database_host | <p>The name of the database server for Zabbix.</p> | str | yes |  |  |
| zbxserver_database_port | <p>The port number for the database server for Zabbix.</p> | int | no |  | 5432 |
| zbxserver_database_name | <p>The name of the database for Zabbix.</p> | str | no |  | zabbix-server |
| zbxserver_user | <p>The name of the user for Zabbix.</p><p>This is not used in this role, if zbxserver_configure_vault is true.</p> | str | no |  | zabbix |
| zbxserver_database_password | <p>The password for the Zabbix database user.</p><p>This is not used in this role, if zbxserver_configure_vault is true.</p> | str | no |  |  |
| zbxserver_database_use_ssl | <p>Whether to use SSL for the Zabbix database.</p> | bool | no |  | False |
| zbxserver_database_ca_file | <p>The path to the CA file for the Zabbix database.</p> | str | no |  |  |
| zbxserver_database_verify_host | <p>Whether to verify the host for the Zabbix database.</p> | bool | no |  | True |
| zbxserver_vault_url | <p>The URL for the HashiCorp Vault server.</p> | str | no |  |  |
| zbxserver_vault_database_mount_point | <p>The mount point for the Zabbix database secret.</p> | str | no |  | zabbix |
| zbxserver_vault_database_path | <p>The path for the Zabbix database secret.</p> | str | no |  | database |
| zbxserver_vault_token | <p>The token for the HashiCorp Vault server.</p> | str | no |  |  |
| zbxserver_server_host | <p>The host for the Zabbix server.</p> | str | yes |  |  |
| zbxserver_server_port | <p>The port for the Zabbix server.</p> | int | no |  | 10051 |
| zbxserver_display_name | <p>The display name for Zabbix.</p> | str | no |  | ZABBIX |
| zbxserver_nginx_redirect_server_name | <p>A list of server names on which to redirect HTTP requests to HTTPS.</p><p>If set to '_', requests for any server name will be redirected.</p> | list of 'str' | no |  | ['_'] |
| zbxserver_nginx_redirect_return_server_name | <p>The server name to return in the redirect.</p><p>By default, this uses the hostname of the request.</p> | str | no |  | $host |
| zbxserver_nginx_max_body_size | <p>The maximum body size for NGINX.</p> | str | no |  | 2M |
| zbxserver_nginx_server_name | <p>A list of server names to accept HTTPS connections to Zabbix.</p><p>If set to '_', requests for any server name will be accepted.</p> | list of 'str' | no |  | ['_'] |
| zbxserver_web_server_certificate_path | <p>The path to the certificate for the web server.</p> | str | yes |  |  |
| zbxserver_web_server_private_key_path | <p>The path to the private key for the web server.</p> | str | yes |  |  |


## License
MIT

## Author and Project Information
Jim Tarpley (@trippsc2)
<!-- END_ANSIBLE_DOCS -->
