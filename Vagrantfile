$script_ansible = <<-SCRIPT
apt-get update
apt-get -y install software-properties-common
apt-add-repository --yes --update ppa:ansible/ansible
apt-get install -y ansible
apt-get install python-apt
SCRIPT


Vagrant.configure("2") do |config|
  
	config.vm.define "Apache-Server" do |svapache|
    		svapache.vm.synced_folder ".", "/vagrant", disabled: true
    		svapache.vm.synced_folder "./Ansible", "/vagrant"
    		svapache.vm.box = "ubuntu/bionic64"

		svapache.vm.network "forwarded_port", guest: 80, host: 8080
		svapache.vm.network "public_network", ip: "192.168.1.30"

		svapache.vm.provision "shell", inline: $script_ansible
    
    		svapache.vm.provision "shell", inline: "cat /vagrant/id_rsa.pub >> .ssh/authorized_keys"
		svapache.vm.provision "shell", inline: "cp /vagrant/ansible.cfg /etc/ansible/"

		svapache.vm.provision "shell", inline: "ansible-playbook -i /vagrant/hosts.yaml /vagrant/playbook.yml"

  	end

	config.vm.define "Mysql-Server" do |svmysql|
		svmysql.vm.synced_folder ".", "/vagrant", disabled: true
		svmysql.vm.synced_folder "./Ansible", "/vagrant"
		svmysql.vm.box = "ubuntu/bionic64"

		svmysql.vm.network "forwarded_port", guest: 80, host: 3306
		svmysql.vm.network "public_network", ip: "192.168.1.31"

		svmysql.vm.provision "shell", inline: $script_ansible
    
    		svmysql.vm.provision "shell", inline: "cat /vagrant/id_rsa.pub >> .ssh/authorized_keys"
		svmysql.vm.provision "shell", inline: "cp /vagrant/ansible.cfg /etc/ansible/"

		svmysql.vm.provision "shell", inline: "ansible-playbook -i /vagrant/hosts.yaml /vagrant/playbook2.yml"
    
	end

  	
end