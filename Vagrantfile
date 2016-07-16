$script = <<SCRIPT
#!/bin/sh
set -e

export DEBIAN_FRONTEND=noninteractive
echo "Update packages"
sudo apt-get -yqq update > /dev/null 2>&1
echo "Prepare Java installation"
echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | sudo /usr/bin/debconf-set-selections > /dev/null 2>&1
sudo apt-get -yqq install curl software-properties-common python-software-properties > /dev/null 2>&1
sudo add-apt-repository -y ppa:webupd8team/java > /dev/null 2>&1
sudo apt-get -yqq update > /dev/null 2>&1
echo "Install Oracle Java 8, libapr1 and openssl"
sudo apt-get -yqq install autoconf libtool dh-autoreconf build-essential wget git oracle-java8-installer > /dev/null 2>&1
#sudo apt-get -yqq install autoconf libtool libssl-dev libkrb5-dev python-dev python-pip haveged openssl wget git oracle-java8-installer oracle-java8-unlimited-jce-policy > /dev/null 2>&1

wget http://apache.openmirror.de/maven/maven-3/3.3.9/binaries/apache-maven-3.3.9-bin.tar.gz > /dev/null 2>&1

sudo mkdir -p /usr/local/apache-maven > /dev/null 2>&1
sudo mv apache-maven-3.3.9-bin.tar.gz /usr/local/apache-maven > /dev/null 2>&1
cd /usr/local/apache-maven
sudo tar -xzf apache-maven-3.3.9-bin.tar.gz > /dev/null 2>&1

export M2_HOME=/usr/local/apache-maven/apache-maven-3.3.9
export M2=$M2_HOME/bin
export MAVEN_OPTS="-Xms256m -Xmx512m"
export PATH=$M2:$PATH

cd /vagrant
cp -a .gnupg ~
export GPG_TTY=$(tty)
mvn deploy -s private_settings.xml

SCRIPT
#End inline script

# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "hashicorp/precise64"
  config.vm.provision "shell", inline: $script

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   sudo apt-get update
  #   sudo apt-get install -y apache2
  # SHELL
end
