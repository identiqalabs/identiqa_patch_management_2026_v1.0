# patch-management-iq

Ansible playbooks for **Windows and Linux patch management** at Identiqa Labs.

> **Status:** No playbooks have been added yet. The structure below is a
> suggested layout to follow as they are added.

## Goals

- Apply OS security and feature updates to Linux and Windows servers from one place
- Run patching in controlled batches, with reboots handled safely
- Report which hosts were patched, which failed, and which need a reboot

## Requirements

- **Control node:** Linux with Ansible 2.14 or newer
- **Linux targets:** SSH access with a sudo-capable user
- **Windows targets:** WinRM (or SSH) enabled, plus the Ansible collections:
  ```bash
  ansible-galaxy collection install ansible.windows community.windows
  ```

## Suggested layout

```
inventory/
  hosts.ini              # [linux] and [windows] groups
  group_vars/
    linux.yml
    windows.yml          # WinRM connection settings
playbooks/
  linux_patch.yml        # apt / dnf updates, then reboot if required
  windows_patch.yml      # ansible.windows.win_updates
  patch_report.yml       # list pending updates without installing
roles/
```

## Usage (once playbooks exist)

```bash
# Dry run: show what would change
ansible-playbook -i inventory/hosts.ini playbooks/linux_patch.yml --check

# Patch Linux servers
ansible-playbook -i inventory/hosts.ini playbooks/linux_patch.yml

# Patch Windows servers
ansible-playbook -i inventory/hosts.ini playbooks/windows_patch.yml
```

Use `--limit <group-or-host>` to patch a subset of hosts.

## Security

Keep credentials (sudo passwords, WinRM passwords) out of the repository. Use
`ansible-vault` or your secrets manager.
