# -*- mode: ruby -*-
# vi: set ft=ruby :
puppet_control_repo   = '/vagrant'

Vagrant.configure("2") do |config|
  config.vm.provider "virtualbox" do |v|
    v.linked_clone = true
    v.customize [ "guestproperty", "set", :id, "/VirtualBox/GuestAdd/VBoxService/--timesync-set-threshold", 1000 ]
  end

  # config.vm.box = "ubuntu/xenial64"
  config.vm.box = "https://cloud-images.ubuntu.com/xenial/current/xenial-server-cloudimg-amd64-vagrant.box"
  config.vm.synced_folder ".", "/vagrant/"
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "1024"
  end

  config.vm.provision "shell", inline: <<~SHELL
    #!/usr/bin/env bash

    # For ansible
    apt-get install -y python

    # Install recommended extra packages
    apt-get install -y \
        linux-image-extra-$(uname -r) \
        linux-image-extra-virtual

    # Allow apt to use repo over HTTPS
    apt-get install -y \
        apt-transport-https \
        ca-certificates \
        curl \
        software-properties-common

    # Add Docker’s official GPG key
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

    # Set up the stable repo
    add-apt-repository \
       "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
       $(lsb_release -cs) \
       stable"

    # Update the packages
    apt-get update

    # Install docker-ce
    apt-get install -y docker-ce

    # Install docker-compose
    sudo curl -fsSL https://github.com/docker/compose/releases/download/1.20.0/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
    chmod +x /usr/local/bin/docker-compose
    /sbin/sysctl -w net.ipv4.conf.all.forwarding=1
    echo net.ipv4.conf.all.forwarding=1 >/etc/sysctl.d/20-net-ipv4-forwarding.conf

    # Access docker w/o sudo
    # usermod -aG docker ubuntu
  SHELL

  config.vm.provision "ansible" do |ansible|
    ansible.playbook           = "remediate_docker.yml"
    ansible.become             = true
    ansible.compatibility_mode = "2.0"
    config_file                = "ansible.cfg"
    ansible.groups = {
      "dockers" => ["default"],
    }

  end

end
