# icinga2downtime

This role add and removes downtimes in icinga2 via [API](https://icinga.com/docs/icinga2/latest/doc/12-icinga2-api/#schedule-downtime).
There is an [Github Issue](https://github.com/ansible/ansible/pull/24131) adding this feature as module, but last action happend back on 2018...
Currently it downtimes Host and all its Services associatid with it.
Removing downtime is done by matching comment and host object.

## Requirements
None so far.

## Variables
There are a bunch of defaults. Only using `dt_icinga`, `dt_user`, `dt_pass` should be sufficient most of the time.

| variable | default | description |
| --- | --- | --- |
| i2d_icinga | None | Icinga2 host to connect to |
| i2d_port | 5665 | Icinga2 port to connect to |
| i2d_user | icinga2 | Username for authentification |
| i2d_pass | icinga2 | Password for authentification |
| i2d_host | "{{ hostvars[inventory_hostname].ansible_host | default(inventory_hostname) }}" | Hostname which should be downtimed. |
| i2d_msg | 'downtimed with ansible-role icinga2downtime' | comment displayed in downtime |
| i2d_author | "{{ lookup('env','USER') }}" | Author name displayed in downtime |
| i2d_from | 'now' | Downtime ranges are set with unixtimestamps. this will be translated by `date` |
| i2d_until | '+2 hours' | Downtime ranges are set with unixtimestamps. this will be translated by `date` |
| i2d_add | no | When set to `yes` adds downtime |
| i2d_remove | no | When set to `yes` removes downtime |

## Dependencies
None so far.

## Example

```
---
- hosts: all
  gather_facts: 'no'
  vars:
    i2d_icinga: icinga2.example.com
    i2d_user: derJD
    i2d_pass: VeryVerySecret!1
  tasks:
    - include_role:
        name: derJD.icinga2downtime
        apply:
          delegate_to: localhost
      vars: { i2d_add: "yes" }

    - [do something fancy]

    - include_role:
        name: derJD.icinga2downtime
        apply:
          delegate_to: localhost
      vars: { i2d_remove: "yes"
```

## License
BSD

## Author Information
[derJD](https://gihub.com/derJD)
