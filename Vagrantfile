# -*- mode: ruby -*-
# vi: set ft=ruby :

system("sudo ansible-galaxy install -r provisioning-jenkins/requirements.yml")
system("sudo ansible-galaxy install -r provisioning-docker/requirements.yml")
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.define :jenkins do |jenkins|
    jenkins.vm.box = "geerlingguy/ubuntu1604"
    jenkins.ssh.insert_key = false
    jenkins.vm.synced_folder ".", "/vagrant", disabled: true
    jenkins.vm.hostname = "jenkins"
    jenkins.vm.network :private_network, ip: "192.168.33.55"
      jenkins.vm.provider :virtualbox do |v|
        v.name = "jenkins"
        v.memory = 512
        v.cpus = 2
        v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
        v.customize ["modifyvm", :id, "--ioapic", "on"]
      end
    # Ansible provisioner.
    jenkins.vm.provision "ansible" do |ansible|
      ansible.playbook = "provisioning-jenkins/playbook.yml"
      ansible.inventory_path = "provisioning-jenkins/inventory"
      ansible.sudo = true
    end
 
  end
  config.vm.define :docker do |docker|
    docker.vm.box = "geerlingguy/ubuntu1604"
    docker.ssh.insert_key = false
    docker.vm.synced_folder ".", "/vagrant", disabled: true
    docker.vm.hostname = "docker"
    docker.vm.network :private_network, ip: "192.168.33.60"
      docker.vm.provider :virtualbox do |v|
        v.name = "docker"
        v.memory = 512
        v.cpus = 2
        v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
        v.customize ["modifyvm", :id, "--ioapic", "on"]
    end  
    # Ansible provisioner.
    docker.vm.provision "ansible" do |ansible|
      ansible.playbook = "provisioning-docker/main.yml"
      ansible.sudo = true
    end
 
  end

end


