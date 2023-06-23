#-*- mode: ruby -*-
# vi: set ft=ruby :


Vagrant.configure("2") do |config|

  config.vm.define "pxeserver" do |server|
    server.vm.box = 'bento/centos-8.4'
    server.vm.disk :disk, size: "15GB", name: "extra_storage1"

    server.vm.host_name = 'pxeserver'
    server.vm.network :private_network, 
                       ip: "10.0.0.20", 
                       virtualbox__intnet: 'pxenet'

    server.vm.network "forwarded_port", guest: 80, host: 8083
    server.vm.network :private_network, ip: "192.168.56.10", adapter: 3

    server.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
      vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    end
  end

  config.vm.define "pxeclient" do |pxeclient|
    pxeclient.vm.box = 'bento/centos-8.4'
    pxeclient.vm.host_name = 'pxeclient'
    pxeclient.vm.network :private_network, ip: "10.0.0.21"
    pxeclient.vm.provider "virtualbox" do |vb|
      vb.memory = "2048"
      vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      vb.customize [
        'modifyvm', :id,
        '--nic1', 'intnet',
        '--intnet1', 'pxenet',
        '--nic2', 'nat',
        '--boot1', 'net',
        '--boot2', 'none',
        '--boot3', 'none',
        '--boot4', 'none'
      ]
      vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    end
  end

end
