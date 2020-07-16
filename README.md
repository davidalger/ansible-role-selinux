# Ansible Role: SELinux

[![Build Status](https://travis-ci.com/davidalger/ansible-role-selinux.svg?branch=master)](https://travis-ci.com/davidalger/ansible-role-selinux)

Configures SELinux on RHEL and CentOS 7/8. Currently supports setting booleans, file context mappings, port types, and linux user mappings.

## Requirements

None.

## Role Variables

See `defaults/main.yml` for complete list of available variables.

## Dependencies

None.

## Example Playbook

    - hosts: web-servers
      vars:

        selinux_booleans:
          - { name: httpd_can_sendmail, state: yes, persistent: yes }

        selinux_fcontexts:
          - target: /var/www/html/shared(/.*)?
            setype: httpd_sys_rw_content_t
            state: present

        selinux_ports:
          - { ports: 6380, proto: tcp, setype: redis_port_t, state: absent }
          - { ports: 2223, proto: tcp, setype: ssh_port_t, state: present }

        selinux_logins:
          - { login: linuxuser, seuser: staff_u, state: absent }
          - { login: linuxuser, seuser: staff_u, serange: s0-s0:c0.c1023, state: present }

      roles:
        - { role: davidalger.selinux, tags: selinux }

## License

This work is licensed under the MIT license. See LICENSE file for details.

## Author Information

This role was created in 2020 by [David Alger](https://davidalger.com/).
