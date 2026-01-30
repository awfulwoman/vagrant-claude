# vagrant-claude

Vagrant config for Ubuntu 24.04 dev VM with Claude CLI.

## Setup

Includes:
- Docker
- Node.js (LTS via NVM)
- Claude CLI
- Git, gh, curl, wget, jq, htop

VM specs:
- 4GB RAM
- 2 CPUs
- Synced folder: `.` → `/agent-workspace`

## Usage

```bash
# Start VM
vagrant up

# SSH into VM
vagrant ssh

# Stop VM
vagrant halt

# Destroy VM
vagrant destroy
```

## SSH Keys

Imports SSH keys from GitHub user `awfulwoman` during provisioning.
