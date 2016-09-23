# -*- mode: ruby -*-
# vi: set ft=ruby :

# global configuration
VAGRANTFILE_API_VERSION = "2"
VAGRANT_BOX = "ubuntu/trusty64"
VAGRANT_BOX_MEMORY = 512
VIRTUAL_BOX_NAME = "ansibleskeleton"

# provisioning configuration
ROLE_FOLDER = "roles"
PLAY_FOLDER = "plays"

# only change these lines if you know what you do
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
    config.vm.box = VAGRANT_BOX
    config.vm.hostname = VIRTUAL_BOX_NAME
    config.vm.define VIRTUAL_BOX_NAME do |ansibleskeleton|
    end

    # forward ssh requests for public keys
    config.ssh.forward_agent = true

    config.vm.network "private_network", ip: "192.168.162.100"
    config.vm.synced_folder "./", "/vagrant", :nfs => true

    # configure virtual box
    config.vm.provider :virtualbox do |vb|
        vb.name = VIRTUAL_BOX_NAME
        vb.customize ["modifyvm", :id, "--memory", VAGRANT_BOX_MEMORY]
    end

    config.vm.provision "ansible" do |ansible|
        ansible.galaxy_role_file = ROLE_FOLDER + "/requirements.yml"
        ansible.galaxy_command = "ansible-galaxy install -i --role-file=%{role_file} --roles-path=" + ROLE_FOLDER + "/external"
        ansible.playbook = PLAY_FOLDER + "/vagrant.yml"
    end
end
