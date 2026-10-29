# Homelab Automation

Automation of a home server infrastructure built around a Rock Pi 4A using Ansible.

This project automates server configuration, package installation, Docker setup, and deployment of an Nginx web server. GitHub Actions is used to automatically check Ansible playbook syntax.

## Tech Stack

- **Ansible** — infrastructure configuration and automation
- **Docker** — container runtime
- **Docker Compose** — containerized service management
- **Nginx** — web server
- **GitHub Actions** — continuous integration
- **Armbian / Debian 13 Trixie** — server operating system

## Project Structure

```text
homelab-automation/
├── ansible.cfg
├── inventory/
│   └── hosts.example.ini
├── playbooks/
│   ├── inspect.yml
│   ├── packages.yml
│   ├── docker.yml
│   └── web.yml
├── roles/
│   └── web/
│       └── tasks/
│           └── main.yml
├── services/
│   └── web/
│       └── compose.yml
└── .github/
    └── workflows/
        └── ansible-check.yml
```

## Prerequisites

- A Linux control machine with Ansible installed
- A Rock Pi 4A running Armbian or Debian
- SSH access to the server
- A user with `sudo` privileges
- Docker and the required Ansible collection for managing Compose services

## Setup

### 1. Install Ansible

On an Ubuntu or Debian control machine:

```bash
sudo apt update
sudo apt install ansible
```

### 2. Configure the inventory

Create a local inventory file from the example:

```bash
cp inventory/hosts.example.ini inventory/hosts.ini
```

Edit `inventory/hosts.ini` and configure the server address, SSH user, and other required connection settings.

The local inventory is excluded from Git to keep machine-specific configuration out of the repository.

### 3. Verify connectivity

```bash
ansible homelab -i inventory/hosts.ini -m ping
```

### 4. Run the playbooks

Inspect the server:

```bash
ansible-playbook -i inventory/hosts.ini playbooks/inspect.yml
```

Install common packages:

```bash
ansible-playbook -i inventory/hosts.ini playbooks/packages.yml --ask-become-pass
```

Install and configure Docker:

```bash
ansible-playbook -i inventory/hosts.ini playbooks/docker.yml --ask-become-pass
```

Deploy the Nginx web service:

```bash
ansible-playbook -i inventory/hosts.ini playbooks/web.yml --ask-become-pass
```

Ansible prompts for the privilege escalation password when required.

## Web Service

The web service runs Nginx in a Docker container managed by Docker Compose.

- **Container image:** `nginx:alpine`
- **Host port:** `8080`
- **Container port:** `80`
- **Restart policy:** `unless-stopped`

The `web` Ansible role creates the service directory, copies the Compose configuration, starts the service, and checks its HTTP response.

To test the service from a machine that can reach the server:

```bash
curl -I http://<ROCK_PI_IP>:8080
```

Replace `<ROCK_PI_IP>` with the server's IP address.

## Ansible Roles

The web deployment is organized as an Ansible role:

```text
roles/web/tasks/main.yml
```

The repository-level `ansible.cfg` configures Ansible to locate roles in the `roles/` directory.

This structure makes the deployment easier to maintain and allows the role to be extended as the infrastructure grows.

## Continuous Integration

GitHub Actions runs on pushes to `main` and pull requests targeting `main`.

The workflow:

1. Checks out the repository.
2. Sets up Python 3.13.
3. Installs Ansible and the `community.docker` collection.
4. Runs syntax checks against the playbooks.

The workflow validates playbook syntax but does not perform a live deployment to the Rock Pi.

## Project Goals

- Automate home server administration.
- Practice Infrastructure as Code with Ansible.
- Use idempotent configuration management.
- Deploy services with Docker Compose.
- Organize reusable automation into Ansible roles.
- Learn Git workflows and CI with GitHub Actions.


