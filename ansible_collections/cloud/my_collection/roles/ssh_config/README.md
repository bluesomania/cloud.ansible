ssh_config
=========
# Ansible Role: ssh_config

# An Ansible role to configure sshd service on Linux servers.

#the role will ensure the ssh daemon has 3 options configured with the following values:
#PasswordAuthentication: yes
#PermitEmptyPasswords: no
#PermitRootLogin: no

Requirements
------------

- Ansible 2.12 or later
- Python 3.x on the managed hosts (required for Ansible modules)

Role Variables
--------------

None.

Dependencies
------------

None.

Example Playbook
----------------


    - hosts: servers
      become: true
      roles:
         - ssh_config

License
-------

MIT

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).

