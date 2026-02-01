# Vagrant Claude Wrapper

Use Claude Code in paranoid mode.

This Vagrant config spins up an Ubuntu 24.04 VM, configures it with Claude Code, and gives the VM access **only** to the current host directory.

In this way you can run Claude Code without it having access to your entire machine (because do you trust an LLM **that** much?).

## Prerequisites

You will need the following dependencies installed on your host machine:

* VirtualBox
* Vagrant

### MacOS

```bash
brew install virtualbox vagrant --cask
```

## Usage

**Copy the [Vagrantfile](./Vagrantfile) to your project.**

```bash
curl -O https://raw.githubusercontent.com/awfulwoman/vagrant-claude/refs/heads/main/Vagrantfile
```

**Initialise the VM.** This will take a few minutes the first time you use it, as it downloads an image, provisions a VM and handles software installation & updates.

```bash
vagrant up
```

**Log into the VM.**

```bash
vagrant ssh
```

You'll find yourself in the working directory.
