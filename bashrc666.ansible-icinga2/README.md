[![Build Status](https://travis-ci.org/bashrc666/ansible-role-icinga2.svg?branch=master)](https://travis-ci.org/bashrc666/ansible-role-icinga2)

Ansible-Role-Icinga2
=======

This role is compatible by settings var and role with :

 - Influxdb
 - manubulon snmp-plugin
 - Icingaweb2


Install of Icingaweb2

 - Ready to go at http://IP/icingaweb2/

Compatibility
-------------
 - CentOS6
 - CentOS7

Requirements
------------

- Mysql role
```
  ansible-galaxy install geerlingguy.mysql
```
- PHP role
```
  ansible-galaxy install geerlingguy.php
```

For CentOS :
 - Disable selinux or make your own policies
 - if icingaweb2 set date.timezone in php.ini

ansible >= 2.1.2

Role Variables
--------------

```
# [[ Icingaweb2 ]]
    icinga2_icingaweb2: boolean
    icinga2_icinga2influxdb: boolean
    icinga2_icinga_snmp_check: boolean
    icinga2_icinga2influxdb: boolean
    icinga2_database:
      name: icinga2
      user: icinga2
      password: icinga2dbpasswd
      host: localhost
```

Dependencies
------------

### On remote Host

Everything will be installed by roles if it's not installed

Example Playbook
----------------

```
---
- hosts: icinga2
  become: yes
  vars:
# [[ Mysql Config icinga2 ]]
    mysql_root_password: TEST
    icinga2_database:
      name: icinga2
      user: icinga2
      password: icinga2dbpasswd
      host: localhost
# [[ Mysql Config ]]
    mysql_databases:
      - name: "{{ icinga2_database.name }}"
        encoding: utf8
        collation: utf8_general_ci
      - name: icingaweb2
        encoding: utf8
        collation: utf8_general_ci
      - name: director
        encoding: utf8
        collation: utf8_general_ci
    mysql_users:
      - name: "{{ icinga2_database.user }}"
        host: "{{ icinga2_database.host }}"
        password: "{{ icinga2_database.password }}"
        priv: "{{ icinga2_database.name }}.*:ALL"
      - name: icingaweb2
        host: "localhost"
        password: icingaweb2dbpasswd
        priv: "icingaweb2.*:ALL"
      - name: director
        host: "localhost"
        password: directordbpasswd
        priv: "director.*:ALL"
# [[ Icinga2 Config ]]
    icinga2_director: True
    icinga2_icingaweb2_fqdn: localhost
    icinga2_icingaweb2: True
    icinga_snmp_check: True
    icinga2influxdb: True
    icinga2livestatus: False
# [[ PHP Config ]]
    php_packages:
      - php-ldap
      - php-pdo
      - php-mysql
      - php-intl
# [[ Roles Def ]]
  roles:
    -
      role: geerlingguy.mysql
    -
      role: geerlingguy.php
    -
      role: ansible-role-icinga2

```

TODO
----

- Automate icingadirector 
[DOCS (https://github.com/Icinga/icingaweb2-module-director/blob/master/doc/03-Automation.md)]


License
-------

BSD

Author Information
------------------

gotoole/CAPENSIS - 2016
