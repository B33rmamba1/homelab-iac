# homelab-iac

Infrastructure as Code for CareFortress homelab using Ansible and Jenkins.

## Overview
Ansible control node running on VM 104 (carefortress-iac, 192.168.1.140).
Manages three KVM guest VMs running inside the CareFortress hypervisor platform.

## Managed Hosts
- medical-vm (10.10.1.177)
- mgmt-vm (10.10.2.161)
- audit-vm (10.10.3.30)

## Playbooks
- `playbooks/provision.yml` — DNS, timezone, package installation, UFW hardening, sshd config
- `playbooks/patch.yml` — apt cache update, package upgrades, reboot handler

## CI/CD
Jenkins runs in Docker on VM 104, bound to localhost only.
Pipeline pulls from this repo and executes both playbooks against all guest VMs.
Access Jenkins UI via SSH tunnel: `ssh -L 8080:127.0.0.1:8080 b33rmamba@192.168.1.140 -N`
