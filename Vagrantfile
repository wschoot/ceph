# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

	config.vm.box = "centos/7"
	config.vm.synced_folder '.', '/vagrant', disabled: true

	config.vm.provider :libvirt do |vb|
		vb.memory = 1024
		vb.storage :file, :size => '20G'
	end

	config.vm.define :cephmaster do |cephmaster_config|
		cephmaster_config.vm.hostname = "cephmaster"
		cephmaster_config.vm.network :private_network, ip: "10.0.15.10"
		cephmaster_config.vm.provision :hostmanager
	end

	N = 3
	(1..N).each do |i|
		config.vm.define "cephstor#{i}" do |node|
			node.vm.hostname = "cephstor#{i}"
			node.vm.network :private_network, ip: "10.0.15.3#{i}"
			node.vm.provision :hostmanager
			if i == N
				node.vm.provision "ansible" do |ansible|
					ansible.playbook = "playbook.yml"
					ansible.limit = "all"
					ansible.groups = {
						"master"  => ["cephmaster"],
						"cephstor" => ["cephstor1", "cephstor2", "cephstor3"],
					}
				end
			end
		end
	end

	if Vagrant.has_plugin?("vagrant-hostmanager")
		config.hostmanager.enabled = false
		config.hostmanager.manage_host = true
		config.hostmanager.manage_guest = true
		config.hostmanager.include_offline = true
	end
end
