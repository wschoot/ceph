# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "centos/7"
  config.vm.synced_folder '.', '/vagrant', disabled: true

  config.vm.provider :libvirt do |vb|
    vb.memory = 1024
  end

	config.vm.define :cephmaster do |cephmaster_config|
		cephmaster_config.vm.hostname = "cephmaster"
		cephmaster_config.vm.network :private_network, ip: "10.0.15.10"
	end

	(1..3).each do |i|
		config.vm.define "cepthstor#{i}" do |node|
			node.vm.hostname = "cepthstor#{i}"
			node.vm.network :private_network, ip: "10.0.15.3#{i}"
		end
	end

	config.vm.provision "ansible" do |ansible|
		ansible.playbook = "playbook.yml"
		ansible.groups = {
			"master"  => ["cephmaster"],
			"cephstor" => ["cephstor1", "cephstor2", "cephstor3"],
		}
	end

	if Vagrant.has_plugin?("vagrant-hostmanager")
		#config.hostmanager.enabled = true
		#config.hostmanager.manage_host = true
		#config.hostmanager.manage_guest = true
	end
end
