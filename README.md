![Lab Status](https://img.shields.io/badge/status-In_Progress-yellow)

# Ansible SSH Lab: Automating Fedora from macOS  
Real-world Ansible learning by automating remote SSH tasks across systems

## Table of Contents
- [Objective](#objective)
- [Environment](#environment)
- [Repository Structure](#repository-structure)
- [SSH Recap](#ssh-recap)
- [Inventory & Configuration](#inventory--configuration)
- [First Ping Test](#first-ping-test)
- [Common Errors & Fixes](#common-errors--fixes)
- [Next Steps](#next-steps)
- [Conclusion](#conclusion)

---

## Objective

Learn Ansible by setting up automated SSH control from a macOS client to a Fedora Linux server — without using passwords. This project documents the full path, errors, solutions, and insights from a cloud security engineer in training.

## Environment

| Role   | Hostname     | IP Address     | OS          | Purpose                            |
|--------|--------------|----------------|-------------|-------------------------------------|
| Client | MacBook Air  | 192.168.1.198  | macOS 15.4  | Runs Ansible and controls Fedora    |
| Server | Fedora Ops   | 192.168.1.8    | Fedora 41   | Target host, configured with SSH key |

> Note: SSH key authentication is already configured from Phase 1 (`ssh-key-auth-lab`).

---

## Repository Structure
ansible-ssh-lab/
├── hosts.ini
├── ansible.cfg
└── README.md

---

## SSH Recap

Before using Ansible, an SSH key-based connection from MacBook to Fedora was set up in Phase 1.  
Connection verified using:
```bash
ssh -i ~/.ssh/id_ed25519 ops-admin@192.168.1.8
```

## Inventory & Configuration

hosts.ini
```
[lab]
192.168.1.8 ansible_user=ops-admin ansible_ssh_private_key_file=~/.ssh/id_ed25519
```
ansible.cfg
```
[defaults]
inventory = hosts.ini
host_key_checking = False
```

## First Ping Test

Ran the following from macOS terminal:
```
ansible lab -m ping
```
Output:
```
192.168.1.8 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```

## Common Errors & Fixes

  •	Permission denied (publickey)
Cause: Wrong path to key or wrong user
Fix: Verified file permissions and SSH config
	•	Operation timed out
Cause: Fedora firewall blocking port 22
Fix: Opened SSH port and restarted sshd

## Next Steps

  •	Create first Ansible playbook to install a package remotely
	•	Automate SELinux and firewall hardening tasks
	•	Document Phase 3: Security automation with playbooks

## Conclusion

Ansible was not just installed, it was learned through broken connections, SSH failures, firewall reconfigurations, and direct system control.
This lab marks the transition from “knowing what Ansible is” to using it with real infrastructure.

