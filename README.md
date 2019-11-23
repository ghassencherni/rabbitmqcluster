# rabbitmqcluster
Ansible Playbook to deploy RabbitMQ cluster in docker containers launched with Vagrant.
The RabbitMQ cliuster is based on one master and two slaves

The cluster configuration based on this document: https://www.rabbitmq.com/clustering.html


## Requirements

- Vagrant and Virtualbox must be installed on your host ( physical host as Node are build from 64bit image ) 

## Getting Started

This deployement script use the role ansible-role-docker created by [githubixx](https://github.com/githubixx) in order to install Docker components on the Nodes.

To run mysql in master and slave replication :

  1. Download and Install [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
  2. Download and Install [Vagrant](https://www.vagrantup.com/downloads.html)
  3. Download the repo or use git to clone in your home directory: git clone https://github.com/ghassencherni/rabbitmqcluster.git
  4. Add the entry “10.0.0.2 Node1” in your hosts file ( /etc/hosts for linux , C:\Windows\System32\drivers\etc\hosts for windows10 )
  5. Move to the downloaded repo : cd rabbitmqcluster
  6. Run the command “vagrant up” : this will run a VM with 3 rabbitmq instances containers, one master and 2 slaves deployed by Ansible

> Note: 
The admin interface of the rabitmq master run isis accessible on port 15672, you can modify this port on the Vagranfile and use port 80 instead for example.
The IP address of the created VM is “10.0.0.2” you can change it in the Vagrantfile if needed.
 
## How to test the deploy :

   1. Open the URL : http://node1:15672/ in the browser
   2. Put the user: admin and the password : "pasword" (default values)
   If every things is ok, you get the dashboard with three nodes as shown below:
   (https://www.howtoforge.com/images/how_to_set_up_rabbitmq_cluster_on_ubuntu_1804_lts/big/9.png)

> Note: This VM uses 2 GB of RAM by default, you might want to manually change the memory requirements to a higher value (the `v.memory` parameter in the `Vagrantfile`).

## Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):


Adding new Bridge network configuration and use it with the cluster due to an issue with the default bridge 
    
    bridge_name: "rabbitmq_bridge"
    
    bridge_subnet: "172.18.0.0/16"
    
    bridge_gateway: "172.18.0.1"



Containers IP address configuration

    container_1_ip: "172.18.0.2"

    container_2_ip: "172.18.0.3"

    container_3_ip: "172.18.0.4"



Containers names, used as tag for templates and in the cluster configuration to allow instances to know each others

    container_1_name: "rabbit1"

    container_2_name: "rabbit2"

    container_3_name: "rabbit3"



secret string shared between the three containers
    
    ERLANG_COOKIE: "12345"
    
    

User and password of the master admin interface

    DEFAULT_USER: "admin"
    
    DEFAULT_PASS: "password"



## Author Information

This script  was created by [Ghassen CHARNI](https://github.com/ghassencherni/)
