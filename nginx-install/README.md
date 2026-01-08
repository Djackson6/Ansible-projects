# Nginx install & service

## Purpose
Installs and enables Nginx on hosts in the `server1` group and ensures system packages are up-to-date using `dnf`.

## What the playbook does
- Uses privilege escalation
- Updates all packages
- Installs the `nginx` package
- Starts and enables the `nginx` service

## Prerequisites
- Control machine with Ansible installed.
- Target host(s) running an OS that uses `dnf` (Fedora, RHEL 8+/CentOS Stream, etc.).
- SSH access to target host(s) with an account that can escalate to root.
- Inventory must define a host or group named `server1`.

## Example inventory
Create `inventory` with the following contents:
```ini
[server1]
your.server.ip.or.hostname ansible_user=your_ssh_user
```

## Running the playbook
Install and start Nginx:
```bash
ansible-playbook -i inventory playbook.yml
```

## Troubleshooting
- If `dnf` tasks fail, verify the target OS and that the SSH user has privilege escalation.
- For service start issues, check `journalctl -u nginx` on the host.
- For connection issues, confirm SSH connectivity and inventory configuration.
