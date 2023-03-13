local_accounts
=========
# Ansible Role: local_accounts

An Ansible role to create specific users accounts on Linux servers. 

Requirements
------------

- Ansible 2.12 or later
- Python 3.x on the managed hosts (required for Ansible modules)
- 'community.crypto' collection (required for authorized_key module)
- 'ansible.posix' collection (required for ansible.posix.authorized_key module)

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
         - local_accounts

License
-------

MIT

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
