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

#   remove mysql-server and reinstall it:
        sudo apt-get remove mysql-server
        sudo service mysql stop  #or mysqld
        sudo killall -9 mysql
        sudo killall -9 mysqld
        sudo apt-get remove --purge mysql-server mysql-client mysql-common
        sudo deluser -f mysql
        sudo rm -rf /var/lib/mysql
        sudo apt-get purge mysql-server-core-5.7
        sudo apt-get purge mysql-client-core-5.7
        sudo rm -rf /var/log/mysql
        sudo rm -rf /etc/mysql
        
        sudo apt update
        sudo apt install mysql-server
        
        (Optional) Adjusting User Authentication and Privileges:
        sudo mysql
        mysql> SELECT user,authentication_string,plugin,host FROM mysql.user;
               ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';
               FLUSH PRIVILEGES;
               SELECT user,authentication_string,plugin,host FROM mysql.user;
               exit
        > CREATE DATABASE IF NOT EXISTS cattle COLLATE = 'utf8_general_ci' CHARACTER SET = 'utf8';
        > GRANT ALL ON cattle.* TO 'cattle'@'%' IDENTIFIED BY 'cattle';
        > GRANT ALL ON cattle.* TO 'cattle'@'localhost' IDENTIFIED BY 'cattle';

# docker exec -it pensive_newton /bin/bash
  If you don't know the IP address of the container, you can get it using the command below:
export INSTANCE_NAME="nginx-bg"
docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' $INSTANCE_NAME

# rancher2.0
docker run -d --restart=unless-stopped \
  -p 80:80 -p 44:443 -p 22:22\
  rancher/rancher:stable --db-host 192.168.0.60 --db-port 3306 --db-user cattle --db-pass cattle --db-name cattle

docker ps

# rancher1.6:
docker run -d --restart=unless-stopped --name rancher16server -p  8080:8080 rancher/server:stable  --db-host 192.168.0.60
docker ps

docker run -d -p 80:80 -p 443:443 rancher/rancher:stable


etcd:
sudo docker run -d --privileged --restart=unless-stopped --net=host -v /etc/kubernetes:/etc/kubernetes -v /var/run:/var/run rancher/rancher-agent:v2.0.4 --server https://192.168.0.60 --token 5jq66h77njz6rn6tdxd9qpj7n8cwzb9cwhzrzsdx7vdhq5clh77r7q --ca-checksum 143e41d423798a49e464aa85bf21f8f8fc9df22ec2ce453cf4617a8c0075c006 --address 192.168.0.61 --internal-address 192.168.0.61 --etcd --controlplane

control:
sudo docker run -d --privileged --restart=unless-stopped --net=host -v /etc/kubernetes:/etc/kubernetes -v /var/run:/var/run rancher/rancher-agent:v2.0.4 --server https://192.168.0.60 --token 5jq66h77njz6rn6tdxd9qpj7n8cwzb9cwhzrzsdx7vdhq5clh77r7q --ca-checksum 143e41d423798a49e464aa85bf21f8f8fc9df22ec2ce453cf4617a8c0075c006 --address 192.168.0.62 --internal-address 192.168.0.62 --controlplane


worker1:
sudo docker run -d --privileged --restart=unless-stopped --net=host -v /etc/kubernetes:/etc/kubernetes -v /var/run:/var/run rancher/rancher-agent:v2.0.4 --server https://192.168.0.60 --token 5jq66h77njz6rn6tdxd9qpj7n8cwzb9cwhzrzsdx7vdhq5clh77r7q --ca-checksum 143e41d423798a49e464aa85bf21f8f8fc9df22ec2ce453cf4617a8c0075c006 --address 192.168.0.63 --internal-address 192.168.0.63 --worker


# docker commands
查看镜像

docker images： 列出images
docker images -a ：列出所有的images（包含历史）
docker images --tree ：显示镜像的所有层(layer) ：已弃用
docker rmi <image ID>： 删除一个或多个image

[root@localhost docker]# docker images
REPOSITORY TAG IMAGE ID CREATED SIZE
docker.io/centos latest a8493f5f50ff 3 days ago 192.5 MB
[root@localhost docker]# docker images -a
REPOSITORY TAG IMAGE ID CREATED SIZE
docker.io/centos latest a8493f5f50ff 3 days ago 192.5 MB
[root@localhost docker]# docker images --tree
unknown flag: --tree
See 'docker images --help'.

 


使用镜像创建窗口与交互式运行
docker run docker.io/centos：创建容器，测试发现此种方式创建的容器不能启动（具体原因待验证）
docker run -i -t docker.io/centos /bin/bash：创建容器并交互式运行


[root@localhost docker]# docker run docker.io/centos /bin/echo hello docker --创建容器，并输出 hello docker
hello docker
[root@localhost docker]# docker run docker.io/centos --创建容器
[root@localhost docker]# docker run -i -t docker.io/centos /bin/bash --创建容器并交互式运行
[root@e9a66fa63cbf /]# ls
anaconda-post.log bin dev etc home lib lib64 lost+found media mnt opt proc root run sbin srv sys tmp usr var
[root@e9a66fa63cbf /]#

 

查看容器

docker ps ：列出当前所有正在运行的container
docker ps -l ：列出最近一次启动的container
docker ps -a ：列出所有的container（包含历史，即运行过的container）
docker ps -q ：列出最近一次运行的container ID

[root@localhost frinder]# docker ps
CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES
[root@localhost frinder]# docker ps -l
CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES
e9a66fa63cbf docker.io/centos "/bin/bash" 2 minutes ago Exited (0) About a minute ago tiny_lamport
[root@localhost frinder]# docker ps -a
CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES
e9a66fa63cbf docker.io/centos "/bin/bash" 2 minutes ago Exited (0) About a minute ago tiny_lamport
b56b4960d31f docker.io/centos "/bin/bash" 5 minutes ago Exited (0) 5 minutes ago amazing_colden
23ccaa5b07c2 docker.io/centos "/bin/echo hello dock" 7 minutes ago Exited (0) 7 minutes ago cocky_tesla
54d23ded3cd1 docker.io/centos "/bin/bash" 32 minutes ago Exited (0) 30 minutes ago trusting_euclid
[root@localhost frinder]# docker ps -q
[root@localhost frinder]#

 

 

 

再次启动容器
docker start/stop/restart <container> ：开启/停止/重启container
docker start [container_id] ：再次运行某个container （包括历史container）
docker attach [container_id] ：连接一个正在运行的container实例（即实例必须为start状态，可以多个窗口同时attach 一个container实例）
docker start -i <container> ：启动一个container并进入交互模式（相当于先start，在attach）
docker run -i -t <image> /bin/bash ：使用image创建container并进入交互模式, login shell是/bin/bash
docker run -i -t -p <host_port:contain_port> ：映射 HOST 端口到容器，方便外部访问容器内服务，host_port 可以省略，省略表示把 container_port 映射到一个动态端口。
注：使用start是启动已经创建过得container，使用run则通过image开启一个新的container。


[root@localhost docker]# docker start e9a66fa63cbf
e9a66fa63cbf
[root@localhost docker]# docker attach e9a66fa63cbf
[root@e9a66fa63cbf /]# exit
exit
[root@localhost docker]# docker restart e9a66fa63cbf
e9a66fa63cbf
[root@localhost docker]# docker attach e9a66fa63cbf
[root@e9a66fa63cbf /]# exit
exit
[root@localhost docker]# docker start -i e9a66fa63cbf
[root@e9a66fa63cbf /]# exit
exit
[root@localhost docker]#

 

 

删除容器
docker rm <container...> ：删除一个或多个container
docker rm `docker ps -a -q` ：删除所有的container
docker ps -a -q | xargs docker rm ：同上, 删除所有的container


[root@localhost docker]# docker rm b56b4960d31f
b56b4960d31f
[root@localhost docker]#

 

 

 

持久化容器与镜像


通过容器生成新的镜像

docker commit <container-id> <image-name>：把一个容器转变为一个新的镜像

[root@localhost docker]# docker commit 54d23ded3cd1 test-image
sha256:6104a9ded2f03269e20f58d02b56221c02d72adf0eeca3257c2a92d78a01b8ee
[root@localhost frinder]# docker images
REPOSITORY TAG IMAGE ID CREATED SIZE
test-image latest 6104a9ded2f0 19 seconds ago 192.5 MB
docker.io/centos latest a8493f5f50ff 3 days ago 192.5 MB

 

持久化容器
export命令用于持久化容器
docker export <CONTAINER ID> > /tmp/export.tar

[root@localhost docker]# docker export e9a66fa63cbf > /tmp/test.tar
[root@localhost docker]# ls /tmp/
anaconda.log ks-script-Ig5NMv storage.log test.tar
[root@localhost docker]#

 

持久化镜像
save命令用于持久化镜像
docker save 镜像ID > /tmp/save.tar

[root@localhost docker]# docker save test-image > /tmp/i-test.tar
[root@localhost docker]# ll /tmp
总用量 390960
-rw-r--r--. 1 root root 1862 4月 10 2017 anaconda.log
-rw-r--r--. 1 root root 200103424 4月 10 18:14 c-test.tar
-rw-r--r--. 1 root root 200116224 4月 10 18:16 i-test.tar

 

导入持久化container（将 container导入成 image）
[root@localhost docker]# cat /tmp/c-test.tar |docker import - export:latest

[root@localhost docker]# docker ps -a
CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES
e9a66fa63cbf docker.io/centos "/bin/bash" 31 minutes ago Exited (0) 18 minutes ago tiny_lamport
23ccaa5b07c2 docker.io/centos "/bin/echo hello dock" 36 minutes ago Exited (0) 21 minutes ago cocky_tesla
54d23ded3cd1 docker.io/centos "/bin/bash" About an hour ago Exited (0) 17 minutes ago trusting_euclid
[root@localhost docker]# docker rm e9a66fa63cbf
e9a66fa63cbf
[root@localhost docker]# docker ps -a
CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES
23ccaa5b07c2 docker.io/centos "/bin/echo hello dock" 36 minutes ago Exited (0) 21 minutes ago cocky_tesla
54d23ded3cd1 docker.io/centos "/bin/bash" About an hour ago Exited (0) 17 minutes ago trusting_euclid
[root@localhost docker]# cat /tmp/c-test.tar |docker import - export:latest
sha256:e65811fb72d82ed380eb3e138ee693b40074726c758f80193d17a4a01d8f9108
[root@localhost docker]# docker ps -a
CONTAINER ID IMAGE COMMAND CREATED STATUS PORTS NAMES
23ccaa5b07c2 docker.io/centos "/bin/echo hello dock" 37 minutes ago Exited (0) 22 minutes ago cocky_tesla
54d23ded3cd1 docker.io/centos "/bin/bash" About an hour ago Exited (0) 18 minutes ago trusting_euclid
[root@localhost docker]# docker images
REPOSITORY TAG IMAGE ID CREATED SIZE
export latest e65811fb72d8 48 seconds ago 192.5 MB
test-image latest 6104a9ded2f0 15 minutes ago 192.5 MB
docker.io/centos latest a8493f5f50ff 3 days ago 192.5 MB

 


导入持久化image
[root@localhost docker]# docker load < /tmp/i-test.tar

[root@localhost docker]# docker images
REPOSITORY TAG IMAGE ID CREATED SIZE
export latest e65811fb72d8 48 seconds ago 192.5 MB
test-image latest 6104a9ded2f0 15 minutes ago 192.5 MB
docker.io/centos latest a8493f5f50ff 3 days ago 192.5 MB
[root@localhost docker]# docker rmi test-image
Untagged: test-image:latest
Deleted: sha256:6104a9ded2f03269e20f58d02b56221c02d72adf0eeca3257c2a92d78a01b8ee
Deleted: sha256:287c689f3983b30f642dab57f59ec5a21aa817e1aaf1d37ea63207a7a8cd0a50
[root@localhost docker]# docker images
REPOSITORY TAG IMAGE ID CREATED SIZE
export latest e65811fb72d8 About a minute ago 192.5 MB
docker.io/centos latest a8493f5f50ff 3 days ago 192.5 MB
[root@localhost docker]# docker load < /tmp/i-test.tar
496bc0b12baf: Loading layer [==================================================>] 3.584 kB/3.584 kB
Loaded image: test-image:latest=====> ] 512 B/3.584 kB
[root@localhost docker]# docker images
REPOSITORY TAG IMAGE ID CREATED SIZE
export latest e65811fb72d8 5 minutes ago 192.5 MB
test-image latest 6104a9ded2f0 19 minutes ago 192.5 MB
docker.io/centos latest a8493f5f50ff 3 days ago 192.5 MB
[root@localhost docker]#

 


修改 image tag：
[root@localhost docker]# docker tag test-image load:my.image

[root@localhost docker]# docker images
REPOSITORY TAG IMAGE ID CREATED SIZE
export latest e65811fb72d8 5 minutes ago 192.5 MB
test-image latest 6104a9ded2f0 19 minutes ago 192.5 MB
docker.io/centos latest a8493f5f50ff 3 days ago 192.5 MB
[root@localhost docker]# docker tag test-image load:my.image
[root@localhost docker]# docker images
REPOSITORY TAG IMAGE ID CREATED SIZE
export latest e65811fb72d8 7 minutes ago 192.5 MB
load my.image 6104a9ded2f0 22 minutes ago 192.5 MB
test-image latest 6104a9ded2f0 22 minutes ago 192.5 MB
docker.io/centos latest a8493f5f50ff 3 days ago 192.5 MB
[root@localhost docker]#

 


export-import与save-load的区别

导出后再导入(export-import)的镜像会丢失所有的历史，而保存后再加载（save-load）的镜像没有丢失历史和层(layer)。这意味着使用导出后再导入的方式，你将无法回滚到之前的层(layer)，同时，使用保存后再加载的方式持久化整个镜像，就可以做到层回滚。（可以执行docker tag <LAYER ID> <IMAGE NAME>来回滚之前的层）。

 

 

一些其它命令

docker logs $CONTAINER_ID #查看docker实例运行日志，确保正常运行
docker inspect $CONTAINER_ID #docker inspect <image|container> 查看image或container的底层信息
docker build <path> 寻找path路径下名为的Dockerfile的配置文件，使用此配置生成新的image
docker build -t repo[:tag] 同上，可以指定repo和可选的tag
docker build - < <dockerfile> 使用指定的dockerfile配置文件，docker以stdin方式获取内容，使用此配置生成新的image
docker port <container> <container port> 查看本地哪个端口映射到container的指定端口，其实用docker ps 也可以看到

 


一些使用技巧

docker文件存放目录
Docker实际上把所有东西都放到/var/lib/docker路径下了。

[root@localhost docker]# ls -F
containers/ devicemapper/ execdriver/ graph/ init/ linkgraph.db repositories-devicemapper volumes/

containers目录当然就是存放容器（container）了，graph目录存放镜像，文件层（file system layer）存放在graph/imageid/layer路径下，这样我们就可以看看文件层里到底有哪些东西，利用这种层级结构可以清楚的看到文件层是如何一层一层叠加起来的。
