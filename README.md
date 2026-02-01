# Vagrant Claude Wrapper

Use Claude Code in paranoid mode.

This Vagrant config spins up an Ubuntu 24.04 VM, configures it with Claude Code, and gives the VM access **only** to the current host directory.

In this way you can run Claude Code without it having access to your entire machine (because do you trust an LLM **that** much?).

## Prerequisites

You will need the following dependencies installed on your host machine:

* VirtualBox
* Vagrant

## Usage

1. Copy the [Vagrantfile](./Vagrantfile) to your project.
2. Run `vagrant up`. This will take a few minutes the first time you use it, as it downloads an image, provisions a VM and handles software installation & updates.
3. Once provisioned run `vagrant ssh`.
