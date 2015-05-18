# Ansible Role: MySQL

[![Ansible Galaxy](http://img.shields.io/badge/galaxy-novuso.mysql-000000.svg)](https://galaxy.ansible.com/list#/roles/3817)
[![MIT License](http://img.shields.io/badge/license-MIT-003399.svg)](http://opensource.org/licenses/MIT)
[![Build Status](https://travis-ci.org/novuso/ansible-role-mysql.svg)](https://travis-ci.org/novuso/ansible-role-mysql)

An Ansible role that manages MySQL on Ubuntu 14.04

## Requirements

Requires the MySQLdb Python package on the remote host. For Ubuntu, this is as
easy as installing `python-mysqldb`.

## Role Variables

Ansible variables are listed here along with their default values:

`mysql_ppa_repo` is a list of Ubuntu PPA repositories that are managed for
this role. Each entry in the list may designate:

* **repo** *required* (ppa: is prepended)
* **state** (default is present)
* **update_cache** (default is yes)
* **validate_certs** (default is yes)

By default, no repositories are used:

    mysql_ppa_repo: []

`mysql_packages` is a list of Ubuntu packages that are managed for this role.
Each entry in the list may designate:

* **name** *required*
* **state** (default is present)

By default, `mysql-server` and `python-mysqldb` (required for some tasks) are
used:

    mysql_packages:
    - name: "mysql-server"
    - name: "python-mysqldb"

`mysql_remove_anon_user` flags whether or not the anonymous user should be
removed.

    mysql_remove_anon_user: true

`mysql_remove_test_db` flags whether or not the test database should be
removed.

    mysql_remove_test_db: true

`mysql_root_password` sets the root password. This value should be kept a
secret. Use a variables file that is not kept in source control, or use
[Ansible Vault](http://docs.ansible.com/playbooks_vault.html).

    mysql_root_password: "root"

`mysql_databases` is a list of database configurations. Each entry in the list
may designate:

* **name** *required*
* **state** (default is present)
* **collation** (default is "utf8_general_ci")
* **encoding** (default is "utf8")

By default, no databases are created:

    mysql_databases: []

`mysql_users` is a list of user configurations. The password values should be
kept secret. Use a variables file that is not kept in source control, or use
[Ansible Vault](http://docs.ansible.com/playbooks_vault.html).
Each entry in the list may designate:

* **name** *required*
* **password** *required*
* **state** (default is present)
* **host** (default is "127.0.0.1")
* **priv** (default is "{{ name }}.*:ALL")

By default, no users are created:

    mysql_users: []

## Dependencies

None

## Example Playbook

**NOTE**: *Do not keep passwords in plain-text in a playbook*. Use
[Vault](http://docs.ansible.com/playbooks_vault.html) or another strategy to
keep your application secure.

    ---
    - hosts: all
      vars:
      - mysql_root_password: "secret"
      - mysql_databases:
        - name: "mydatabase"
      - mysql_users:
        - name: "myuser"
          password: "pass"
          priv: "mydatabase.*:ALL"
      roles:
      - novuso.mysql

## License

This is released under the [MIT license](http://opensource.org/licenses/MIT).
