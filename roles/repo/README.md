<!-- BEGIN_ANSIBLE_DOCS -->

# Ansible Role: trippsc2.zabbix.repo
Version: 1.1.17

This role installs the Zabbix official repository on a Linux system.

## Requirements

| Platform | Versions |
| -------- | -------- |
| Debian | <ul><li>bookworm</li></ul> |
| EL | <ul><li>9</li><li>8</li></ul> |
| Ubuntu | <ul><li>noble</li><li>jammy</li></ul> |

## Dependencies

None.

## Role Arguments
|Option|Description|Type|Required|Choices|Default|
|---|---|---|---|---|---|
| zbx_major_version | <p>The major version of Zabbix to be deployed.</p> | str | yes | <ul><li>7.4</li><li>7.2</li><li>7.0</li></ul> |  |


## License
MIT

## Author and Project Information
Jim Tarpley (@trippsc2)
<!-- END_ANSIBLE_DOCS -->
