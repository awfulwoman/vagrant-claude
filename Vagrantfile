# This Vagrantfile is used to wrap a VM around instances of Claude Code. This enables Claude to be run with much more relaxed permissions, without worrying about it going mental and trashing your laptop.

vm_name = File.basename(Dir.getwd)
workspace_path = "/workspace-#{vm_name}"

Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-24.04"
  config.vm.box_version = "202510.26.0"

  config.vm.synced_folder ".", workspace_path, type: "virtualbox"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "4096"
    vb.cpus = 2
    vb.gui = false
    vb.name = vm_name
    vb.customize ["modifyvm", :id, "--audio", "none"]
    vb.customize ["modifyvm", :id, "--usb", "off"]
  end
  
  # Provision as root
  config.vm.provision "shell", inline: <<-SHELL
    export DEBIAN_FRONTEND=noninteractive

    echo "#########################################"
    echo "# APT INSTALL"
    echo "#########################################"
    apt update
    apt install -y docker.io git unzip jq curl gh wget htop ansible

    echo "#########################################"
    echo "# APT UPGRADE"
    echo "#########################################"
    apt upgrade -y

    echo "#########################################"
    echo "# USER SETUP"
    echo "#########################################"
    usermod -aG docker vagrant
    chown -R vagrant:vagrant #{workspace_path}
    
  SHELL
end


# Provision as vagrant user
Vagrant.configure("2") do |config|
  $script = <<-SCRIPT

  echo "#########################################"
  echo "# NVM SETUP"
  echo "#########################################"
  curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
  export NVM_DIR="$HOME/.nvm"
  [ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"
  nvm install --lts
  nvm alias default lts/*

  echo "#########################################"
  echo "# CLAUDE SETUP"
  echo "#########################################"
  curl -fsSL https://claude.ai/install.sh | bash
  echo 'export PATH="$HOME/.local/bin:$PATH"' >> $HOME/.bashrc


  echo "#########################################"
  echo "# CUSTOMISE USER"
  echo "#########################################"
  echo "cd #{workspace_path}" >> $HOME/.bashrc

  # Auto-start ssh-agent on first login
  cat >> $HOME/.bashrc << 'EOF'
  if [ ! -f ~/.ssh_reminder_shown ]; then
    eval "$(ssh-agent -s)" > /dev/null
    echo "==> SSH agent started. Add your key:"
    ssh-add ~/.ssh/id_ed25519 && touch ~/.ssh_reminder_shown
  fi
  EOF

  SCRIPT
  config.vm.provision "shell", inline: $script, privileged: false

  # Copy over credentials to working VM
  config.vm.provision "file", source: "~/.gitconfig", destination: ".gitconfig"
  config.vm.provision "file", source: "~/.ssh/id_ed25519", destination: ".ssh/id_ed25519"
  config.vm.provision "file", source: "~/.ssh/id_ed25519.pub", destination: ".ssh/id_ed25519.pub"

end
