Server Setup
=========

[![CI](https://github.com/scottharwell/ansible-role-server-setup/workflows/CI/badge.svg)](https://github.com/scottharwell/ansible-role-server-setup/actions?query=workflow%3A+CI)

This is a simple role to configure Linux servers with a base set of packages and shell configurations that I prefer.  It will configure a list of users on the remote Linux servers with Fish shell as the default shell, will display NeoFetch on login, will configure Fish with Oh My Fish and the _Bob the Fish_ theme, and will configure VIM with my preferred configured.

Requirements
------------

Ansible >= 2.0

Role Variables
--------------

* `users`: A list of users on the remote server to create or update. This is typically the default user to login to the server and ultimately the `scott` account that I configure for myself.
  ```yaml
  users:
    - username: scott
    home_path: /home/scott
  ```
* `ssh_public_keys`: A list of SSH public keys that I want to deploy to the user accounts.  These are local files copied to the remote server
  ```yaml
  ssh_public_keys:
    - "{{ lookup('file', lookup('env','HOME') + '/.ssh/id_ed25519.pub') }}"
    - "{{ lookup('file', lookup('env','HOME') + '/.ssh/id_rsa.pub') }}"
  ```
* `omf_install_script_url`: The location to the install script for Oh My Fish.
  ```yaml
  omf_install_script_url: "https://get.oh-my.fish"
  ```
* `ultimate_vim_git_url`: URL to the Git repo for Ultimate VIM.
  ```yaml
  omf_install_script_url: "https://github.com/amix/vimrc.git"
  ```

Dependencies
------------

N/A

Example Playbook
----------------

Example playbook that configures my home VMs configured through Vagrant.

```yaml
 - hosts: vagrant_vms
   become: true

   vars:
     ssh_public_keys:
       - "{{ lookup('file', lookup('env','HOME') + '/.ssh/id_ed25519.pub') }}"
       - "{{ lookup('file', lookup('env','HOME') + '/.ssh/id_ed25519_prompt_ios.pub') }}"
     users:
       - username: vagrant
         home_path: /home/vagrant
       - username: scott
         home_path: /home/scott

   roles:
     - role: server-setup

```

License
-------

MIT
