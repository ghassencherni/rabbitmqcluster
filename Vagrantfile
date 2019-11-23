VAGRANTFILE_API_VERSION = "2"
 Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
 config.vm.box = "generic/centos7"
 config.vm.hostname = "Node1"

 config.vm.provision "shell", inline: <<-SHELL
    sudo yum update
    yum -y install python2-pip
    sudo pip install --upgrade pip
    pip install docker-py
    sudo yum -y install docker
     
    #Open mysql master and slave ports
    firewall-cmd --zone=public --add-port=15672/tcp --permanent
    firewall-cmd --reload
    
    SHELL

 config.vm.provision :ansible do |ansible|

 ansible.playbook = "deploy-rabbitmq-cluster.yml"
 ansible.inventory_path= "hosts"
 ansible.limit = 'all'
 
 
 ansible.sudo = true
 #ansible.extra_vars = { ansible_ssh_user: 'vagrant' }
 #ansible.raw_arguments  = "--private-key=~/.vagrant.d/insecure_private_key"
 ansible.host_key_checking = false

 # using a specific IP.
 config.vm.network :private_network, ip: "10.0.0.2"
 
 # Port forwording for rabbitmq master
 config.vm.network "forwarded_port", guest: 15672, host: 15672

 config.vm.provider :virtualbox do |vb|

 vb.customize ["modifyvm", :id, "--memory", "2048"]

 # these 2 commands massively speed up DNS resolution, which means outbound
 # connections don't take forever (eg the WP admin dashboard and update page)
 vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
 vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
 
 end
 end
 end
