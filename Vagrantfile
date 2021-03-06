# -*- mode: ruby -*-
# vi: set ft=ruby :

require './provision_reboot'

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu-server-trusty"
  config.vm.box_url = "https://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box"

  #config.vm.network :forwarded_port, guest: 3000, host: 3000, auto_correct: true

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network :private_network, ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network :public_network

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "~", "/vagrant"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider :virtualbox do |vb|
    vb.name = "vagrant-nodejs-mongodb-kafka"
    vb.customize [
      "modifyvm", :id,
      "--memory", "512"
    ]
  end

  module OS
    def OS.windows?
      (/cygwin|mswin|mingw|bccwin|wince|emx/ =~ RUBY_PLATFORM) != nil
    end
  end

  # The shell to use when executing SSH commands from Vagrant
  config.ssh.shell = "bash -c 'BASH_ENV=/etc/profile exec bash'"
  
  # PROVISION
  # update the OS
  config.vm.provision "shell", path: "https://raw.githubusercontent.com/joaquimserafim/vagrant-provision/master/provision.sh", args: "update_os"

  # need the 'provision_reboot' file for this
  # now we need to reboot in the middle of the provision
  if OS.windows?
    config.vm.provision :windows_reboot
  else
    config.vm.provision :unix_reboot
  end

  # THE SOFTWARE TO BE INSTALL COME HERE
  config.vm.provision "shell", path: "https://raw.githubusercontent.com/joaquimserafim/vagrant-provision/master/provision.sh", args: "git htop nodejs mongodb zookeeper kafka"
end
