# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-18.04"
  config.vm.box_check_update = false
  config.vm.network :forwarded_port, guest: 80, host: 8080
  config.vm.network :forwarded_port, guest: 22, host: 12222, id: "ssh"
  #config.vm.network "public_network"
  config.vm.provider "virtualbox" do |vb|
    vb.memory = 4096
    vb.cpus = 2
  end

  config.vm.provision "shell", inline: <<-SHELL

    # Prepare Yocto environment.
    sudo apt-get update
    sudo apt-get -y install gawk wget git-core diffstat unzip texinfo gcc-multilib \
     build-essential chrpath socat cpio python3 python3-pip python3-pexpect \
     xz-utils debianutils iputils-ping python3-git python3-jinja2 libegl1-mesa libsdl1.2-dev \
     pylint3 xterm

  SHELL

  config.vm.provision "shell", privileged: false, inline: <<-SHELL

    # Cd into a directory where you keep utilities and make sure it's in your PATH
    mkdir ~/bin
    cd ~/bin/
    # Download the repo script
    curl https://storage.googleapis.com/git-repo-downloads/repo > repo
    # Make repo executable
    chmod a+x repo

    mkdir ~/yocto
    cd ~/yocto
    # Fetch the manifest and checkout the target release version
    repo init -u git://github.com/Xilinx/yocto-manifests.git -b rel-v2021.1
    # Fetch all the source from the repositories in the manifest
    repo sync
    # Checkout the corresponding release for each repository
    repo start rel-v2021.1 --all

  SHELL

end


