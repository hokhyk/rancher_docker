sudo apt-get install     apt-transport-https     ca-certificates     curl     software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo apt-key fingerprint 0EBFCD88
lsb_release -cs


 sudo add-apt-repository    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
	
sudo apt-get update
ok@ciserver:/mnt/data/software$ apt-cache madison docker-ce
sudo apt-get install docker-ce=17.03.2~ce-0~ubuntu-xenial
sudo docker run hello-world

sudo netstat -tunlp
vmware-hostd stop
sudo gpasswd -a hok docker
newgrp - docker

mysql -uroot --socket=/tmp/akonadi-vagrant.HRCVLU/mysql.socket

> CREATE DATABASE IF NOT EXISTS cattle COLLATE = 'utf8_general_ci' CHARACTER SET = 'utf8';
> GRANT ALL ON cattle.* TO 'cattle'@'%' IDENTIFIED BY 'cattle';
> GRANT ALL ON cattle.* TO 'cattle'@'localhost' IDENTIFIED BY 'cattle';


# rancher2.0
docker run -d --restart=unless-stopped \
  -p 80:80 -p 44:443 -p 22:22\
  --db-host 192.168.0.60 --db-port 3306 --db-user cattle --db-pass cattle --db-name cattle \
  rancher/rancher:v1.6
  
docker ps

# rancher1.6:
docker run -d --restart=unless-stopped --name rancher16server  -p 8080:80 -p 4430:443 -p 2200:22  rancher/server:stable  --db-host 192.168.0.60 --db-port 3306 --db-user cattle --db-pass cattle --db-name cattle

docker ps

# docker run -d -p 80:80 -p 443:443 rancher/rancher:stable


etcd:
sudo docker run -d --privileged --restart=unless-stopped --net=host -v /etc/kubernetes:/etc/kubernetes -v /var/run:/var/run rancher/rancher-agent:v2.0.4 --server https://192.168.0.60 --token 5jq66h77njz6rn6tdxd9qpj7n8cwzb9cwhzrzsdx7vdhq5clh77r7q --ca-checksum 143e41d423798a49e464aa85bf21f8f8fc9df22ec2ce453cf4617a8c0075c006 --address 192.168.0.61 --internal-address 192.168.0.61 --etcd --controlplane

control:
sudo docker run -d --privileged --restart=unless-stopped --net=host -v /etc/kubernetes:/etc/kubernetes -v /var/run:/var/run rancher/rancher-agent:v2.0.4 --server https://192.168.0.60 --token 5jq66h77njz6rn6tdxd9qpj7n8cwzb9cwhzrzsdx7vdhq5clh77r7q --ca-checksum 143e41d423798a49e464aa85bf21f8f8fc9df22ec2ce453cf4617a8c0075c006 --address 192.168.0.62 --internal-address 192.168.0.62 --controlplane


worker1:
sudo docker run -d --privileged --restart=unless-stopped --net=host -v /etc/kubernetes:/etc/kubernetes -v /var/run:/var/run rancher/rancher-agent:v2.0.4 --server https://192.168.0.60 --token 5jq66h77njz6rn6tdxd9qpj7n8cwzb9cwhzrzsdx7vdhq5clh77r7q --ca-checksum 143e41d423798a49e464aa85bf21f8f8fc9df22ec2ce453cf4617a8c0075c006 --address 192.168.0.63 --internal-address 192.168.0.63 --worker

