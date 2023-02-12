# o0_o.inventory

This Ansible role creates, updates and versions inventory files on `localhost`.

The `host_vars` and `group_vars` directories are created in relation to the first value found in `ansible_inventory_sources`. Boilerplate is written for any host or group variables files that don't already exist (see `templates/`).

The `update host inventory variables` handler will update host variables in `host_vars/host.yml`. The specific variables that are updated are defined in the `inv_host_vars` dictionary (see role variables below).

Inventory versioning can by handled by git or the `backup` parameter found in Ansible moduels that change file content. The `inv_git` and `inv_bu` boolean variables (see role variables below) control which behaviors are used, if any.

## Requirements

None

## Role variables

### Defaults

#### Comment boilerplate

```yaml
default_comment_prefix: "# vim: ts=8:sw=8:sts=8:noet:ft=cfg\n#"
ansible_comment_prefix: "# vim: ts=2:sw=2:sts=2:et:ft=yaml.ansible\n#"
yaml_comment_prefix: "# vim: ts=2:sw=2:sts=2:et:ft=yaml\n#"
yaml_python_prefix: "# vim: ts=4:sw=4:sts=4:et:ft=python\n#"
```

Comment prefixes are applied to the comment header of templated files based on file type. These values are used throughout the `o0_o` Ansible collections and are not limited to the local inventory. While it is common to set Vim modelines at the end of a document, they are also supported at the beginning and doing so prevents users from having to scroll to the end of a document to understand the intended formatting.

```yaml
default_comment_postfix: "#\n########################################################################"
```

The comment postfix is applied to the comment header of templated files (also not limited to the local inventory). This uses a 72-character width [as defined in PEP8](https://peps.python.org/pep-0008/#maximum-line-length).

#### Automatic inventory versioning and/or backup

```yaml
inv_git: true
```

This variable controls whether or not to automatically handle versioning of the inventory with `git`. If set to `true` and a repository doesn't exist in the inventory directory, one will be created for you. `[YUBIKEY PRESS]` will be included at the end of the name of any tasks that commit changes to the inventory repository in case a Yubikey is being used for signing commits and requires a press or PIN entry.

```yaml
inv_bu: false
```

This variable is passed to the `backup` parameter of the Ansible `template`, `blockinfile` and `lineinfile` modules when modifying inventory files. While both `inv_bu` and `inv_git` may be any combination of `true` and `false`, `inv_bu` should be considered an alternative to `inv_git` as enabling both would be redundant.

#### Inventory host variables

```yaml
inv_host_vars:
  ansible_host:
  ansible_user:
  ansible_python_interpreter:
  ansible_become_method:
  ansible_port:
  ansible_ssh_private_key_file:
  ansible_connection:
  ansible_network_os:
  ansible_network_cli_ssh_type:
  tz:
  locale:
  mac:
  repos:
    block: true
    title: Repositories
  net_mgr:
    block: true
    title: Network management daemon
```

These variables are proactively written to each host's `host_vars` file. They can be updated with the `update host inventory variables` handler.

### Vars

```yaml
pre_vars: "{{ hostvars[inventory_hostname] }}"
```

The `pre_vars` variable records a snapshot of the host's variables. It is defined in the first task in `tasks/main.yml` unless it is already defined. This can be used later to see if values have changed. It is useful for notifying the `update host inventory variables` handler.

## Dependencies

None

## Example playbook

There is nothing special about running the role. It is included as a dependency for `o0_o.host.connection`, but is not explicitly included or imported into any other `o0_o` roles.

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

## Author information

Email: o@o0-o.ooo
