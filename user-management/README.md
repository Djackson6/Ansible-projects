# Create sudo user

## Purpose
- Creates a `devuser` account
- Ensures the `sudo` group exists
- Configures passwordless sudo for that user
- The playbook validates edits to `/etc/sudoers` with `visudo` to avoid corrupting the file.

## What the playbook does
- Runs on `hosts: server1`.
- Uses `become: yes` for privileged operations.
- Ensures the `sudo` group exists.
- Ensures the user `devuser` is present with:
  - shell: `/bin/bash`
  - home: `/home/devuser`
  - group membership: `sudo`
- Adds a line to `/etc/sudoers`: `devuser ALL=(ALL) NOPASSWD: ALL` using `lineinfile`.
- Validates `/etc/sudoers` edits with `validate: 'visudo -cf %s'` to prevent lockout.

## Security note
Modifying `/etc/sudoers` can lock out administrative access if done incorrectly. This playbook uses `visudo` validation; do not remove `validate`. Keep private keys and secrets out of the repository — use Ansible Vault for sensitive data.

## Prerequisites
- Ansible on the control machine.
- Target host(s) reachable via SSH and able to run privileged commands.
- Inventory must define `server1`.

## Example inventory
Create `inventory`:
```ini
[server1]
your.server.ip.or.hostname ansible_user=your_ssh_user
```

## Running the playbook
Create the `devuser` and grant passwordless sudo:
```bash
ansible-playbook -i inventory user-playbook.yml
```


## Troubleshooting
- If the sudoers validation fails, run `visudo -cf /etc/sudoers` manually to see syntax errors.
- Ensure the SSH user specified in the inventory can run privileged commands.
