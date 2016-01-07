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
| `rancid_umask` | `027` | Umask used on files created by RANCID. |
| `rancid_tmpdir` | `/tmp` | Where RANCID keeps temp files. |
| `rancid_acl_sort` | `[undefined]` | Whether or not to sort ACLs. |
| `rancid_nopipe` | `[undefined]` |  |
| `rancid_filter_pwds` | `NO` | How much to filter passwords from the config files. |
| `rancid_nocommstr` | `NO` | Whether or not to remove SNMP community strings. |
| `rancid_maxrounds` | `4` | How many times to retry failed devices.  |
| `rancid_oldtime` | `24` | How many hours befoure complaining about devices that cannot be reached. |
| `rancid_locktime` | `4` | How many hours before complaining a collection group's lockfile is hung. |
| `rancid_par_count` | `5` | Number of devices that can collect simultaneously. |
| `rancid_maildomain` | `[undefined]` | Only needed if you want to adjust the mail delivery settings based on domain. |
| `rancid_htmlmails` | `NO` | Whether to send colorized diffs in email. |
| `rancid_mailheaders` | `Precedence: bulk` | How to mark messages sent by RANCID. |
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

