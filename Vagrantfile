# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # Used box
  config.vm.box = "ubuntu/xenial64"
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "512" 
  end

  # Accessing "localhost:8080" will access port 80 on the guest machine.
  config.vm.network "forwarded_port", guest:80, host:8080, auto_correct: true

  # Private Network
  config.vm.network :private_network, ip: "192.168.68.8"

  # Install stuff
  config.vm.provision :shell, :path => ".provision/bootstrap.sh"
end