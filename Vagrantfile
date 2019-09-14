# -*- mode: ruby -*-
# vi: set ft=ruby :

network = '192.168.0'	

nodes = [
{ :hostname => 'server1', :ip => "#{network}.101", :ram => 2048 },
{ :hostname => 'server2', :ip => "#{network}.102", :ram => 2048 },
{ :hostname => 'server3', :ip => "#{network}.103", :ram => 2048 }]

Vagrant.configure("2") do |config|
	nodes.each do |node|
		config.vm.define node[:hostname] do |nodeconfig|
			nodeconfig.vm.box = "centos/7"
                        nodeconfig.vm.box_version = "1803.01"
			nodeconfig.vm.hostname = node[:hostname]
			nodeconfig.vm.network :private_network, ip: node[:ip]
			memory = node[:ram] ? node[:ram] : 512;
			cpu = node[:cpu] ? node[:cpu] : 1;

	   public_key = File.read("id_rsa.pub")
	   config.vm.provision "shell", inline: <<-SCRIPT
        	chmod 600 /home/vagrant/.ssh/is_rsa
        	echo 'Copying ansible-vm public SSH Keys to the VM'
        	#mkdir -p /home/vagrant/.ssh
        	chmod 700 /home/vagrant/.ssh
        	echo '#{public_key}' >> /home/vagrant/.ssh/authorized_keys
       		chmod -R 600 /home/vagrant/.ssh/authorized_keys
        	echo 'Host 192.168.*.*' >> /home/vagrant/.ssh/config
        	echo 'StrictHostKeyChecking no' >> /home/vagrant/.ssh/config
        	echo 'UserKnownHostsFile /dev/null' >> /home/vagrant/.ssh/config
        	chmod -R 600 /home/vagrant/.ssh/config
        	SCRIPT


		nodeconfig.vm.provider :virtualbox do |vb|
		vb.customize [ "modifyvm", :id, "--memory", memory.to_s,]
		end
	end
end
end
