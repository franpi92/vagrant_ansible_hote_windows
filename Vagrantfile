# -*- mode: ruby -*-
# vi: set ft=ruby :
NODE_COUNT = 2

Vagrant.configure("2") do |config|
 
  config.vm.box = "generic/rhel8"
  private_key = File.read("id_rsa")
  public_key = File.read("id_rsa.pub")
  config.vm.provision :shell, :inline =>"
     echo 'Copie des cles root'
     mkdir -p /root/.ssh
     chmod 700 /root/.ssh
     echo '#{private_key}' > /root/.ssh/id_rsa
     chmod 600 /root/.ssh/id_rsa
     echo '#{public_key}'  > /root/.ssh/id_rsa.pub
     chmod 600 /root/.ssh/id_rsa.pub
     echo '#{public_key}'  > /root/.ssh/authorized_keys
     chmod 600 /root/.ssh/authorized_keys
     ", privileged: true

  config.vm.define "manager" do |manager|
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
