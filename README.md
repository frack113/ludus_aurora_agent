# Ansible Role: AURORA free EDR Deployment

An Ansible Role that installs [Aurora Agent](https://www.nextron-systems.com/aurora/) on Windows. Optionally sends logs to a SIEM via UDP or integrates with Splunk Universal Forwarder.

> [!WARNING]
> You must have a package and a valid license.
> Check https://www.nextron-systems.com/aurora/

## Add Ludus Role

The role can be installed from Galaxy:

`ludus ansible role add frack113.ludus_aurora_agent`

## Requirements

Before running this role, your Aurora package and license file must be copied to the `files` folder **of the Ludus host**:
- aurora-agent.lic
- aurora-agent.zip

Specifically this files folder:

```
/opt/ludus/users/<your-ludus-username>/.ansible/roles/frack113.ludus_aurora_agent/files
```

If using this role with Splunk, the [Aurora TA add-on](https://github.com/NextronSystems/TA-aurora) must be installed on the Splunk server to consume the EDR logs. It is also recommended to add `SHOULD_LINEMERGE = false` to the `nextron:aurora:edr` sourcetype to avoid parsing errors.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

```yaml
# Install the Aurora dashboard
ludus_aurora_dashboard: false

# Send logs to a SIEM via UDP (e.g., '192.168.1.100:514')
ludus_aurora_udp_target: ''
# Format for UDP logs (e.g., 'syslog')
ludus_aurora_udp_format: ''

# Aurora configuration preset (standard, minimal, reduced, or intense)
ludus_aurora_config_preset: 'standard'

# Configure Splunk Universal Forwarder integration
ludus_aurora_splunk_forwarder: false

# Enable JSON logging to file
ludus_aurora_json_logging: false

# Aurora Agent installation path
ludus_aurora_install_path: 'C:\Program Files\Aurora-Agent'

# Path for JSON log output (used when ludus_aurora_json_logging is true)
ludus_aurora_log_path: 'C:\Program Files\Aurora-Agent\aurora_alerts.json.log'
```

More information here: https://aurora-agent-manual.nextron-systems.com/en/latest/

## Dependencies

None.

## Example Playbook

With dashboard and SIEM integration:

```yaml
- hosts: aurora-agent
  roles:
    - frack113.ludus_aurora_agent
  vars:
    ludus_aurora_dashboard: true
    ludus_aurora_udp_target: '192.168.1.100:514'
    ludus_aurora_udp_format: 'syslog'
```

## Example Ludus Range Configs

Basic configuration:

```yaml
ludus:
  - vm_name: "{{ range_id }}-aurora"
    hostname: "{{ range_id }}-aurora"
    template: win11-22h2-x64-enterprise-template
    vlan: 99
    ip_last_octet: 2
    ram_gb: 8
    cpus: 4
    roles:
      - frack113.ludus_aurora_agent
```

With custom configuration:

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
    role_vars:
      ludus_aurora_dashboard: true
      ludus_aurora_config_preset: 'intense'
      ludus_aurora_splunk_forwarder: true
      ludus_aurora_json_logging: true
```

## License

GPLv3

## Author Information

This role was created by [frack113](https://github.com/frack113), for [Ludus](https://ludus.cloud/).
