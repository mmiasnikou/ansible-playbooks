# Ansible Playbooks

Production-ready Ansible playbooks for server provisioning and application deployment.

## Playbooks

### 🔐 server-init.yml
Initial server setup and security hardening.

**What it does:**
- System update & essential packages
- Create admin user with SSH key
- SSH hardening (disable root, password auth)
- UFW firewall configuration
- Fail2Ban setup
- Kernel security settings
- Automatic security updates

```bash
ansible-playbook playbooks/server-init.yml -l webservers
```

### 🐳 docker-install.yml
Docker and Docker Compose installation.

**What it does:**
- Remove old Docker versions
- Install Docker CE from official repo
- Configure Docker daemon (logging, storage)
- Add users to docker group
- Create common networks
- Setup cleanup cron job

```bash
ansible-playbook playbooks/docker-install.yml -l webservers
```

### 🚀 deploy-app.yml
Application deployment with Docker.

**What it does:**
- Clone repository
- Build and start containers
- Health checks
- Symlink management
- Keep last 5 releases
- Rollback support
- Telegram notifications

```bash
ansible-playbook playbooks/deploy-app.yml -l webservers -e "app_branch=main"
```

## Quick Start

```bash
# 1. Configure inventory
cp inventory/hosts.example inventory/hosts
nano inventory/hosts

# 2. Test connection
ansible all -m ping

# 3. Run playbooks
ansible-playbook playbooks/server-init.yml
ansible-playbook playbooks/docker-install.yml
ansible-playbook playbooks/deploy-app.yml
```

## Directory Structure

```
ansible-playbooks/
├── ansible.cfg
├── inventory/
│   └── hosts.example
├── playbooks/
│   ├── server-init.yml
│   ├── docker-install.yml
│   └── deploy-app.yml
├── roles/           # (optional) reusable roles
├── group_vars/      # (optional) group variables
└── templates/       # (optional) Jinja2 templates
```

## Variables

### server-init.yml
```yaml
admin_user: deploy          # Admin username
ssh_port: 22                # SSH port
allowed_ssh_users: [deploy] # Users allowed SSH
timezone: UTC               # Server timezone
```

### docker-install.yml
```yaml
docker_users: [deploy]      # Users to add to docker group
```

### deploy-app.yml
```yaml
app_name: myapp                          # Application name
app_user: deploy                         # Application owner
app_path: /opt/myapp                     # Install path
app_repo: https://github.com/user/repo   # Git repository
app_branch: main                         # Branch to deploy
app_domain: app.example.com              # Domain
telegram_bot_token: ""                   # Optional notifications
telegram_chat_id: ""                     # Optional notifications
```

## Usage Examples

```bash
# Deploy specific branch
ansible-playbook playbooks/deploy-app.yml -e "app_branch=develop"

# Deploy to specific server
ansible-playbook playbooks/deploy-app.yml -l web1

# Dry run (check mode)
ansible-playbook playbooks/server-init.yml --check

# Verbose output
ansible-playbook playbooks/server-init.yml -vvv

# With custom inventory
ansible-playbook -i inventory/production playbooks/deploy-app.yml
```

## Requirements

- Ansible 2.10+
- Python 3.8+
- SSH access to target servers
- `community.docker` collection (for docker tasks)

```bash
# Install Ansible
pip install ansible

# Install required collections
ansible-galaxy collection install community.docker
```

## Author

Mikhail Miasnikou — System Administrator / DevOps Engineer

## License

MIT
