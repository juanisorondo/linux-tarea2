# -*- mode: ruby -*-
# vi: set ft=ruby :

required_plugins = %w( vagrant-cachier )
required_plugins.each do |plugin|
  system "vagrant plugin install #{plugin}" unless Vagrant.has_plugin? plugin
end

VAGRANTFILE_API_VERSION = "2"
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :box
  end

  # Provider
  (1..2).each do |n|
    config.vm.define "vm0#{n}" do |srv|
        srv.vm.box ="ubuntu/trusty64"
        srv.vm.hostname = "vm0#{n}"
        srv.vm.network "private_network", ip: "10.0.0.1#{n}"
    end
  end

  # Provision 
  config.vm.provision "shell", inline: <<-SHELL
    sudo su
    adduser --disabled-password --gecos "" ucu
    echo ucu:pass | chpasswd
    cp /vagrant/files/sshd_config /etc/ssh/
    service ssh restart
    cp /vagrant/files/ssh_config /etc/ssh/
    chmod o+w /vagrant -R
    if ! [ -f "/vagrant/files/ssh" ]
      then
        ssh-keygen -t rsa -N '' -f /vagrant/files/ssh/id_rsa
    fi
    mkdir /home/ucu/.ssh
    cp /vagrant/files/ssh/id_rsa /home/ucu/.ssh/
    cp /vagrant/files/ssh/id_rsa.pub /home/ucu/.ssh/authorized_keys
    chown -R ucu /home/ucu/.ssh
    chmod 700 /home/ucu/.ssh
    chmod -R 600 /home/ucu/.ssh/*
        
  SHELL

end
