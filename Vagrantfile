# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # General Vagrant VM configuration
  config.vm.box = "ubuntu/jammy64"
  config.ssh.insert_key = true
  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.vm.provider :virtualbox do |v|
    v.memory = 512
    v.linked_clone = true
  end

  # CA server
  config.vm.define "ca-server" do |ca|
    ca.vm.hostname = "ca-server.test"
    ca.vm.network :private_network, ip: "192.168.56.2"
  end

  # VPN server
  config.vm.define "vpn-server" do |vpn|
    vpn.vm.hostname = "vpn-server.test"
    vpn.vm.network :private_network, ip: "192.168.56.3"
  end

  # Ansible provisioning
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "vagrant/vagrant-provisioning.yml"
    ansible.groups = {
      "ca_server" => ["ca-server"],
      "vpn_server" => ["vpn-server"],
    }
  end
end