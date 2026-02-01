# Vagrant Claude Wrapper

Use Claude Code in paranoid mode.

This Vagrant config spins up an [Ubuntu 24.04 VM](https://portal.cloud.hashicorp.com/vagrant/discover/bento/ubuntu-24.04), [configures it with Claude Code](https://code.claude.com/docs/en/quickstart#step-1:-install-claude-code), and gives the VM access **only** to the current host directory. To make things more immediately usable it also copies in your current users SSH keys and your git config.

In this way you can run Claude Code without it having access to your entire machine (because do you trust an LLM **that** much?).

## Prerequisites

You will need the following dependencies installed on your host machine:

* VirtualBox
* Vagrant

### Installing Dependencies on MacOS

```bash
brew install virtualbox vagrant --cask
```

### Installing Dependencies on Ubuntu/Debian

```bash
apt install virtualbox vagrant
```

## Usage

Copy the [Vagrantfile](./Vagrantfile) to your project.

```bash
curl -O https://raw.githubusercontent.com/awfulwoman/vagrant-claude/refs/heads/main/Vagrantfile
```

Initialise the VM. This will take a few minutes the first time you use it, as it downloads an image, provisions a VM and handles software installation & Ubuntu updates.

```bash
vagrant up
```

Log into the VM.

```bash
vagrant ssh
```

You'll find yourself in the working directory of the VM. Run `ls` to confirm it can see the files on the host.

You can now start Claude Code.

```bash
claude
```

May the lord have mercy on your soul.
