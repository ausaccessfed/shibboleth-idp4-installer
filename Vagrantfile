# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  # config.vm.define "Shibboleth-Idp" do |subconfig|
  #   # https://app.vagrantup.com/micczu/boxes/centos7-ansible
  #   subconfig.vm.box = "micczu/centos7-ansible"
  #   subconfig.vm.box_version = "0.0.1"

  #   subconfig.vm.hostname = "shibboleth-idp-4"
  #   # config.vm.network "private_network", ip: "55.55.55.150"
  #   #config.vm.synced_folder ".", "/home/vagrant/shared/"
  #   config.vm.synced_folder ".", "/shibboleth-idp4-installer"

  #   subconfig.vm.provider "virtualbox" do |vb|
  #       vb.memory = "4096"
  #       vb.cpus = 2
  #     end
  # end

  config.vm.define "Shib-Installer-V4" do |subconfig|
    subconfig.vm.box = "centos/7"
    subconfig.vm.hostname = "shib-installer-v4"

    # config.vm.network "private_network", ip: "55.55.55.150"
    # config.vm.synced_folder "../openldap_server", "/openldap_server"

    subconfig.vm.provider "virtualbox" do |vb|
        vb.memory = "4096"
        vb.cpus = 2
      end
  end

    
  
  config.vm.provision "shell", inline: <<-SHELL    
    #
    # Update and install basic linux programs for development
    #
    sudo yum update -y     
    sudo yum install -y epel-release
    sudo yum install -y wget
    sudo yum install -y curl
    sudo yum install -y vim
    sudo yum install -y git    
    sudo yum install -y build-essential
    sudo yum install -y unzip 
    sudo yum install -y pyOpenSSL 
    sudo yum install -y ansible
    sudo yum install -y openldap openldap-clients openldap-servers
    #
    # Install Ansible
    #
    # cd /usr/local/src
    # sudo yum -y install git python-jinja2 python-paramiko PyYAML make MySQL-python
    # sudo git clone https://github.com/ansible/ansible.git
    # cd /usr/local/src/ansible
    # sudo git submodule update --init --recursive
    # sudo make install
    # sudo echo "[localhost]" > ~/ansible_hosts
    # sudo echo "localhost ansible_connection=local" >> ~/ansible_hosts
    # export ANSIBLE_INVENTORY=~/ansible_hosts
  SHELL

end