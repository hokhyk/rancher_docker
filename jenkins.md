# jenkins installation
## java 8 instalation on ubuntu:
  sudo apt-get install openjdk-8-jdk-headless

## jenkins installation: 
    wget -q -O - https://pkg.jenkins.io/debian/jenkins.io.key | sudo apt-key add -
    sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ >
    /etc/apt/sources.list.d/jenkins.list'
    sudo apt-get update
    sudo apt-get install jenkins

# jenkins setup
## visit 192.168.0.81:8080   and input the password
  sudo cat /var/lib/jenkins/secrets/initialAdminPassword
  This pasword is all used as jenkins  default admin password.  admin/XXXXXX(initialAdminPassword)

## create first admin user
    vagrant/vagrant    email: hokhyk@aliyun.com

# start using jenkins
