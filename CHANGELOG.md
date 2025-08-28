# Changelog

All notable changes to this project will be documented in this file.

## [1.1.17] - 2025-08-28

### Role - agent2

- Fixed logrotate configuration file name.

## [1.1.16] - 2025-06-11

### Collection

- Changed repository URL to use GitHub Organization.
- Corrected missing or extra dependencies.

## [1.1.15] - 2025-05-16

### Role - agent2

- Changed OS validation.

### Role - nginx

- Changed OS validation.

### Role - proxy

- Changed OS validation.

### Role - proxy_postgresql

- Changed OS validation.

### Role - proxy_sqlite3

- Changed OS validation.

### Role - repo

- Changed OS validation.

### Role - server

- Changed OS validation.

### Role - server_postgresql

- Changed OS validation.

## [1.1.14] - 2025-03-31

### Role - agent2

- Fixed handler bug that caused Windows systems to not restart the service.

## [1.1.13] - 2025-03-31

### Role - agent2

- Fixed task include bugs.

## [1.1.12] - 2025-03-27

### Role - server

- Fixed typo in SAML task.

## [1.1.11] - 2025-03-27

### Role - server

- Fixed typo in SAML attribute.

## [1.1.10] - 2025-03-10

### Role - agent2

- Fixed include/import tasks paths.

## [1.1.9] - 2025-03-06

### Role - agent2

- Added support for Fedora.

### Role - repo

- Added support for Fedora.

## [1.1.8] - 2025-01-09

### Collection

- Added Changelog.
- Updated collection README documentation.
- *proxy_sqlite3* role added.

### Role - agent2

- Removed support for version 6.4.

### Role - host

- Added the `zbxhost_proxy_group` variable. It is mutually exclusive with the `zbxhost_proxy` variable.
- Fixed a bug that caused the `monitored_by` parameter to not be set on the host when `zbxhost_proxy` was defined.

### Role - nginx

- Removed support for version 6.4.

### Role - proxy

- Removed support for version 6.4.
- Changed default value of the `zbxproxy_database_type` variable from `postgresql` to `sqlite3`.
- Changed the `zbxproxy_database_name` variable to remove the default when `sqlite3` is the value of the `zbxproxy_database_type` variable.

### Role - proxy_postgresql

- Removed support for version 6.4.

### Role - server

- Removed support for version 6.4.

### Role - server_postgresql

- Removed support for version 6.4.

## [1.1.7] - 2024-12-17

### Collection

- *proxy* role added.
- *proxy_postgresql* role added.

### Role - agent2

- The `zbxagent_major_version` has been made mandatory on Linux systems to prevent unintentional major version upgrades.
- Added preliminary support for version 7.2.  This cannot be tested in an automated manner until *community.zabbix.zabbix* httpapi plugin is updated for version 7.2.
- Changed the way the version number is determined for the Windows plugin URL to use the Chocolatey package version.
- Updated documentation and role metadata for readability.

### Role - nginx

- The `required` option has been added to the `zbxserver_major_version` variable and the default value removed to prevent unintentional major version upgrades.
- Added preliminary support for version 7.2.  This cannot be tested in an automated manner until *community.zabbix.zabbix* httpapi plugin is updated for version 7.2.

### Role - repo

- Added preliminary support for version 7.2.  This cannot be tested in an automated manner until *community.zabbix.zabbix* httpapi plugin is updated for version 7.2.

### Role - server

- The `required` option has been added to the `zbxserver_major_version` variable and the default value removed to prevent unintentional major version upgrades.
- Added preliminary support for version 7.2.  This cannot be tested in an automated manner until *community.zabbix.zabbix* httpapi plugin is updated for version 7.2.
- Added missing configuration directives for version 7.0+.
- Removed tasks for configuring MSSQL ODBC.  The Zabbix Agent 2 plugins are mature for monitoring this.  If this functionality is needed, it should be done outside of this role.

### Role - server_postgresql

- The `required` option has been added to the `zbxserver_major_version` variable and the default value removed to prevent unintentional major version upgrades.
- Added preliminary support for version 7.2.  This cannot be tested in an automated manner until *community.zabbix.zabbix* httpapi plugin is updated for version 7.2.

## [1.1.6] - 2024-10-01

### Role - agent2

- Fixed bug that caused the agent plugins to always be installed.

## [1.1.5] - 2024-09-18

### Role - host

- Added `throttle` option to host group creation task to prevent multiple hosts from trying to create the same host groups simultaneously.

## [1.1.4] - 2024-09-18

### Role - server

- Added explicit group memberships to the default `Admin` user password change, since the API assumes all groups are needed when they are not supplied.

## [1.1.3] - 2024-09-18

### Role - host

- Fixed bug that caused VMware UUID to always be looked up, even when it was already supplied through the `zbxhost_vmware_uuid` variable.

## [1.1.2] - 2024-09-06

### Role - agent2

- Added `no_log` option to the `zbxagent_mongodb_default_password`, `zbxagent_mssql_default_password`, and `zbxagent_pgsql_default_password` variables.
- Added `no_log` option to the `password` option of the `zbxagent_mssql_sessions` and `zbxagent_pgsql_sessions` variables.

### Role - host

- Added `no_log` option to the `vmware_password` and `zabbix_password` variables.

### Role - nginx

- Added `no_log` option to the `zbxserver_database_password` variable.

### Role - server

- Added `no_log` option to the `zbxserver_database_password` variable.
- Added `no_log` option to the `password` option of the `zbxserver_vault_managed_users` and `zbxserver_unmanaged_users` variables.
- Added `no_log` option to the `bind_password` option of the `zbxserver_ldap_directories` variable.

### Role - server_postgresql

- Added `no_log` option to the `zabbix_database_password` variable.

## [1.1.1] - 2024-08-09

### Collection

- Minimum Ansible version changed from `2.14` to `2.15` due to EOL status.

## [1.1.0] - 2024-08-09

### Collection

- Added **ansible.utils** collection dependency at version `>=2.2.0`.
- Changed version requirement for **community.general** collection dependency from `>=1.0.0` to `>=8.1.0`.
- *host* role added.

### Role - agent2

- Removed tasks for adding the host to the Zabbix server from this role.  They have been moved to the *host* role in this collection.

## [1.0.0] - 2024-08-08

### Collection

- Initial release.
- *agent2* role added.
- *nginx* role added.
- *repo* role added.
- *server* role added.
- *server_postgresql* role added.
