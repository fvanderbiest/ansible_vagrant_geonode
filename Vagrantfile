# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

$script = <<SCRIPT
echo configuring ssh in guest...
sudo sed -i '/AcceptEnv/d' /etc/ssh/sshd_config
sudo /etc/init.d/ssh restart
SCRIPT

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # Use the same key for each machine 
  config.ssh.insert_key = false

  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--memory", "4096"]
    vb.customize ["modifyvm", :id, "--cpus", "2"]
  end

  config.vm.define "vagrant1" do |vagrant1|
      vagrant1.vm.box = "ubuntu/trusty64"
      vagrant1.vm.network "forwarded_port", guest: 22, host: 9999, id: "ssh"
      vagrant1.vm.network "forwarded_port", guest: 80, host: 8001
      vagrant1.vm.network "forwarded_port", guest: 8080, host: 8080
  end

  config.vm.provision "shell", inline: $script

  config.vm.provision "ansible" do |ansible|
    # execute this playbook for vm provisioning:
    ansible.playbook = "main.yml"
    # display ansible-playbook output:
    ansible.verbose = "v"
    # limit ansible-playbook execution to one machine:
    ansible.limit = "mygeonode"
    # ... as referenced in our "hosts" file:
    ansible.inventory_path = "hosts"
    # ssh connection parameters for ansible:
    ansible.extra_vars = { ansible_ssh_host: '127.0.0.1', ansible_ssh_user: 'vagrant', ansible_ssh_port: 9999 }
  end

end
