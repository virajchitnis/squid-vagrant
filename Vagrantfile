# -*- mode: ruby -*-
# vi: set ft=ruby :

BOX_NAME = "squid-vagrant"
BOX_IP = "192.168.56.200"

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
  config.vm.hostname = BOX_NAME

  config.vm.provider "virtualbox" do |v|
    v.name = BOX_NAME
  end

  # Host-only networking for proxy connections.
  config.vm.network "private_network", ip: BOX_IP, :name => 'vboxnet0', :adapter => 2

  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y squid tmux
    service squid stop
    systemctl disable squid
    cp /vagrant/squid.conf /etc/squid/squid.conf
  SHELL

  # Run squid in foreground in tmux.
  # If squid is run automatically via the system service manager,
  # the user is unable to access the vagrant directory.
  # But running the squid via tmux runs it as the vagrant user, thereby allowing
  # access to the vagrant directory.
  config.vm.provision "shell", privileged: false, run: "always", inline: <<-SHELL
    tmux new-session -d "squid -N -d1"
  SHELL
end
