#ansible Collection - cloud.my_collection

This collection contains two roles for setting up local user accounts,group accounts under role (local_accounts), and configuring SSH on a target machine under role (ssh-config).

Usage:

- git clone https://github.com/bluesomania/cloud.ansible.git

- cd cloud.ansible

- git checkout ansible_collectionsv3/ansible_collectionsv2

- cd cloud/my_collection/

- ansible-galaxy collection install cloud-my_collection-1.0.0.tar.gz  


#This will install the cloud.my_collection collection on your local machine.

License

This collection is licensed under the MIT license.

Roles: 

local_accounts
=========
# Ansible Role: local_accounts

An Ansible role to create users accounts and groups with passwordless authentication on Linux servers.

Requirements
------------

- Ansible 2.12 or later
- Python 3.x on the managed hosts (required for Ansible modules)
- 'community.crypto' collection (required for authorized_key module)
- 'ansible.posix' collection (required for ansible.posix.authorized_key module)
- sudo privilages for the specified user which will be performing the tasks on the target machines, these configurations are defined in 'ansible.cfg' as follows:

# (string) Privilege escalation method to use when `become` is enabled.
        become_method=sudo

# (string) The user your login/remote user 'becomes' when using privilege escalation, most systems will use 'root' when no user is specified.
        become_user=root

# (boolean) Toggles the use of privilege escalation, allowing you to 'become' another user after login.
        become=True

# (boolean) Toggle to prompt for privilege escalation password.
        become_ask_pass=False

# (string) Sets the login user for the target machines
# When blank it uses the connection plugin's default, normally the user currently executing Ansible.
        remote_user=ansible
---

Role Variables
--------------
# This role has the following variables that can be set in 'defaults/main.yml'

- local_users: A list of dictionaries variable specifying the local user accounts to create. Each dictionary must contain the following keys:
    name:       #The username of the local account.
    uid:        #The UID of the local account.
    group:      #The primary group of the local account.
    system:     #Boolean value indicating whether the account is a system account.
    expires:    #The expiry date of the account.

- local_groups: A list of dictionaries variable specifying the local user groups to create. Each dictionary must contain the following keys:

    name: local_adm     # The name of the local group to create
    system: yes         # Set to yes to create a system group

Dependencies
------------
these collections are needed for invoking certain modules that are used for this specific role, the collections and their modules are referenced below:
- 'community.crypto' collection (required for authorized_key module)
- 'ansible.posix' collection (required for ansible.posix.authorized_key module)

Example Playbook
----------------

- name: Create users with their  primary groups and add their ssh keys to passwordless authentication.
  hosts: myhosts         #label 'myhosts' which corresponds to a group of hosts defined in the inventory file, the tasks in the playbook will be run on all hosts that are members of the 'myhosts'
  become: true           #'become: true' allows the playbook to execute tasks as a different user or as the root user, depending on the configuration specified in 'ansible.cfg'
  gather_facts: true     #'gather_facts: true' "gather facts" is a built-in mechanism that collects information about the target hosts and saves it as variables that can be used later in the playbook.

  tasks:
    - name: local_accounts role         #current role name which is called local_account this role will be imported to implement it's tasks that is located in 'tasks/main.yml'
      import_role:                      #import_role is an Ansible task module that allows you to use a role from another playbook or collection in your current playbook.
        name: cloud.my_collection.local_accounts        #name: cloud.my_collection.local_accounts specifies the name of the role that is being imported. cloud.my_collection refers to the collection in which the role is located, and local_accounts is the name of the role itself.
---

License
-------

MIT

ssh_config
=========
# Ansible Role: ssh_config

# An Ansible role to configure sshd service on Linux servers.

#the role will ensure the sshd daemon has 3 options configured with the following values:
#PasswordAuthentication: yes
#PermitEmptyPasswords: no
#PermitRootLogin: no

# These options could be changed to other values accordingly in the defaults/main.yml'

Requirements
------------

- Ansible 2.12 or later
- Python 3.x on the managed hosts (required for Ansible modules)
- sudo privilages for the specified user which will be performing the tasks on the target machines, these configurations are defined in 'ansible.cfg' as follows:

# (string) Privilege escalation method to use when `become` is enabled.
        become_method=sudo

# (string) The user your login/remote user 'becomes' when using privilege escalation, most systems will use 'root' when no user is specified.
        become_user=root

# (boolean) Toggles the use of privilege escalation, allowing you to 'become' another user after login.
        become=True

# (boolean) Toggle to prompt for privilege escalation password.
        become_ask_pass=False

# (string) Sets the login user for the target machines
# When blank it uses the connection plugin's default, normally the user currently executing Ansible.
        remote_user=ansible

Role Variables
--------------
# This role has the following variables that can be set in 'defaults/main.yml'

# This is a list of SSHD configuration options which defined as A list of dictionaries variable, to set using the `ansible.builtin.lineinfile` module. Each attribute in the list contains the `name` and `value` of the configuration option that will be set in the `sshd_config_entries`.

sshd_config_entries:

  - name: "PasswordAuthentication"
    value: "yes"
  - name: "PermitEmptyPasswords"
    value: "no"
  - name: "PermitRootLogin"
    value: "no"

# This is the file path of the `sshd_config` file that will be configured with the new configuration options, this is the default file path for configuring sshd service. 

sshd_config_file: "/etc/ssh/sshd_config"
--------

Handlers: 
---------

This handler is defined in  'notify: restart sshd' in the 'task/mail.yml' in the role directory.

Restart SSHD service:

This handler is triggered when any of the tasks in the role make changes to the SSH configuration file.
It restarts the SSH service to ensure that the changes take effect.


Dependencies
------------

None.

Example Playbook
----------------
- name:  #the role will ensure the sshd daemon has 3 options configured with the following values:'PasswordAuthentication: yes','PermitEmptyPasswords: no','PermitRootLogin: no'

  hosts: my_hosts         #label 'my_hosts' which corresponds to a group of hosts defined in the inventory file, the tasks in the playbook will be run on all hosts that are members of the 'my_hosts'
  become: true           #'become: true' allows the playbook to execute tasks as a different user or as the root user, depending on the configuration specified in 'ansible.cfg'
  gather_facts: true     #'gather_facts: true' "gather facts" is a built-in mechanism that collects information about the target hosts and saves it as variables that can be used later in the playbook.

  tasks:
    - name: ssh_config role        #current role name which is called ssh_config this role will be imported to implement it's tasks that is located in 'tasks/main.yml'
      import_role:                      #import_role is an Ansible task module that allows you to use a role from another playbook or collection in your current playbook.
        name: cloud.my_collection.ssh_config        #name: cloud.my_collection.ssh_config specifies the name of the role that is being imported. cloud.my_collection refers to the collection in which the role is located, and ssh_config is the name of the role itself.

License
-------

MIT

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).

