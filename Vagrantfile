vm_name = File.basename(Dir.getwd)

Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-24.04"
  config.vm.box_version = "202510.26.0"

  #config.vm.network "forwarded_port", guest: 3000, host: 3000, auto_correct: true
  config.vm.synced_folder ".", "/agent-workspace", type: "virtualbox"

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
    apt install -y docker.io git unzip jq curl gh wget

    echo "#########################################"
    echo "# APT UPGRADE"
    echo "#########################################"
    apt upgrade -y

    echo "#########################################"
    echo "# USER SETUP"
    echo "#########################################"
    usermod -aG docker vagrant
    chown -R vagrant:vagrant /agent-workspace
    
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
  echo "# IMPORT KEYS"
  echo "#########################################"
  ssh-import-id-gh awfulwoman

  echo "#########################################"
  echo "# CLAUDE SETUP"
  echo "#########################################"
  curl -fsSL https://claude.ai/install.sh | bash
  echo 'export PATH="$HOME/.local/bin:$PATH"' >> $HOME/.bashrc

  echo "cd /agent-workspace" >> $HOME/.bashrc

  SCRIPT

  config.vm.provision "shell", inline: $script, privileged: false
end