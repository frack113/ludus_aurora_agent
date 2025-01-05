# Ansible Role: AURORA free EDR Deployment

An Ansible Role that installs [Aurora Agent](https://www.nextron-systems.com/aurora/) on windows .

> [!WARNING]
> you must have a package and a valid license
> Check https://www.nextron-systems.com/aurora/

## Requirements

In the `files` folder:
- aurora-agent.lic
- aurora-agent.zip

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    # If you want to install the dashboard
    ludus_aurora_dashboard: false

    # If you want to send log to a SIEM by UDP 
    ludus_aurora_udp_target: ''
    ludus_aurora_udp_format: ''

More information here https://aurora-agent-manual.nextron-systems.com/en/latest/

## Dependencies

None.

## Example Playbook

```yaml
- hosts: aurora-agent
  roles:
    - frack113.ludus_aurora_agent
  vars:
    ludus_aurora_dashboard: true
```

## Example Ludus Range Config

```yaml
ludus:
  - vm_name: "{{ range_id }}-aurora"
    hostname: "{{ range_id }}-aurora"
    template: win11-22h2-x64-enterprise-template
    vlan: 99
    ip_last_octet: 2
    ram_gb: 8
    cpus: 4
    windows:
      install_additional_tools: true
    roles:
      - frack113.ludus_aurora_agent
```

## License

[//]: # (If you change the License type, be sure to change the actual LICENSE file as well)
GPLv3

## Author Information

This role was created by [frack113](https://github.com/frack113), for [Ludus](https://ludus.cloud/).
