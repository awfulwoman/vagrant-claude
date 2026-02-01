# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Vagrant config for Ubuntu 24.04 dev VM wrapping Claude Code instances. Enables Claude to run with relaxed permissions in isolated environment.

## Architecture

**Key Concept**: This Vagrantfile dynamically names VM and workspace based on parent directory name:
- `vm_name = File.basename(Dir.getwd)` - VM named after directory
- Workspace path: `/workspace-#{vm_name}` - e.g., `/workspace-vagrant-claude`
- Synced folder: `.` → workspace path

**VM Specs**:
- 4GB RAM, 2 CPUs
- Ubuntu 24.04 (bento box)
- VirtualBox provider

**Provisioning**:
- Root provisioning: Docker setup, user permissions
- User provisioning: NVM, Claude CLI, SSH agent setup (mostly commented out in current state)
- Copies host credentials: `.gitconfig`, SSH keys (`id_ed25519`)

**SSH Agent**: Auto-starts on first login, prompts for `ssh-add ~/.ssh/id_ed25519`, creates `~/.ssh_reminder_shown` marker.

## Common Commands

```bash
# VM lifecycle
vagrant up        # Start VM
vagrant ssh       # SSH into VM
vagrant halt      # Stop VM
vagrant destroy   # Destroy VM

# Inside VM
cd /workspace-vagrant-claude  # Navigate to synced workspace (auto-cd on login)
```

## Notes

- Most provisioning steps currently commented out (lines 25-34, 85-87 in Vagrantfile)
- SSH keys imported from host system during provisioning
- Claude CLI path added to `.bashrc`: `$HOME/.local/bin`
