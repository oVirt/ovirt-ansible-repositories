oVirt Repositories
==================

The `oVirt.repositories` role is used to set the repositories required for
oVirt engine or host installation. By default it copies content of
/etc/yum.repos.d/ to /tmp/repo-backup-{{timestamp}}, so it's easy to undo that operation.

Requirements
------------

 * oVirt Python SDK version 4
 * Ansible version 2.4

Role Variables
--------------

| Name                                       | Default value         |  Description                              |
|--------------------------------------------|-----------------------|-------------------------------------------|
| ovirt_repositories_ovirt_release_rpm       | UNDEF                 | URL of oVirt release package, which contains required repositories configuration. |
| ovirt_repositories_use_subscription_manager| False                 | If true it will use repos from subscription manager and the value of ovirt_repositories_ovirt_release_rpm will be ignored |
| ovirt_repositories_ovirt_version           | 4.2                   | oVirt release version (Supported versions [4.1, 4.2]). Will be used to enable the required repositories in case ovirt_repositories_use_subscription_manager is set. |
| ovirt_repositories_target_host             | engine                | Type of the target machine, which should be one of [engine, host]. This parameter takes effect only in case ovirt_repositories_use_subscription_manager is set to True. |
| ovirt_repositories_rh_username             | UNDEF                 | Username to use for subscription manager. |
| ovirt_repositories_rh_password             | UNDEF                 | Password to use for subscription manager. |
| ovirt_repositories_pool_ids                | UNDEF                 | List of pools ids to subscribe to. |
| ovirt_repositories_repos_backup_path       | /tmp/repo-backup-{{timestamp}} | Directory to backup the original repositories configuration


Dependencies
------------

No.

Example Playbook
----------------

```yaml
---
- name: Setup repositories using oVirt release package
  hosts: localhost

  vars:
    ovirt_repositories_ovirt_release_rpm: http://resources.ovirt.org/pub/yum-repo/ovirt-master-release.rpm

  roles:
    - role: oVirt.repositories

- vars_files:
    # Contains encrypted `username` and `password` variables using ansible-vault
    - passwords.yml

- name: Setup repositories using Subscription Manager
  hosts: localhost

  vars:
    ovirt_repositories_use_subscription_manager: True
    ovirt_repositories_rh_username: "{{ovirt_repositories_rh_username}}"
    ovirt_repositories_rh_password: "{{ovirt_repositories_rh_password}}"
    # The following pool IDs are not valid and should be replaced.
    ovirt_repositories_pool_ids:
      - 0123456789abcdef0123456789abcdef
      - 1123456789abcdef0123456789abcdef

  roles:
    - role: oVirt.repositories
```

License
-------

Apache License 2.0
