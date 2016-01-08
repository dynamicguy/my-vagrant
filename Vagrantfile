# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.require_version ">= 1.6.5"
unless Vagrant.has_plugin?("vagrant-vbguest")
  raise "Please install the vagrant-vbguest plugin by running `vagrant plugin install vagrant-vbguest`"
end

MEMORY = 4096
CPU_COUNT = 2
VAGRANTFILE_API_VERSION = "2"

$script = <<SCRIPT
export PYTHONUNBUFFERED=1
sudo apt-get update

SCRIPT

rel = ""

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "ubuntu/trusty64"

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
  config.vm.network "private_network", ip: "192.168.33.10"

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
    # vb.memory = MEMORY.to_s
		# vb.cpus = CPU_COUNT.to_s
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--memory", MEMORY.to_s]
    vb.customize ["modifyvm", :id, "--cpus", CPU_COUNT.to_s]

    # Allow DNS to work for Ubuntu 12.10 host
    # http://askubuntu.com/questions/238040/how-do-i-fix-name-service-for-vagrant-client
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
		vb.customize ["modifyvm", :id, "--vram", "16"]
  end

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.	
  config.push.define "atlas" do |push|
    push.app = "ferdous/ubuntu"
		push.vcs = true		
		# set your env var as export ATLAS_TOKEN = "your token goes here"
  end

	# config.vm.network :forwarded_port, guest: 8764, host: 8764
	# config.vm.network :forwarded_port, guest: 8983, host: 8983
	# config.vm.network :forwarded_port, guest: 9983, host: 9983
	
	config.vm.synced_folder "../vagrant_apps", "/vagrant"
  config.ssh.insert_key = true  

  # Enable X11 forwarding so we can interact with GUI applications
  if ENV['VAGRANT_X11']
    config.ssh.forward_x11 = true
  end

  # Use vagrant-vbguest plugin to make sure Guest Additions are in sync
  config.vbguest.auto_reboot = true
  config.vbguest.auto_update = true

  # We can then tell the Vagrant instance to update itself.
  config.vm.provision "shell", inline: $script, args: rel

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   sudo apt-get update
  # 		sudo apt-get install -y build-essential
  # 		sudo apt-get install -y oracle-java8-installer
  # 		sudo apt-get install -y oracle-java8-set-default
  # SHELL
end
