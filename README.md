RANCID Git
======

This installs [`rancid-git`](https://github.com/dotwaffle/rancid-git), a fork of RANCID 2 that supports using Git.

Requirements
------------

SSH pubilc key installed on remote git repository/repositories.
Commit access to all git remotes
SSH access to all network devices via `rancid` user account and password

Role Variables
--------------

| Name              | Default Value       | Description          |
|-------------------|---------------------|----------------------|
| `rancid_version` | 2.3.9-2 | Version number in rancid-git rpm filename. |
| `rancid_repo_dir` | `/var/rancid/git` | Where the repository will go. |
| `rancid_home` | `/var/rancid` | Home directory for rancid user. | 
| `rancid_git_name` | `rancid` | Name used for git commits. |
| `rancid_git_email` | `rancid@acme.com` | Email used for git commits. |
| `install_rancid` | `False` | Whether or not to install rancid. This gets changed via set_fact when `rancid_version` does not match the installed version. |
| `rancid_list_of_groups` | `firewalls routers switches` | Space delimited list of groups added to `LIST_OF_GROUPS` in `/etc/rancid/rancid.conf`. |
| `rancid_notify_groups` | (see `defaults.yml`) | For each group in LIST_OF_GROUPS, who gets emailed the diffs. Added to `/etc/aliases`. |
| `rancid_network_devices` | (see `defaults.yml`) | Create one entry per network device witch `name`, `type`, `ip`, `state`, and `group`. Refer to RANCID documentation for appropriate values for `type`. |
| `rancid_git_remotes` | (see `defaults.yml`) | Repository/repositories where changes are pushed |
| `rancid_ssh_private_key` | (see `defaults.yml`) | Multiline variable containing the SSH private key used by the `rancid` account. If this variable is not defined, a key will be created. **This should be stored in an Ansible vault.** |
| `rancid_ssh_public_key` | (see `defaults.yml`) | SSH pubilc key used by the `rancid` user. If `rancid_ssh_private_key` is not defined, a key will be generated. |


Dependencies
------------

None.

Example Playbook
-------------------------
    - name: Setup RANCID
      hosts: rancid.acme.com
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

