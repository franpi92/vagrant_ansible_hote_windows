# -*- mode: ruby -*-
# vi: set ft=ruby :
NODE_COUNT = 2

Vagrant.configure("2") do |config|
 
  #config.vm.box = "debian/jessie64"
  config.vm.box = "debian/bullseye64"
  private_key = File.read("id_rsa")
  public_key = File.read("id_rsa.pub")
  hosts_file = File.read("hosts")
  config.vm.provision :shell, :inline =>"
    echo '==== Copie des cles root'
      mkdir -p /root/.ssh
      chmod 700 /root/.ssh
      echo '#{private_key}' > /root/.ssh/id_rsa
      chmod 600 /root/.ssh/id_rsa
      echo '#{public_key}'  > /root/.ssh/id_rsa.pub
      chmod 600 /root/.ssh/id_rsa.pub
      echo '#{public_key}'  > /root/.ssh/authorized_keys
      chmod 600 /root/.ssh/authorized_keys
    echo '==== Installation de Python'
      apt-get update
      apt install -y python3-virtualenv sshpass
     ", privileged: true

  config.vm.define "manager" do |manager|
    manager.vm.provision :shell, :inline =>"
      echo '==== Simulation sommaire de DNS'
      echo '#{hosts_file}' > /etc/hosts
      ", privileged: true    
    manager.vm.hostname = "manager01"
    manager.vm.network "private_network", ip: "192.168.33.10"
  end

  (1..NODE_COUNT).each do |node_index|
		config.vm.define "node#{node_index}" do |node|
		  node.vm.hostname = "node-#{node_index}"
		  node.vm.network  :private_network, ip: "192.168.33.#{node_index + 10}"
		end
	end

end
