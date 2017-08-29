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

  config.vm.provision "shell", inline: $script

  config.vm.define "vagrant1" do |vagrant1|
      vagrant1.vm.box = "ubuntu/trusty64"
      vagrant1.vm.network "forwarded_port", guest: 80, host: 8001
      vagrant1.vm.network "forwarded_port", guest: 8080, host: 8080
  end
end
