RANCID
======

A brief description of the role goes here.

Requirements
------------

SSH pubilc key installed on remote git repository/repositories.
Write access to all git repositories
SSH access to all network devices via `rancid` user account and password

Role Variables
--------------

**rancid_version** Version number in rancid rpm filename.

**rancid_repo_dir** Where the repository will go. (Default: /var/rancid/git)

**rancid_home** Home directory for rancid user. (Default: /var/rancid)

**rancid_git_name** Name used for git commits.

**rancid_git_email** Email used for git commits.

**install_rancid** Whether or not to install rancid. This gets changed via set_fact when `rancid_version` does not match the installed version.

**rancid_list_of_groups** Space delimited list of groups added to `LIST_OF_GROUPS` in `/etc/rancid/rancid.conf`. (Default: firewalls routers switches)

**rancid_notify_groups** For each group in LIST_OF_GROUPS, who gets emailed the diffs. Added to `/etc/aliases`.

**rancid_network_devices** Create one entry per network device witch `name`, `type`, `ip`, `state`, and `group`. Refer to RANCID documentation for appropriate values for `type`.

**rancid_git_remotes** Repository/repositories where changes are pushed

**rancid_ssh_private_key** Multiline variable containing the SSH private key used by the `rancid` account. If this variable is not defined, a key will be created. **This should be stored in an Ansible vault.**

**rancid_ssh_public_key** SSH pubilc key used by the `rancid` user. If `rancid_ssh_private_key` is not defined, a key will be generated.


Dependencies
------------

None.

Example Playbook
-------------------------
    - name: Setup RANCID
      hosts: server
      sudo: yes

      vars:
          rancid_ssh_public_key: 'ssh-rsa foo== rancid'
          rancid_git_name: Rancid
          rancid_git_email: rancid@acme.com

        roles:
          - rancid

License
-------

MIT

