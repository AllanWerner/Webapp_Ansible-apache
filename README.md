# Webapp Ansible Apache

Ansible automation project that provisions a remote Linux server and deploys a containerized web application (Apache httpd or WordPress) using Docker.

## Overview

This project uses Ansible playbooks to automate the full deployment workflow of a web server, from initial system setup to running a containerized application:

- Installs required system packages (`epel-release`, `git`, `wget`, `python3`, `pip`) on CentOS hosts
- Installs Docker's Python SDK to let Ansible manage containers
- Deploys a static website served by an **Apache httpd** container, built from a Jinja2 template
- Deploys a **WordPress** stack via an external, reusable Ansible role pulled from Ansible Galaxy
- Secures sensitive data (SSH/privilege escalation password) with **Ansible Vault**

## Tech Stack

- **Ansible** — configuration management and orchestration
- **Docker** — application containerization (`httpd`, WordPress role)
- **CentOS** — target operating system
- **Ansible Vault** — encryption of credentials
- **Jinja2** — dynamic templating of the served web page

## Project Structure

```
.
├── ansible.cfg                # Ansible configuration (inventory path, privilege escalation, SSH options)
├── hosts.yml                  # Inventory file (defines the "prod" host group)
├── deploy.yml                 # Playbook: installs Docker deps and runs an Apache httpd container
├── wordpress.yml              # Playbook: cleans Docker and deploys WordPress via an external role
├── roles/
│   └── requirements.yml       # External role dependency (ansible-role-containerized-wordpress)
├── group_vars/
│   └── prod.yml                # Vault-encrypted variables for the "prod" group
├── files/
│   └── secrets/
│       └── credentials.yml     # Vault-encrypted connection credentials
└── templates/
    └── index.html.j2          # Jinja2 template for the Apache website
```

## Prerequisites

- Ansible installed on the control node
- SSH access to the target host(s)
- Target host running CentOS with Docker support
- Ansible Vault password to decrypt credentials

## Usage

1. Install the external role dependencies:
   ```bash
   ansible-galaxy install -r roles/requirements.yml
   ```

2. Update `hosts.yml` with your target host(s) under the `prod` group.

3. Deploy the Apache httpd container:
   ```bash
   ansible-playbook deploy.yml --ask-vault-pass
   ```

4. Or deploy the WordPress stack:
   ```bash
   ansible-playbook wordpress.yml --ask-vault-pass
   ```

## Security

Sensitive values (SSH/privilege escalation password) are encrypted with **Ansible Vault** and stored in `group_vars/prod.yml` and `files/secrets/credentials.yml`. They are never committed in plaintext.

## Skills Demonstrated

- Writing and structuring Ansible playbooks, inventories, and roles
- Managing external dependencies with Ansible Galaxy
- Container-based application deployment with Docker
- Secrets management with Ansible Vault
- Configuration templating with Jinja2
