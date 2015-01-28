# -*- mode: ruby -*-
# vi: set ft=ruby :

BOX_NAME = ENV["BOX_NAME"] || "trusty"
BOX_URI = ENV["BOX_URI"] || "https://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box"
BOX_MEMORY = ENV["BOX_MEMORY"] || "2048"

Vagrant::configure("2") do |config|
  config.ssh.forward_agent = true

  config.vm.box = BOX_NAME
  config.vm.box_url = BOX_URI

  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    vb.customize ["modifyvm", :id, "--memory", BOX_MEMORY]
  end

  config.vm.define "ptero", primary: true do |vm|
    vm.vm.network :forwarded_port, guest: 80, host: 8080
    vm.vm.provision :shell, :inline => "apt-get update"
    vm.vm.provision :shell, :inline => "apt-get -qq -y install git python-dev python-pip rabbitmq-server redis-server postgresql-server-dev-9.3"
    vm.vm.provision :shell, :inline => "pip install tox"
    vm.vm.provision :shell, :privileged => false, :inline => "git clone http://github.com/genome/ptero"
  end
end
