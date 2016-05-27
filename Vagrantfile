def configure_vm(vm, **opts)

	vm.box = opts.fetch(:box, "centos/7")
		
    vm.network :private_network, ip: opts[:private_ip]

    vm.provider "virtualbox" do |vb|
      vb.memory = 2048
      vb.cpus = 2
    end
    
    vm.provider :libvirt do |domain|
		domain.memory = opts.fetch(:memory, 4096)
		domain.cpus = opts.fetch(:cpus, 2)
		domain.nested = true
	end

	# Disable default share, because we dont use it
	vm.synced_folder ".", "/vagrant", disabled: true
end


def apply_ansible(vm, playbook)
	vm.provision :ansible do |ansible|
		ansible.host_key_checking = false
		ansible.playbook = playbook
		ansible.verbose = "v"
	end
end


Vagrant.configure(2) do |config|



	# ---------------------------------------------------------------------------------------------
	#
	# Definition of the VM for running test web application in k8s.
	#
	config.vm.define "test" do |test|
	
		configure_vm(test.vm, private_ip: "10.9.8.7")

		apply_ansible(test.vm, "test.yaml")
        
	end

end
