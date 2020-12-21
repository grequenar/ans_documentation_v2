---
layout: post
title: ans-ibe-sql-r-mariadb
permalink: /linux/ans-ibe-sql-r-mariadb.html 
---

## Repo URL

 [ans-ibe-linux-r-mariadb][ans-ibe-linux-r-mariadb]

[ans-ibe-linux-r-mariadb]: https://github.com/bertvv/ansible-role-mariadb

## Description

An Ansible role for managing MariaDB in RedHat-based distributions. Specifically, the responsibilities of this role are to:

- Install MariaDB packages from the official MariaDB repositories
- Remove unsafe defaults:
    - set database root password (remark that once set, this role is unable to *change* the database root password)
    - remove anonymous users
    - remove test database
- Create users and databases
- Manage configuration files `server.cnf` and `custom.cnf`

## Requirements

None.

## Role Variables

None of the variables below are required. When not defined by the user, the [default values](defaults/main.yml) are used.

### Basic configuration

| Variable                       | Default         | Comments                                                                                                     |
| :---                           | :---            | :---                                                                                                         |
| `mariadb_bind_address`         | '127.0.0.1'     | Set this to the IP address of the network interface to listen on, or '0.0.0.0' to listen on all interfaces.  |
| `mariadb_configure_swappiness` | true            | When `true`, this role will set the "swappiness" value (see `mariadb_swappiness`.                            |
| `mariadb_custom_cnf`           | {}              | Dictionary with custom configuration.                                                                        |
| `mariadb_databases`            | []              | List of dicts specifying the databases to be added. See below for details.                                   |
| `mariadb_mirror`               | yum.mariadb.org | Download mirror for the .rpm package (1)                                                                     |
| `mariadb_port`                 | 3306            | The port number used to listen to client requests                                                            |
| `mariadb_root_password`        | ''              | The MariaDB root password. (2)                                                                               |
| `mariadb_server_cnf`           | {}              | Dictionary with server configuration.                                                                        |
| `mariadb_service`              | mariadb         | Name of the service (should e.g. be 'mysql' on CentOS for MariaDB 5.5)                                       |
| `mariadb_swappiness`           | '0'             | "Swappiness" value (string). System default is 60. A value of 0 means that swapping out processes is avoided.|
| `mariadb_users`                | []              | List of dicts specifying the users to be added. See below for details.                                       |
| `mariadb_version`              | '10.4'          | The version of MariaDB to be installed. Default is the current stable release.                               |

#### Remarks

(1) Installing MariaDB from the default yum repository can be very slow (some users reported more than 10 minutes). The variable `mariadb_mirror` allows you to specify a custom download mirror closer to your geographical location that may speed up the installation process. E.g.:

```yaml
mariadb_mirror: 'mariadb.mirror.nucleus.be/yum'
```

(2) **It is highly recommended to set the database root password!** Leaving the password empty is a serious security risk. The role will issue a warning if the variable was not set.

### Server configuration

You can specify the configuration in `/etc/my.cnf.d/server.cnf`, specifically in the `[mariadb]` section, by providing a dictionary of keys/values in the variable `mariadb_server_cnf`. Please refer to the [MariaDB Server System Variables documentation](https://mariadb.com/kb/en/mariadb/server-system-variables/) for details on the possible settings.

For settings that don't get a `= value` in the config file, leave the value empty. All values should be given as strings, so numerical values should be quoted.

 In the following example, `slow-query-log`'s value is left empty:

```yaml
mariadb_server_cnf:
  slow-query-log:
  slow-query-log-file: 'mariadb-slow.log'
  long-query-time: '5.0'
```

This would result in the following `server.cnf`:

```ini
[mariadb]
slow-query-log
slow-query-log-file = mariadb-slow.log
```

### Custom configuration

Settings for other sections than `[mariadb]`, can be set with `mariadb_custom_cnf`. These settings will be written to `/etc/mysql/my.cnf.d/custom.cnf`.

Just like `mariadb_server_cnf`, the variable `mariadb_custom_cnf` should be a dictionary. Keys are section names and values are dictionaries with key-value mappings for individual settings.

The following example enables the general query log:

```yaml
mariadb_custom_cnf:
  mysqld:
    general-log:
    general-log-file: queries.log
    log-output: file
```

The resulting config file will look like this:

```ini
[mysqld]
general-log-file=queries.log
general-log
log-output=file
```

### Adding databases

Databases are defined with a dict containing the fields `name:` (required), and `init_script:` (optional). The init script is a SQL file that is executed when the database is created to initialise tables and populate it with values.

```yaml
mariadb_databases:
  - name: appdb1
  - name: appdb2
    init_script: files/init_appdb2.sql
```

### Adding users

Users are defined with a dict containing fields `name:`, `password:`, `priv:`, and, optionally, `host:`, and `append_privs`. The password is in plain text and `priv:` specifies the privileges for this user as described in the [Ansible documentation](https://docs.ansible.com/ansible/latest/collections/community/mysql/mysql_user_module.html).

An example:

```yaml
mariadb_users:
  - name: john
    password: letmein
    priv: '*.*:ALL,GRANT'
  - name: jack
    password: sekrit
    priv: 'jacksdb.*:ALL'
    append_privs: 'yes'
    host: '192.168.56.%'
```

## Dependencies

None.

## Example Playbook

```yaml
  - name: Molecule test playbook
    hosts: all
    vars:
      mariadb_mirror: 'mariadb.mirror.nucleus.be/yum'
      mariadb_bind_address: '0.0.0.0'
      mariadb_root_password: 'atOrryag&'
      mariadb_server_cnf:
        general-log:
        general-log-file: 'queries.log'
        slow-query-log:
        slow-query-log-file: 'mariadb-slow.log'
        long-query-time: '5.0'
      mariadb_custom_cnf:
        mariadb:
          autoset_open_files_limit:
          max-connections: '20'
        mysqld:
          language: /usr/share/mysql/dutch
      mariadb_configure_swappiness: true
      mariadb_swappiness: '10'
      mariadb_databases:
        - name: emptydb
        - name: app1db
          init_script: ../common/init.sql
        - name: app2db
          init_script: ../common/init.sql
      mariadb_users:
        - name: app1usr
          password: 'Ej{Quiv3'
          priv: 'app1db.*:ALL'
          host: '172.17.%'
        - name: app2usr
          password: 'tytsHeab=3'
          priv: 'app2db.*:ALL'
          host: '172.17.%'
    roles:
      - role: bertvv.mariadb
```

