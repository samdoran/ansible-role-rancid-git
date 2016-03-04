RANCID Git
======
[![Galaxy](https://img.shields.io/badge/galaxy-samdoran.rancid--git-blue.svg?style=flat)](https://galaxy.ansible.com/samdoran/rancid-git)

This role installs [`rancid-git`](https://github.com/dotwaffle/rancid-git), a fork of RANCID 2 that supports using Git.

Requirements
------------

SSH access to all network devices via the `rancid` user account and password is required. Make sure the target device permits the `rancid` host in any ACLs that protect the management plane.

If you wish you have `rancid` push to any remote repositories, you also need the SSH public key installed on any remote git repository/repositories as well as commit access to all git remotes.

Role Variables
--------------

This role will not work "out of the box" &mdash; you will need to define `rancid_network_devices` and `rancid_clogin` at a minimum. If you wish to have `rancid` push to remotes or send emails, you should also define `rancid_git_remotes` as well as `rancid_notify_groups`.

Defining values in `rancid_clogin` can be rather tricky if you use different usernames and/or passwords for your devices (which isn't a bad idea). One way to solve this is to create a dictionary that stores the passwords (which should be an [Ansible Vault](http://docs.ansible.com/ansible/playbooks_vault.html)):

```yaml
# This file is encrypted with ansible-vault
vault_rancid_passwords:
  core-router-01:
    password: "all-your-device-are-belong-to-us"
    enable: "i-am-not-an-enabler"

  core-switch-01:
    password: "BigGnarlyPassword"
    enable: "enable-babel-fo-fabel"

    ...
```

And reference those in `rancid_clogin`:

```yaml
# This is a host_var, or vars/main.yaml, etc.
rancid_clogin:
  - directive: user
    glob: "*"
    values:
      - rancid

  - directive: password
    glob: "core-router-01"
    values:
      - "{{ vault_rancid_passwords['core-router-01']['password'] }}"
      - "{{ vault_rancid_passwords['core-router-01']['enable'] }}"

  - directive: password
    glob: "core-switch-01"
    values:
      - "{{ vault_rancid_passwords['core-switch-01']['password'] }}"
      - "{{ vault_rancid_passwords['core-switch-01']['enable'] }}"
```


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
| `rancid_list_of_groups` | `firewalls routers switches` | Space delimited list of groups added to `LIST_OF_GROUPS` in `/etc/rancid/rancid.conf`. This is autmatically generated from `rancid_network_devices` |
| `rancid_notify_groups` | (see `defaults/main.yml`) | For each group in LIST_OF_GROUPS, who gets emailed the diffs. Added to `/etc/aliases`. |
| `rancid_network_devices` | (see `defaults/main.yml`) | Create one `key` per device group, and one entry per network device witch `name`, `type`, `ip`, and `state`. Refer to RANCID documentation for appropriate values for `type`. |
| `rancid_clogin` | (see `defaults/main.yml`) | Create entries in the `.cloginrc` file. |
| `rancid_git_remotes` | `[undefined]` | Repository/repositories where changes are pushed. See `defaults/main.yml` for details. |
| `rancid_ssh_private_key` | (see `defaults/main.yml`) | Multiline variable containing the SSH private key used by the `rancid` account. If this variable is not defined, a key will be created. **This should be stored in an Ansible vault.** |
| `rancid_ssh_public_key` | (see `defaults/main.yml`) | SSH pubilc key used by the `rancid` user. If `rancid_ssh_private_key` is not defined, a key will be generated. |
| `rancid_configure_postfix` | `False` | Whether to configure postfix. Only use if email notifications are desired and email _is not_ configured with another role. |
| `rancid_relayhost` | `[undefined]` | IP or FQDN of the email relay host. |


Dependencies
------------

None.

Example Playbook
-------------------------
    - name: Setup RANCID
      hosts: rancid.acme.com
      sudo: yes

      vars:
          rancid_ssh_public_key: 'ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDhv9b... rancid'
          rancid_git_name: Rancid
          rancid_git_email: rancid@acme.com

        roles:
          - rancid

License
-------

MIT

