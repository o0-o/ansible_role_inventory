# o0_o.inventory

This Ansible role generates inventory boilerplate on `localhost`.

## Requirements

Only one inventory source should be provided and it should be a directory. If multiple inventory sources are provided, boilerplate is written to the first, which must be a directory. If the inventory source does not exist, it will be created as a directory.

## Role Variables

### Defaults

#### Automatic inventory versioning and/or backup

```yaml
inv_git: true
```

This variable controls whether or not to automatically handle versioning of the inventory with `git`. If set to `true` and a repository doesn't exist in the inventory directory, one will be created for you. `[YUBIKEY PRESS]` will be included at the end of the name of any tasks that commit changes to the inventory repository in case a Yubikey is being used for signing commits and requires a press or PIN entry.

```yaml
inv_bu: false
```

This variable is passed to the `backup` parameter of the Ansible `template`, `blockinfile` and `lineinfile` modules when modifying inventory files. While both `inv_bu` and `inv_git` may be any combination of `true` and `false`, `inv_bu` should be considered an alternative to `inv_git` as enabling both would be redundant.

#### Comment boilerplate

```yaml
default_comment_prefix: "# vim: ts=8:sw=8:sts=8:noet:ft=cfg\n#"
ansible_comment_prefix: "# vim: ts=2:sw=2:sts=2:et:ft=yaml.ansible\n#"
yaml_comment_prefix: "# vim: ts=2:sw=2:sts=2:et:ft=yaml\n#"
yaml_python_prefix: "# vim: ts=4:sw=4:sts=4:et:ft=python\n#"
```

Comment prefixes are applied to the comment header of templated files based on file type. These values are used throughout the `o0_o` Ansible collections and are not limited to the local inventory.

```yaml
default_comment_postfix: "#\n########################################################################"
```

The comment postfix is applied to the comment header of templated files (also not limited to the local inventory). This uses a 72-character width [as defined in PEP8](https://peps.python.org/pep-0008/#maximum-line-length).

```yaml
safe_zone_comment: "# ADD CUSTOM CONFIGURATION BELOW #######################################"
```

The safe zone comment is used to separate inventory files between Ansible-managed blocks and user-managed blocks. Note that many Ansible managed blocks in the inventory are editable, and those are indicated by `(Editable)` at the end of the block's marker string.

#### Operating system defaults

```yaml
arch_defaults:
  archlinux: x86_64
  centos: x86_64
  debian: amd64
  fedora: x86_64
  freebsd: amd64
  openbsd: amd64
  rocky: x86_64
  ubuntu: amd64

repo_defaults:
  archlinux:
    https:
      - geo.mirror.pkgbuild.com
  centos:
    https:
      - mirror.centos.org/centos
  debian:
    https:
      - deb.debian.org/debian
  fedora:
    https:
      - mirrors.fedoraproject.org/fedora
  freebsd:
    https:
      - ftp.FreeBSD.org
  openbsd:
    https:
      - cdn.openbsd.org/pub/OpenBSD
  rocky:
    https:
      - mirrors.rockylinux.org
  ubuntu:
    https:
      - mirrors.edge.kernel.org/ubuntu

release_defaults:
  archlinux: latest
  centos: '7.9.2009'
  debian: '11.4.0'
  fedora: '36'
  freebsd: '13.1'
  openbsd: '7.1'
  ubuntu: 'jammy' #22.04 LTS
```

These are various default values for operating systems explicitly supported by the other roles in the `o0_o` Ansible collections. They are written to the `group_vars/all.yml` inventory file where they can be edited later (edits will persist through subsequent runs of this role).

## Dependencies

None

## Example Playbook

```yaml
- name: Example playbook using the o0_o.inventory role
  hosts: all
  gather_facts: false
  any_errors_fatal: true
  roles:
     - o0_o.inventory
```

## License

MIT

## Author Information

Email: o@o0-o.ooo
