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
  3. Download the repo or use git to clone in your home directory: git clone https://github.com/ghassencherni/mysqlcluster.git
  4. Add the entry “10.0.0.3 Node2” in your hosts file ( /etc/hosts for linux , C:\Windows\System32\drivers\etc\hosts for windows10 )
  5. Move to the downloaded repo : cd mysqlcluster
  6. Run the command “vagrant up” : this will run a VM with master and slave msyql containers deployed by Ansible

> Note: 
The master mysql  runs on port 3307 and the slave on 3308.
The IP address of the created VM is “10.0.0.3” you can change it in the Vagrantfile if needed.
 
## How to test the replication:
Connect to mysql master from your host and using the root:
   1. Run the command:
mysql --protocol=tcp --port=3307 --user=root -pghassen
    
    2. Create new database “test”:
mysql> create database mydatabase;

    3. Now connect to the slave server :
mysql --protocol=tcp --port=3308 --user=root -pghassen

    4. Check that “mydatabase” is created on the slave node:
mysql> show databases;

> Note: This VM uses 2 GB of RAM by default, you might want to manually change the memory requirements to a higher value (the `v.memory` parameter in the `Vagrantfile`).

## Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):



Adding new Bridge network configuration and use it with the cluster due to an issue with the default bridge 
 
    bridge_name: "mysql_bridge"
 
    bridge_subnet: "172.20.0.0/16"
 
    bridge_gateway: "172.20.0.1"



Container 1 & 2 ip address, 
 
    container_1_ip: "172.20.0.2"  used for the master
 
    container_2_ip: "172.20.0.3"  used for the slave



Containers names ( used as tag for templates )
    
    master_name: "my-master"
    
    slave_name: "my-slave"



The default password for the root MYSQL, used to configure the replication and to test the deployement 
    ROOT_PASS: "ghassen" 

## Author Information

This script  was created by [Ghassen CHARNI](https://github.com/ghassencherni/)
