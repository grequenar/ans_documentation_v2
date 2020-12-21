---
layout: post
title: ans-ibe-linux-r-SELinux
permalink: /linux/ans-ibe-linux-r-SELinux.html 
---

## Repo URL

 [ans-ibe-linux-r-SELinux][ans-ibe-linux-r-SELinux]

[ans-ibe-linux-r-SELinux]: https://github.com/tvallas/ansible-role-selinux

## Description

Ansible role to manage SELinux policy modules.

## Requirements

None.

## Role Variables

#### `selinux_semodules` (optional)

Manages type enforcement policy files. For example:

```yaml
semodule_semodules:
  - name: measurinator_zabbix-agent.te
    module: |
      module measurinator_zabbix-agent 1.2;
      require {
      	type zabbix_agent_t;
      	class process setrlimit;
      }
      require {
      	type mysqld_etc_t;
      	type zabbix_agent_t;
      	class file { open read };
      }
      #============= zabbix_agent_t ==============
      allow zabbix_agent_t self:process setrlimit;
      allow zabbix_agent_t mysqld_etc_t:file { open read };
```

#### `selinux_file_type_enforcement_dest` (optional)

Destination directory for type enforcement files.

#### `selinux_booleans`

Configures SElinux booleans. For example:

```yaml
selinux_booleans:
  - name: daemons_enable_cluster_mode
    state: yes
```

## Dependencies

None.

## Example Playbook
```yaml
    - hosts: servers
      roles:
         - { role: selinux, tags: selinux }
```
