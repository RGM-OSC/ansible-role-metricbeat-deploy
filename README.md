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

| name                         | default value             | usage                                                        | perimeter |
| ---------------------------- | ------------------------- | ------------------------------------------------------------ | --------- |
| `rgm_server`                 |                           | RGM IP or hostname to get install script and use API         | Both      |
| `rgm_username`               |                           | RGM username account allowed to create new hosts             | Both      |
| `rgm_password`               |                           | RGM password account allowed to create new hosts             | Both      |
| `d_rgm_template`             | RGM_LINUX_ES              | RGM template applied to new hosts                            | Linux     |
| `d_rgm_win_template`         | RGM_WINDOWS_ES            | RGM template applied to new windows hosts                    | Windows   |
| `d_rgm_exportjob_name`       | Incremental Nagios Export | RGM export job name used to import new hosts                 | Linux     |
| `no_export_config`           | false                     | Do not export rgm configuration. usefull for larges volumes  | Windows   |
| `script_verbose`             | false                     | Launch script in verbose mode                                | Windows   |
| `forceinstall`               |                           | Force Metricbeat reinstallation                              | Windows   |
| `remove_metricbeat_services` | false                     | Launch only metricbeat service deletion                      | Windows   |
| `rgmapi_version`             | v2                        | Select the api version, v1 is for previous RGM installations | Both      |
| `force_rgm_registration`     |                           | create this variable to force rgm host registration even metricbeat already installed | Linux     |

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
  vars:
    rgm_server: <rgm_servername>
    rgm_username: <rgm_admin_username>
    rgm_password: <rgm_admin_password>

roles:
    - role-metricbeat-deploy
```

Another example for windows to force reinstall (or update metricbeat) but without register again. vars for authentication are not managed here

```yaml
---
- hosts: monitored
  vars:
    forceinstall: true
    no_export_config: true

  roles:
    - role-metricbeat-deploy
```



## License

BSD

## Author Information

Initial write by Vincent Fricou <vincent@fricouv.eu> (2019), release under the terms of BSD licences
Updated by Guillaume AUGEY 2021 & 2023

### Contributions

Oriane SAVIO - Windows deployment parts