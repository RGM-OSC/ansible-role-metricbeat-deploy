# RGM Metricbeat deploy ansible role

Ansible role to deploy metricbeat monitoring agent from RGM oneliner.

It perform following tasks :

- Install needed packages to perform metricbeat agent installation (lsb_release)
- Deploy metricbeat agent with RGM oneliner script
- Create new hosts into Lilac configuration
- Perform export job to include hosts in nagios

> **WARNING**. from now, rgm authentication variables where renames:
> - `rgm_adm_username` -> `rgm_username`
> - `rgm_adm_password` -> `rgm_password`
> - `rgm_ip` -> `rgm_server`
 

## Requirements

Some of this variables **absolutely need to be defined** to get fully working playbook :

- rgm_username
- rgm_password
- rgm_server

## Role Variables

| name                             | default value             | usage                                                    |
| -------------------------------- | --------------------------| -------------------------------------------------------- |
| ```rgm_server```                 |                           | RGM IP or hostname to get install script and use API     |
| ```rgm_username```               |                           | RGM username account allowed to create new hosts                   |
| ```rgm_password```               |                           | RGM password account allowed to create new hosts                   |
| ```d_rgm_template```             | RGM_LINUX_ES              | RGM template applied to new hosts                        |
| ```d_rgm_win_template```         | RGM_WINDOWS_ES            | RGM template applied to new windows hosts                |
| ```d_rgm_exportjob_name```       | Incremental Nagios Export | RGM export job name used to import new hosts             |
| ```no_export_config```           | no                        | Windows only. do not export rgm configuration. usefull for larges volumes |
| ```script_verbose```             | no                        | Windows only. launch script in verbose mode              |
| ```remove_metricbeat_services``` | no                        | Launch only metricbeat service deletion                  |
| ```rgmapi_version```             | v2                        | Select the api version, v1 is for previous RGM installations |
| ```force_rgm_registration```     |                           | create this variable to force rgm host registration even metricbeat already installed |

## Role tags

| Tag       | Usage                                                                        |
| --------- | ---------------------------------------------------------------------------- |
| configure | Relaunch metricbeat configuration steps (rewrite files) and restart services |

## Recommendations

To improve security usage of this role, we recommend to store all sensitives datas in ansible custom vault.  
All variables such as :

- `rgm_username`
- `rgm_password`
- `rgm_server`

> Note: You could store more other variables in vault such as `d_rgm_template` to centralize variables filling.

## Dependencies

collection: 
- community.general
- ansible.windows

## Example Playbook

```yaml
---
- hosts: monitored
  roles:
    - role-metricbeat-deploy
  vars:
    rgm_server: <rgm_servername>
    rgm_username: <rgm_admin_username>
    rgm_password: <rgm_admin_password>
```

## License

BSD

## Author Information

Initial write by Vincent Fricou <vincent@fricouv.eu> (2019), release under the terms of BSD licences
Updated by Guillaume AUGEY 2021 & 2023

### Contributions

Oriane SAVIO - Windows deployment parts