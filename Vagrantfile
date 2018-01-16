# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.define "test" do |test|
    test.vm.hostname = "vagrant-verium-miner-test"
    test.vm.network :private_network, type: "dhcp"

    test.vm.provision "shell", inline: "sudo apt-get -y update && sudo apt-get -y upgrade && sudo apt-get -y install python"

    test.vm.provision "ansible" do |ansible|
      ansible.playbook = "ansible/playbook.yml"
    end

    if Vagrant.has_plugin?("vagrant-cachier")
      test.cache.scope = :machine
    end

    test.vm.synced_folder ".", "/vagrant"

    test.vm.provider :virtualbox do |vb, override|
      override.vm.box = "ubuntu/xenial64"
      vb.memory = 4096
      vb.cpus = 6
      vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    end

    test.vm.provider :docker do |d|
      d.image = "tknerr/baseimage-ubuntu:16.04"
      d.privileged = true
      d.has_ssh = true
    end
  end

end
