<!-- BEGIN_ANSIBLE_DOCS -->

# Ansible Role: trippsc2.zabbix.server
Version: 1.2.0

This role installs and configures the Zabbix server on a Linux system.

## Requirements

| Platform | Versions |
| -------- | -------- |
| Debian | <ul><li>trixie</li><li>bookworm</li></ul> |
| EL | <ul><li>10</li><li>9</li><li>8</li></ul> |
| Ubuntu | <ul><li>noble</li><li>jammy</li></ul> |

## Dependencies
| Role |
| ---- |
| trippsc2.zabbix.repo |

| Collection |
| ---------- |
| ansible.posix |
| community.general |
| community.hashi_vault |
| community.zabbix |
| trippsc2.hashi_vault |

## Role Arguments
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| vault_url | <p>The URL for accessing HashiCorp Vault.</p><p>Alternatively, this can be configured through ansible.cfg or environment variables.</p> | str | no |  |  |
| vault_token | <p>The token for accessing HashiCorp Vault.</p><p>Alternatively, this (or any other authentication method) can be configured through ansible.cfg or environment variables.</p> | str | no |  |  |
| zabbix_host | <p>The hostname or IP address of the Zabbix server for API requests.</p> | str | yes |  |  |
| zabbix_port | <p>The port on which to connect to Zabbix server for API requests.</p> | int | yes |  |  |
| zabbix_ssl | <p>Whether to use SSL for API requests.</p> | bool | no |  | False |
| zabbix_validate_certs | <p>Whether to validate SSL certificates for API requests.</p><p>If not specified, the default behavior is to validate certificates.</p> | bool | no |  |  |
| zabbix_url_path | <p>The URL path for the Zabbix API.</p><p>If using Apache for web frontend, this should be set to `zabbix` unless configured otherwise.</p><p>If using NGINX for web frontend, this should be set to an empty string unless configured otherwise.</p> | str | no |  |  |
| zabbix_username | <p>The username to use for API requests.</p> | str | yes |  |  |
| zabbix_password | <p>The password to use for API requests.</p> | str | yes |  |  |
| zbxserver_major_version | <p>The major version of Zabbix to install.</p><p>This defaults to the latest supported version for the OS.</p> | str | yes | <ul><li>7.4</li><li>7.2</li><li>7.0</li></ul> |  |
| zbxserver_configure_selinux | <p>Whether to manage SELinux settings for the Zabbix server.</p><p>For EL systems, this defaults to true.</p><p>For other systems, this defaults to false.</p> | bool | no |  |  |
| zbxserver_configure_firewall | <p>Whether to manage the host firewall for use with Zabbix server.</p> | bool | no |  | True |
| zbxserver_configure_logrotate | <p>Whether to manage logrotate settings for the Zabbix server.</p> | bool | no |  | True |
| zbxserver_configure_vault | <p>Whether to configure the Zabbix server to use HashiCorp Vault for database credentials and storing user credentials.</p> | bool | no |  | True |
| zbxserver_user | <p>The Linux user as which the Zabbix server will run.</p><p>If *zbxserver_configure_vault* is set to false, this user will be used to connect to a PostgreSQL database.</p><p>This user will be created, if it does not already exist.</p> | str | no |  | zabbix |
| zbxserver_group | <p>The primary Linux group of the *zbxserver_user* user.</p><p>This group will be created, if it does not already exist.</p> | str | no |  | zabbix |
| zbxserver_additional_groups | <p>A list of additional Linux groups to which the *zbxserver_user* user will belong.</p><p>On Debian-based systems, this defaults to `ssl-cert`.</p><p>On EL systems, this will not be defined.</p> | list of 'str' | no |  |  |
| zbxserver_firewall_type | <p>The type of host firewall to configure for use with Zabbix server.</p><p>For Debian and EL systems, this defaults to `firewalld`.</p><p>For Ubuntu systems, this defaults to `ufw`.</p> | str | no | <ul><li>firewalld</li><li>ufw</li></ul> |  |
| zbxserver_database_type | <p>The database type of the Zabbix server.</p> | str | no | <ul><li>postgresql</li><li>mysql</li></ul> | postgresql |
| zbxserver_database_name | <p>The name of the database of the Zabbix server.</p> | str | no |  | zabbix-server |
| zbxserver_allow_root | <p>Whether the Zabbix server daemon should run as the root user.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#allowroot</p> | bool | no |  | False |
| zbxserver_allow_software_update_check | <p>Whether the Zabbix server should check for software updates.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#allowsoftwareupdatecheck</p> | bool | no |  | True |
| zbxserver_enable_global_scripts | <p>Whether to enable global scripts for the Zabbix server.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#enableglobalscripts</p> | bool | no |  | True |
| zbxserver_listen_port | <p>The port on which the Zabbix server will listen for agent connections.</p><p>Valid values are between 1024 and 32767.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#listenport</p> | int | no |  | 10051 |
| zbxserver_listen_ip | <p>The IP addresses on which the Zabbix server will listen for incoming connections.</p><p>If not specified, the server will listen on all available IP addresses.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#listenip</p> | list of 'str' | no |  |  |
| zbxserver_source_ip | <p>The IP address to use as the source address for outgoing connections.</p><p>If not specified, the OS TCP/IP stack will decide which address to use.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#sourceip</p> | list of 'str' | no |  |  |
| zbxserver_stats_allowed_ip | <p>The IP addresses from which Zabbix server statistics can be retrieved.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#statsallowedip</p> | list of 'str' | no |  | ['127.0.0.1'] |
| zbxserver_listen_backlog | <p>The maximum length of the queue of pending connections.</p><p>If not specified, the OS default will be used.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#listenbacklog</p> | int | no |  |  |
| zbxserver_service_manager_sync_frequency | <p>The frequency at which the Zabbix server will synchronize with the service manager.</p><p>Valid values are between 1 and 3600.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#servicemanagersyncfrequency</p> | int | no |  | 30 |
| zbxserver_web_service_url | <p>The URL of the Zabbix web service.</p><p>If not specified, the server will not use the web service.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#webservicesurl</p> | str | no |  |  |
| zbxserver_web_driver_url | <p>The URL of the Zabbix web driver.</p><p>If not specified, the server will not use the web driver.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#webdriverurl</p> | str | no |  |  |
| zbxserver_tls_ca_file | <p>The path to the CA certificate file used for validating the server certificate and any client certificate authentication attempts.</p><p>If not specified, certificates will not be validated.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#tlscacertfile</p> | path | no |  |  |
| zbxserver_tls_crl_file | <p>The path to the certificate revocation list file.</p><p>If not specified, certificate revocation lists will not be checked.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#tlscrlfile</p> | path | no |  |  |
| zbxserver_tls_cert_file | <p>The path to the server certificate file.</p><p>If not specified, the server will not use TLS encryption.</p><p>If specified, the *zbxserver_tls_key_file* option must also be specified.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#tlscertfile</p> | path | no |  |  |
| zbxserver_tls_key_file | <p>The path to the server private key file.</p><p>If not specified, the server will not use TLS encryption.</p><p>If specified, the *zbxserver_tls_cert_file* option must also be specified.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#tlskeyfile</p> | path | no |  |  |
| zbxserver_tls_cipher_cert_13 | <p>The list of ciphers in OpenSSL format that the server will accept for TLS 1.3 connections with a certificate.</p><p>If not specified, the server will use the system-wide default ciphers.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#tlsciphercert13</p> | str | no |  |  |
| zbxserver_tls_cipher_cert | <p>The list of ciphers in OpenSSL format that the server will accept for TLS connections with a certificate.</p><p>If not specified, the server will use the system-wide default ciphers.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#tlsciphercert</p> | str | no |  |  |
| zbxserver_tls_cipher_psk_13 | <p>The list of ciphers in OpenSSL format that the server will accept for TLS 1.3 connections with a pre-shared key.</p><p>If not specified, the server will use the system-wide default ciphers.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#tlscipherpsk13</p> | str | no |  |  |
| zbxserver_tls_cipher_psk | <p>The list of ciphers in OpenSSL format that the server will accept for TLS connections with a pre-shared key.</p><p>If not specified, the server will use the system-wide default ciphers.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#tlscipherpsk</p> | str | no |  |  |
| zbxserver_tls_cipher_all_13 | <p>The list of ciphers in OpenSSL format that the server will accept for all TLS 1.3 connections.</p><p>If not specified, the server will use the system-wide default ciphers.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#tlscipherall13</p> | str | no |  |  |
| zbxserver_tls_cipher_all | <p>The list of ciphers in OpenSSL format that the server will accept for all TLS connections.</p><p>If not specified, the server will use the system-wide default ciphers.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#tlscipherall</p> | str | no |  |  |
| zbxserver_ha_node_name | <p>The name of the Zabbix server node in a high-availability cluster.</p><p>This name must be unique within the cluster.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#hanodename</p> | str | no |  |  |
| zbxserver_node_address | <p>The URL segment of the Zabbix server node in a high-availability cluster.</p><p>This should be in a format that can be used to construct a URL (e.g. <address>:<port-number>).</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#nodeaddress</p> | str | no |  |  |
| zbxserver_log_type | <p>The type of logging to use for the Zabbix server.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#logtype</p> | str | no | <ul><li>file</li><li>system</li><li>console</li></ul> | file |
| zbxserver_log_file | <p>The path to the log file for the Zabbix server.</p><p>This option is only used if *zbxserver_log_type* is set to `file`.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#logfile</p> | path | no |  | /var/log/zabbix/zabbix_server.log |
| zbxserver_log_file_size | <p>The maximum size of the log file in megabytes.</p><p>Valid values are between 0 and 1024.</p><p>If set to 0, the log file will not be rotated.</p><p>This option is only used if *zbxserver_log_type* is set to `file`.</p><p>If *zbxserver_manage_logrotate* is set to `true`, this option will default to and should be set to 0 to prevent conflicts.</p><p>If *zbxserver_manage_logrotate* is set to `false`, this option will default to 1.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#logfilesize</p> | int | no |  |  |
| zbxserver_pid_file | <p>The path to the PID file for the Zabbix server.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#pidfile</p> | path | no |  | /run/zabbix/zabbix_server.pid |
| zbxserver_socket_dir | <p>The directory in which the Zabbix server will create its socket file.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#socketdir</p> | path | no |  | /run/zabbix |
| zbxserver_database_host | <p>The host to which the Zabbix server will connect to the database.</p><p>If the database is PostgreSQL, setting this to an empty string will cause the server to connect via a Unix socket on the local system.</p><p>If the database is MySQL, setting this to `localhost` will cause the server to connect via a Unix socket on the local system.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#dbhost</p> | str | no |  | localhost |
| zbxserver_database_socket | <p>The path to the MySQL Unix socket file for accessing the database.</p><p>This is only used if *zbxserver_database_type* is set to `mysql`.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#dbsocket</p> | path | no |  |  |
| zbxserver_database_schema | <p>The schema to use for the Zabbix server database.</p><p>This is only used if *zbxproxy_database_type* is set to `postgresql`.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#dbschema</p> | str | no |  |  |
| zbxserver_database_port | <p>The port on which the Zabbix server will connect to the database.</p><p>Valid values are between 1024 and 65535.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#dbport</p> | int | no |  | 5432 |
| zbxserver_database_password | <p>The password for the Zabbix server to connect to the database.</p><p>If *zbxserver_configure_vault* is set to `true`, this option will be ignored.</p><p>If *zbxserver_configure_vault* is set to `false` and *zbxserver_database_host* is not set to an empty string, this option must be set.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#dbpassword</p> | str | no |  |  |
| zbxserver_allow_unsupported_db_versions | <p>Whether to allow the Zabbix server to connect to unsupported versions of the database.</p><p>This may be needed for newer versions of TimescaleDB.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#allowunsupporteddbversions</p> | bool | no |  | True |
| zbxserver_vault_database_mount_point | <p>The HashiCorp Vault KV2 mount point where the Zabbix server database secret is stored.</p><p>If *zbxserver_configure_vault* is set to `true`, this is required.  Otherwise, it will be ignored.</p> | str | no |  |  |
| zbxserver_vault_database_path | <p>The path within the HashiCorp Vault KV2 mount point where the Zabbix server database secret is stored.</p><p>The secret at this path must have the keys `username` and `password` defined.</p><p>If *zbxserver_configure_vault* is set to `true`, this is required.  Otherwise, it will be ignored.</p> | str | no |  |  |
| zbxserver_db_tls_connect | <p>Defines the TLS behavior for connecting to the database.</p><p>If set to `unencrypted`, the server will not use TLS encryption.</p><p>If set to `require`, the server will use TLS encryption but will not validate the server certificate.</p><p>If set to `verify-ca`, the server will use TLS encryption and will validate the server certificate chain, but not the host name.</p><p>If set to `verify-full`, the server will use TLS encryption and will validate the server certificate chain and host name.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#dbtlsconnect</p> | str | no | <ul><li>unencrypted</li><li>require</li><li>verify_ca</li><li>verify_full</li></ul> | unencrypted |
| zbxserver_db_tls_ca_file | <p>The path to the CA certificate file used for validating the database server certificate.</p><p>If *zbxserver_db_tls_connect* is set to `verify-ca` or `verify-full`, this is required. Otherwise, it will be ignored.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#dbtlscacertfile</p> | path | no |  |  |
| zbxserver_db_tls_cert_file | <p>The path to the client certificate file used for authenticating the Zabbix server to the database server.</p><p>If client certificate authentication is not being used, this option should not be set.</p><p>If *zbxserver_db_tls_key_file* is set, this option must also be set.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#dbtlscertfile</p> | path | no |  |  |
| zbxserver_db_tls_key_file | <p>The path to the client private key file used for authenticating the Zabbix server to the database server.</p><p>If client certificate authentication is not being used, this option should not be set.</p><p>If *zbxserver_db_tls_cert_file* is set, this option must also be set.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#dbtlskeyfile</p> | path | no |  |  |
| zbxserver_db_tls_cipher | <p>The list of ciphers in OpenSSL format that the server will accept for TLS connections to the database.</p><p>If not specified, the server will use the system-wide default ciphers.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#dbtlscipher</p> | str | no |  |  |
| zbxserver_db_tls_cipher_13 | <p>The list of ciphers in OpenSSL format that the server will accept for TLS 1.3 connections to the database.</p><p>If not specified, the server will use the system-wide default ciphers.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#dbtlscipher13</p> | str | no |  |  |
| zbxserver_vault_token | <p>The token used to authenticate to HashiCorp Vault.</p><p>If *zbxserver_configure_vault* is set to `true`, this is required. Otherwise, it will be ignored.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#vaulttoken</p> | str | no |  |  |
| zbxserver_vault_url | <p>The URL of the HashiCorp Vault server.</p><p>If *zbxserver_configure_vault* is set to `true`, this option must be set. Otherwise, it will be ignored.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#vaulturl</p> | str | no |  |  |
| zbxserver_history_storage | <p>Whether to enable history storage in ElasticSearch/OpenSearch.</p> | bool | no |  | False |
| zbxserver_history_storage_url | <p>The URL of the ElasticSearch/OpenSearch server.</p><p>If *zbxserver_history_storage* is set to `true`, this is required.  Otherwise, it will be ignored.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#historystorageurl</p> | str | no |  |  |
| zbxserver_history_storage_types | <p>The index in ElasticSearch/OpenSearch where the Zabbix server will store history data.</p><p>If *zbxserver_history_storage* is set to `true`, this is required. Otherwise, it will be ignored.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#historystoragetypes</p> | list of 'str' | no | <ul><li>uint</li><li>dbl</li><li>str</li><li>log</li><li>text</li></ul> | ['uint', 'dbl', 'str', 'log', 'text'] |
| zbxserver_history_storage_date_index | <p>Whether to create a separate index for each day's history data.</p><p>If *zbxserver_history_storage* is set to `true`, this is required. Otherwise, it will be ignored.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#historystoragedateindex</p> | bool | no |  | False |
| zbxserver_export | <p>Whether to enable export of historical data.</p> | bool | no |  | False |
| zbxserver_export_dir | <p>The directory in which the Zabbix server will store exported historical data.</p><p>If *zbxserver_export* is set to `true`, this is required. Otherwise, it will be ignored.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#exportdir</p> | path | no |  |  |
| zbxserver_export_file_size | <p>The maximum size of the exported data files.</p><p>If *zbxserver_export* is set to `true`, this is required. Otherwise, it will be ignored.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#exportfilesize</p> | bytes | no |  | 1G |
| zbxserver_export_type | <p>The types of data to export.</p><p>If *zbxserver_export* is set to `true`, this is required. Otherwise, it will be ignored.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#exporttype</p> | list of 'str' | no | <ul><li>events</li><li>history</li><li>trends</li></ul> | ['events', 'history', 'trends'] |
| zbxserver_start_pollers | <p>The number of poller processes to start.</p><p>Valid values are between 0 and 1000.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#startpollers</p> | int | no |  | 5 |
| zbxserver_start_agent_pollers | <p>The number of agent poller processes to start.</p><p>Valid values are between 0 and 1000.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#startagentpollers</p> | int | no |  | 1 |
| zbxserver_start_browser_pollers | <p>The number of browser poller processes to start.</p><p>Valid values are between 0 and 1000.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#startbrowserpollers</p> | int | no |  | 1 |
| zbxserver_start_http_agent_pollers | <p>The number of HTTP agent poller processes to start.</p><p>Valid values are between 0 and 1000.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#starthttpagentpollers</p> | int | no |  | 1 |
| zbxserver_start_snmp_pollers | <p>The number of asynchronous SNMP poller processes to start.</p><p>Valid values are between 0 and 1000.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#startsnmppollers</p> | int | no |  | 1 |
| zbxserver_start_ipmi_pollers | <p>The number of IPMI poller processes to start.</p><p>Valid values are between 0 and 1000.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#startipmipollers</p> | int | no |  | 0 |
| zbxserver_start_pollers_unreachable | <p>The number of poller processes to start for unreachable hosts.</p><p>Valid values are between 0 and 1000.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#startpollersunreachable</p> | int | no |  | 1 |
| zbxserver_start_preprocessors | <p>The number of preprocessor processes to start.</p><p>Valid values are between 0 and 1000.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#startpreprocessors</p> | int | no |  | 16 |
| zbxserver_start_history_pollers | <p>The number of history poller processes to start.</p><p>Valid values are between 0 and 1000.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#starthistorypollers</p> | int | no |  | 5 |
| zbxserver_start_trappers | <p>The number of trapper processes to start.</p><p>Valid values are between 0 and 1000.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#starttrappers</p> | int | no |  | 5 |
| zbxserver_start_pingers | <p>The number of pinger processes to start.</p><p>Valid values are between 0 and 1000.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#startpingers</p> | int | no |  | 1 |
| zbxserver_start_discoverers | <p>The number of discoverer processes to start.</p><p>Valid values are between 0 and 1000.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#startdiscoverers</p> | int | no |  | 5 |
| zbxserver_start_http_pollers | <p>The number of HTTP poller processes to start.</p><p>Valid values are between 0 and 1000.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#starthttppollers</p> | int | no |  | 1 |
| zbxserver_start_connecctors | <p>The number of connector processes to start.</p><p>Valid values are between 0 and 1000.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#startconnectors</p> | int | no |  | 0 |
| zbxserver_start_timers | <p>The number of timer processes to start.</p><p>Valid values are between 1 and 1000.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#starttimers</p> | int | no |  | 1 |
| zbxserver_start_escalators | <p>The number of escalator processes to start.</p><p>Valid values are between 1 and 100.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#startescalators</p> | int | no |  | 1 |
| zbxserver_start_alerters | <p>The number of alerter processes to start.</p><p>Valid values are between 1 and 100.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#startalerters</p> | int | no |  | 3 |
| zbxserver_start_java_pollers | <p>The number of Java poller processes to start.</p><p>Valid values are between 0 and 1000.</p><p>If *zbxserver_use_java_gateway* is set to `false`, this is ignored.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#startjavapollers</p> | int | no |  | 0 |
| zbxserver_start_vmware_collectors | <p>The number of VMware collector processes to start.</p><p>Valid values are between 0 and 250.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#startvmwarecollectors</p> | int | no |  | 0 |
| zbxserver_start_snmp_trapper | <p>The number of SNMP trapper processes to start.</p><p>Valid values are between 0 and 1.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#startsnmptrapper</p> | int | no |  | 0 |
| zbxserver_start_db_syncers | <p>The number of database syncer processes to start.</p><p>Valid values are between 1 and 100.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#startdbsyncers</p> | int | no |  | 4 |
| zbxserver_start_proxy_pollers | <p>The number of proxy poller processes to start.</p><p>Valid values are between 0 and 250.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#startproxypollers</p> | int | no |  | 1 |
| zbxserver_start_lld_processors | <p>The number of low-level discovery (LLD) processor processes to start.</p><p>Valid values are between 0 and 100.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#startlldprocessors</p> | int | no |  | 2 |
| zbxserver_start_report_writers | <p>The number of report writer processes to start.</p><p>Valid values are between 0 and 100.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#startreportwriters</p> | int | no |  | 0 |
| zbxserver_start_odbc_pollers | <p>The number of ODBC poller processes to start.</p><p>Valid values are between 0 and 1000.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#startodbcpollers</p> | int | no |  | 1 |
| zbxserver_cache_size | <p>The size of the cache in memory.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#cachesize</p> | bytes | no |  | 32M |
| zbxserver_history_cache_size | <p>The size of the history cache in memory.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#historycachesize</p> | bytes | no |  | 8M |
| zbxserver_history_index_cache_size | <p>The size of the history index cache in memory.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#historyindexcachesize</p> | bytes | no |  | 4M |
| zbxserver_trend_cache_size | <p>The size of the trend cache in memory.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#trendcachesize</p> | bytes | no |  | 4M |
| zbxserver_trend_function_cache_size | <p>The size of the trend function cache in memory.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#trendfunctioncachesize</p> | bytes | no |  | 4M |
| zbxserver_value_cache_size | <p>The size of the value cache in memory.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#valuecachesize</p> | bytes | no |  | 8M |
| zbxserver_vmware_cache_size | <p>The size of the VMware cache in memory.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#vmwarecachesize</p> | bytes | no |  | 8M |
| zbxserver_max_concurrent_checks_per_poller | <p>The maximum number of checks that can be processed concurrently by a poller process.</p><p>Valid values are between 1 and 1000.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#maxconcurrentchecksperpoller</p> | int | no |  | 1000 |
| zbxserver_use_java_gateway | <p>Whether to use the Java Gateway for monitoring.</p> | bool | no |  | False |
| zbxserver_java_gateway | <p>The IP address or hostname of the Java Gateway.</p><p>If *zbxserver_use_java_gateway* is set to `true`, this is required. Otherwise, it will be ignored.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#javagateway</p> | str | no |  |  |
| zbxserver_java_gateway_port | <p>The port on which the Java Gateway will listen for incoming connections.</p><p>Valid values are between 1024 and 32767.</p><p>If *zbxserver_use_java_gateway* is set to `false`, this will be ignored.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#javagatewayport</p> | int | no |  | 10052 |
| zbxserver_vmware_frequency | <p>The frequency in seconds at which the Zabbix server will poll VMware data.</p><p>Valid values are between 10 and 86400.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#vmwarefrequency</p> | int | no |  | 60 |
| zbxserver_vmware_perf_frequency | <p>The frequency in seconds at which the Zabbix server will poll VMware performance data.</p><p>Valid values are between 10 and 86400.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#vmwareperffrequency</p> | int | no |  | 60 |
| zbxserver_vmware_timeout | <p>The timeout in seconds for VMware data collection.</p><p>Valid values are between 1 and 300.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#vmwaretimeout</p> | int | no |  | 10 |
| zbxserver_snmp_trapper_file | <p>The path to the file where SNMP traps will be stored.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#snmptrapperfile</p> | path | no |  | /tmp/zabbix_traps.tmp |
| zbxserver_housekeeping_frequency | <p>The frequency in hours at which the Zabbix server will perform housekeeping tasks.</p><p>Valid values are between 0 and 24.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#housekeepingfrequency</p> | int | no |  | 1 |
| zbxserver_max_housekeeper_delete | <p>The maximum number of records to delete during housekeeping.</p><p>Valid values are between 0 and 1000000.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#maxhousekeeperdelete</p> | int | no |  | 5000 |
| zbxserver_problem_housekeeping_frequency | <p>The frequency in seconds at which the Zabbix server will perform problem housekeeping tasks.</p><p>Valid values are between 1 and 3600.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#problemhousekeepingfrequency</p> | int | no |  | 60 |
| zbxserver_cache_update_frequency | <p>The frequency in seconds at which the Zabbix server will update the cache.</p><p>Valid values are between 1 and 3600.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#cacheupdatefrequency</p> | int | no |  | 10 |
| zbxserver_timeout | <p>The timeout in seconds for processing requests.</p><p>Valid values are between 1 and 30.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#timeout</p> | int | no |  | 3 |
| zbxserver_trapper_timeout | <p>The timeout in seconds for processing trapper requests.</p><p>Valid values are between 1 and 300.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#trappertimeout</p> | int | no |  | 300 |
| zbxserver_unreachable_period | <p>The period with no response in seconds after which a host is considered unavailable.</p><p>Valid values are between 1 and 3600.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#unreachableperiod</p> | int | no |  | 45 |
| zbxserver_unreachable_delay | <p>The period in seconds between checks for hosts that did not respond previously but are not yet considered unavailable.</p><p>Valid values are between 1 and 3600.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#unreachabledelay</p> | int | no |  | 15 |
| zbxserver_unavailable_delay | <p>The period in seconds between checks for hosts that are considered unavailable.</p><p>Valid values are between 1 and 3600.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#unavailabledelay</p> | int | no |  | 60 |
| zbxserver_alert_scripts_path | <p>The path to the directory where alert scripts are stored.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#alertscripts</p> | path | no |  | /usr/lib/zabbix/alertscripts |
| zbxserver_external_scripts | <p>The path to the directory where external scripts are stored.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#externalscripts</p> | path | no |  | /usr/lib/zabbix/externalscripts |
| zbxserver_fping_location | <p>The path to the fping binary.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#fpinglocation</p> | path | no |  | /usr/sbin/fping |
| zbxserver_fping6_location | <p>The path to the fping6 binary.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#fping6location</p> | path | no |  | /usr/sbin/fping6 |
| zbxserver_ssh_key_location | <p>The path to the SSH key used for SSH checks.</p><p>If not specified, the default SSH key will be used.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#sshkeylocation</p> | path | no |  |  |
| zbxserver_tmp_dir | <p>The directory in which the Zabbix server will store temporary files.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#tmpdir</p> | path | no |  | /tmp |
| zbxserver_proxy_config_frequency | <p>The frequency in seconds at which the Zabbix server will synchronize with the proxy configuration.</p><p>Valid values are between 1 and 604800.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#proxyconfigfrequency</p> | int | no |  | 10 |
| zbxserver_proxy_data_frequency | <p>The frequency in seconds at which the Zabbix server will synchronize with the proxy data.</p><p>Valid values are between 1 and 3600.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#proxydatafrequency</p> | int | no |  | 1 |
| zbxserver_sms_devices | <p>A list of SMS modem devices to use for sending notifications.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#smsdevices</p> | list of 'str' | no |  |  |
| zbxserver_include_files | <p>A list of paths to additional configuration files to include in the Zabbix server configuration.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#include</p> | list of 'path' | no |  |  |
| zbxserver_include_directories | <p>A list of directories containing additional configuration files to include in the Zabbix server configuration.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#include</p> | list of 'path' | no |  |  |
| zbxserver_ssl_cert_location | <p>The path to the SSL certificate files to use in web monitoring.</p><p>If not specified, the server will not use SSL client certificates for web monitoring.</p><p>If specified, the *zbxserver_ssl_key_location* option must also be specified.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#sslcertlocation</p> | path | no |  |  |
| zbxserver_ssl_key_location | <p>The path to the SSL key files to use in web monitoring.</p><p>If not specified, the server will not use SSL client certificates for web monitoring.</p><p>If specified, the *zbxserver_ssl_cert_location* option must also be specified.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#sslkeylocation</p> | path | no |  |  |
| zbxserver_load_module_path | <p>The path to the directory containing loadable modules.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#loadmodulepath</p> | path | no |  |  |
| zbxserver_load_modules | <p>A list of loadable modules to load.</p><p>Reference: https://www.zabbix.com/documentation/current/en/manual/appendix/config/zabbix_server#loadmodule</p> | list of 'str' | no |  |  |
| zbxserver_vault_users_mount_point | <p>The HashiCorp Vault KV2 mount point where the Zabbix server user secrets are stored.</p> | str | no |  |  |
| zbxserver_vault_users_base_path | <p>The base path within the HashiCorp Vault KV2 mount point where the Zabbix server user secrets are stored.</p><p>The value should not include a trailing slash.</p> | str | no |  |  |
| zbxserver_user_groups | <p>A list of user groups to create in the Zabbix server.</p> | list of dicts of 'zbxserver_user_groups' options | no |  | [] |
| zbxserver_vault_managed_users | <p>A list of users whose credentials are stored in HashiCorp Vault.</p><p>Each user should be specified as a string.</p> | list of dicts of 'zbxserver_vault_managed_users' options | no |  | [{'username': 'ansible', 'role_name': 'Super admin role'}] |
| zbxserver_unmanaged_users | <p>A list of users that are not managed in HashiCorp Vault.</p> | list of dicts of 'zbxserver_unmanaged_users' options | no |  | [] |
| zbxserver_ldap_directories | <p>A list of LDAP directories to configure for the Zabbix server.</p> | list of dicts of 'zbxserver_ldap_directories' options | no |  | [] |
| zbxserver_saml_directories | <p>A list of SAML directories to configure for the Zabbix server.</p> | list of dicts of 'zbxserver_saml_directories' options | no |  | [] |
| zbxserver_default_auth_type | <p>The default authentication type to use for users.</p> | str | no | <ul><li>internal</li><li>ldap</li></ul> | internal |
| zbxserver_disabled_users_group | <p>The name of the user group to which disabled users should be moved.</p> | str | no |  | Disabled |
| zbxserver_min_password_length | <p>The minimum length of a user's password.</p> | int | no |  | 8 |
| zbxserver_password_complexity | <p>The complexity level of a user's password.</p> | list of 'str' | no | <ul><li>contain_uppercase_and_lowercase_letters</li><li>contain_digits</li><li>contain_special_characters</li><li>avoid_easy_to_guess</li></ul> | ['avoid_easy_to_guess'] |
| zbxserver_jit_provisioning_interval | <p>The interval in seconds at which just-in-time provisioning should be performed.</p> | str | no |  | 1h |
| zbxserver_ldap_enabled | <p>Whether LDAP authentication is enabled.</p><p>If *zbxserver_ldap_directories* is specified, this will default to `true`. Otherwise, it will default to `false`.</p> | bool | no |  |  |
| zbxserver_ldap_jit_status | <p>Whether just-in-time provisioning is enabled for LDAP users.</p> | bool | no |  | False |
| zbxserver_active_ldap_directory | <p>The name of the LDAP directory to use for authentication.</p><p>If *zbxserver_ldap_enabled* is set to `true`, this is required.</p> | str | no |  |  |
| zbxserver_saml_enabled | <p>Whether SAML authentication is enabled.</p><p>If *zbxserver_saml_directories* is specified, this will default to `true`. Otherwise, it will default to `false`.</p> | bool | no |  |  |
| zbxserver_saml_jit_status | <p>Whether just-in-time provisioning is enabled for SAML users.</p> | bool | no |  | False |
| zbxserver_logrotate_period | <p>The period at which logrotate should rotate the Zabbix server's log file.</p> | str | no | <ul><li>daily</li><li>weekly</li><li>monthly</li></ul> | daily |
| zbxserver_logrotate_retention | <p>The number of rotated log files to retain.</p> | int | no |  | 14 |
| zbxserver_logrotate_mode | <p>The permissions to set on the rotated log files.</p> | str | no |  | 0640 |
| zbxserver_templates | <p>A list of templates to import into the Zabbix server from a path on the controller node.</p> | list of 'path' | no |  | [] |

### Options for zbxserver_user_groups
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| name | <p>The name of the user group.</p> | str | yes |  |  |
| gui_access | <p>Whether the user has GUI access and the authentication method to use.</p> | str | no | <ul><li>default</li><li>internal</li><li>LDAP</li><li>disable</li></ul> |  |
| host_group_rights | <p>A list of host group rights for the user group.</p> | list of dicts of 'host_group_rights' options | no |  |  |
| template_group_rights | <p>A list of template group rights for the user group.</p> | list of dicts of 'template_group_rights' options | no |  |  |

### Options for zbxserver_user_groups > host_group_rights
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| host_group | <p>The name of the host group.</p> | str | yes |  |  |
| permission | <p>The permission level for the host group.</p> | str | yes | <ul><li>denied</li><li>read-only</li><li>read-write</li></ul> |  |

### Options for zbxserver_user_groups > template_group_rights
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| template_group | <p>The name of the host group.</p> | str | yes |  |  |
| permission | <p>The permission level for the host group.</p> | str | yes | <ul><li>denied</li><li>read-only</li><li>read-write</li></ul> |  |

### Options for zbxserver_vault_managed_users
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| username | <p>The username of the user.</p> | str | yes |  |  |
| password | <p>The password of the user.</p><p>If not specified, the password will be generated before storing in HashiCorp Vault.</p> | str | no |  |  |
| name | <p>The display name of the user.</p><p>If not specified, the username will be used.</p> | str | no |  |  |
| surname | <p>The surname of the user.</p> | str | no |  |  |
| groups | <p>A list of groups to which the user belongs.</p> | list of 'str' | no |  |  |
| role_name | <p>The name of the role to assign to the user.</p> | str | yes |  |  |
| lang | <p>The language to use for the user.</p> | str | no | <ul><li>en_GB</li><li>en_US</li><li>zh_CN</li><li>cs_CZ</li><li>fr_FR</li><li>he_IL</li><li>it_IT</li><li>ko_KR</li><li>ja_JP</li><li>nb_NO</li><li>pl_PL</li><li>pt_BR</li><li>pt_PT</li><li>ru_RU</li><li>sk_SK</li><li>tr_TR</li><li>uk_UA</li><li>default</li></ul> |  |
| refresh | <p>The period in seconds at which the user's credentials should be refreshed.</p> | int | no |  |  |
| theme | <p>The theme to use for the user.</p> | str | no | <ul><li>default</li><li>blue-theme</li><li>dark-theme</li></ul> | default |
| timezone | <p>The timezone to use for the user.</p> | str | no |  |  |
| user_medias | <p>A list of media entries for the user.</p> | list of dicts of 'user_medias' options | no |  |  |

### Options for zbxserver_vault_managed_users > user_medias
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| active | <p>Whether the media entry is active.</p> | bool | no |  | True |
| mediatype | <p>The type of media.</p> | str | yes |  |  |
| period | <p>The period in seconds at which the media should be checked.</p> | int | no |  |  |
| sendto | <p>The address to which the media should be sent.</p> | raw | yes |  |  |
| severity | <p>The severity levels at which the media should be sent.</p> | dict of 'severity' options | no |  |  |

### Options for zbxserver_vault_managed_users > user_medias > severity
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| not_classified | <p>Whether to send notifications for not classified events.</p> | bool | no |  | True |
| information | <p>Whether to send notifications for information events.</p> | bool | no |  | True |
| warning | <p>Whether to send notifications for information events.</p> | bool | no |  | True |
| average | <p>Whether to send notifications for average events.</p> | bool | no |  | True |
| high | <p>Whether to send notifications for high events.</p> | bool | no |  | True |
| disaster | <p>Whether to send notifications for disaster events.</p> | bool | no |  | True |

### Options for zbxserver_unmanaged_users
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| username | <p>The username of the user.</p> | str | yes |  |  |
| password | <p>The password of the user.</p> | str | yes |  |  |
| name | <p>The display name of the user.</p><p>If not specified, the username will be used.</p> | str | no |  |  |
| surname | <p>The surname of the user.</p> | str | no |  |  |
| groups | <p>A list of groups to which the user belongs.</p> | list of 'str' | no |  |  |
| role_name | <p>The name of the role to assign to the user.</p> | str | yes |  |  |
| lang | <p>The language to use for the user.</p> | str | no | <ul><li>en_GB</li><li>en_US</li><li>zh_CN</li><li>cs_CZ</li><li>fr_FR</li><li>he_IL</li><li>it_IT</li><li>ko_KR</li><li>ja_JP</li><li>nb_NO</li><li>pl_PL</li><li>pt_BR</li><li>pt_PT</li><li>ru_RU</li><li>sk_SK</li><li>tr_TR</li><li>uk_UA</li><li>default</li></ul> |  |
| refresh | <p>The period in seconds at which the user's credentials should be refreshed.</p> | int | no |  |  |
| theme | <p>The theme to use for the user.</p> | str | no | <ul><li>default</li><li>blue-theme</li><li>dark-theme</li></ul> | default |
| timezone | <p>The timezone to use for the user.</p> | str | no |  |  |
| user_medias | <p>A list of media entries for the user.</p> | list of dicts of 'user_medias' options | no |  |  |

### Options for zbxserver_unmanaged_users > user_medias
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| active | <p>Whether the media entry is active.</p> | bool | no |  | True |
| media_type | <p>The type of media.</p> | str | yes |  |  |
| period | <p>The period in seconds at which the media should be checked.</p> | int | no |  |  |
| sendto | <p>The address to which the media should be sent.</p> | raw | yes |  |  |
| severity | <p>The severity levels at which the media should be sent.</p> | dict of 'severity' options | no |  |  |

### Options for zbxserver_unmanaged_users > user_medias > severity
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| not_classified | <p>Whether to send notifications for not classified events.</p> | bool | no |  | True |
| information | <p>Whether to send notifications for information events.</p> | bool | no |  | True |
| warning | <p>Whether to send notifications for information events.</p> | bool | no |  | True |
| average | <p>Whether to send notifications for average events.</p> | bool | no |  | True |
| high | <p>Whether to send notifications for high events.</p> | bool | no |  | True |
| disaster | <p>Whether to send notifications for disaster events.</p> | bool | no |  | True |

### Options for zbxserver_ldap_directories
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| name | <p>The name of the LDAP directory within Zabbix server.</p> | str | yes |  |  |
| host | <p>The hostname or IP address of the LDAP server.</p> | str | yes |  |  |
| encryption | <p>The type of encryption to use for the LDAP connection.</p> | str | no | <ul><li>unencrypted</li><li>ldaps</li><li>starttls</li></ul> | unencrypted |
| port | <p>The port on which the LDAP server is listening.</p><p>If using LDAPS, the default is 636. Otherwise, the default is 389.</p> | int | no |  |  |
| description | <p>A description of the LDAP directory.</p> | str | no |  |  |
| base_dn | <p>The base distinguished name for querying the LDAP directory.</p> | str | yes |  |  |
| filter | <p>The filter to use when authenticating users.</p> | str | no |  |  |
| bind_dn | <p>The distinguished name to use for binding to the LDAP server.</p><p>If anonymous binding is allowed and desired, this should not be set.</p> | str | no |  |  |
| bind_password | <p>The password to use for binding to the LDAP server.</p><p>If anonymous binding is allowed and desired, this should not be set.</p> | str | no |  |  |
| username_attribute | <p>The attribute to use for the username.</p> | str | no |  | sAMAccountName |
| name_attribute | <p>The attribute to use for the user's first name.</p> | str | no |  | displayName |
| last_name_attribute | <p>The attribute to use for the user's last name.</p> | str | no |  | sn |
| email_attribute | <p>The attribute to use for the user's email address.</p> | str | no |  | mail |
| user_group_attribute | <p>The attribute to use for the user's group membership.</p> | str | no |  | memberOf |
| group_mappings | <p>A list of group mappings for the LDAP directory.</p> | list of dicts of 'group_mappings' options | no |  | [] |

### Options for zbxserver_ldap_directories > group_mappings
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| name | <p>The name of the group in the LDAP directory.</p><p>Use '*' to match users that don't match any other group.</p> | str | yes |  |  |
| role | <p>The name of the role to assign to users that match this group.</p> | str | yes |  |  |
| user_groups | <p>A list of Zabbix user groups to assign to users that match this group.</p> | list of 'str' | no |  |  |

### Options for zbxserver_saml_directories
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| name | <p>The name of the LDAP directory within Zabbix server.</p> | str | yes |  |  |
| sp_entity_id | <p>The entity ID of the Zabbix server.</p> | str | yes |  |  |
| idp_entity_id | <p>The entity ID of the identity provider.</p> | str | yes |  |  |
| sso_url | <p>The URL to use for single sign-on.</p> | str | yes |  |  |
| slo_url | <p>The URL to use for single logout.</p> | str | no |  |  |
| encrypt_assertions | <p>Whether to encrypt SAML assertions.</p> | bool | no |  | False |
| encrypt_nameid | <p>Whether to encrypt the NameID.</p> | bool | no |  | False |
| sign_assertions | <p>Whether to sign SAML assertions.</p> | bool | no |  | False |
| sign_authn_requests | <p>Whether to sign SAML AuthN responses.</p> | bool | no |  | False |
| sign_logout_requests | <p>Whether to sign SAML logout requests.</p> | bool | no |  | False |
| sign_logout_responses | <p>Whether to sign SAML logout responses.</p> | bool | no |  | False |
| sign_messages | <p>Whether to sign SAML messages.</p> | bool | no |  | False |
| username_attribute | <p>The attribute to use for the username.</p> | str | no |  | NameID |
| name_format | <p>The format to use for the NameID.</p> | str | no |  | urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified |
| name_attribute | <p>The attribute to use for the user's first name.</p> | str | no |  | DisplayName |
| surname_attribute | <p>The attribute to use for the user's last name.</p> | str | no |  | LastName |
| email_attribute | <p>The attribute to use for the user's email address.</p> | str | no |  | Email |
| user_group_attribute | <p>The attribute to use for the user's group membership.</p> | str | no |  | Groups |
| group_mappings | <p>A list of group mappings for the SAML directory.</p> | list of dicts of 'group_mappings' options | no |  | [] |

### Options for zbxserver_saml_directories > group_mappings
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| name | <p>The name of the group in the SAML directory.</p><p>Use '*' to match users that don't match any other group.</p> | str | yes |  |  |
| role | <p>The name of the role to assign to users that match this group.</p> | str | yes |  |  |
| user_groups | <p>A list of Zabbix user groups to assign to users that match this group.</p> | list of 'str' | no |  |  |


## License
MIT

## Author and Project Information
Jim Tarpley (@trippsc2)
<!-- END_ANSIBLE_DOCS -->
