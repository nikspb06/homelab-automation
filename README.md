cat > README.md <<'EOF'
# HomeLab Automation

A learning project for automating and managing my home server using Ansible.

## Hardware and OS

- **Server:** Rock Pi 4A
- **OS:** Armbian (Debian-based)
- **Automation:** Ansible
- **Version control:** Git

## Current features

- SSH connectivity to the server through Ansible
- Automatic system facts gathering
- Basic server inspection (OS, memory, CPU architecture)

## Project structure

```text
homelab-automation/
├── inventory/
│   └── hosts.example.ini
├── playbooks/
│   └── inspect.yml
├── .gitignore
└── README.md

## Planned improvements

- Automate package updates and installation
- Manage directories and configuration with Ansible
- Deploy a service using Docker Compose
- Document deployment and testing procedures
## Goal
Learn Linux administration, Ansible automation, Git, and container deployment through a practical home lab.
EOF
