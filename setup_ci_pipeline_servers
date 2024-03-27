#! /bin/bash

sudo yum update -y
sudo systemctl stop firewalld
sudo setsebool httpd_can_network_connect 1

sudo hostnamectl set-hostname app-server

# Add the user DevOps
sudo useradd devops

# set password : the below command will avoid re entering the password
echo "devops" | passwd --stdin devops


# modify the sudoers file at /etc/sudoers and add entry
echo 'devops     ALL=(ALL)      NOPASSWD: ALL' | sudo tee -a /etc/sudoers


# this command is to add an entry to file : echo 'PasswordAuthentication yes' | sudo tee -a /etc/ssh/sshd_config
# the below sed command will find and replace words with spaces "PasswordAuthentication no" to "PasswordAuthentication yes"
sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config
sudo service sshd restart

sudo yum install tree wget zip unzip gzip vim net-tools git bind-utils python2-pip jq -y

sudo su - devops -c "git config --global user.name 'devops'"
sudo su - devops -c "git config --global user.email 'devops@gmail.com'"


sudo chown -R devops:devops /opt

# install MVN in OPT

cd /opt


# tomcat installation(deployment server)

wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.85/bin/apache-tomcat-9.0.85.tar.gz

tar -xzvf apache-tomcat-9.0.85.tar.gz
mv apache-tomcat-9.0.85 appserver
rm -rf apache-tomcat-9.0.85.tar.gzr



# tomcat as a service(register)

sudo chown -R devops:devops /opt

 echo '[Unit]

        Description=Tomcat Server
        After=syslog.target network.target

       [Service]
        Type=forking
        User=devops
        Group=devops

        Environment=JAVA_HOME=/usr/lib/jvm/java-11-openjdk-11.0.22.0.7-1.el7_9.x86_64
        Environment='JAVA_OPTS=-Djava.awt.headless=true'
        Environment=CATALINA_HOME=/opt/appserver/
        Environment=CATALINA_BASE=/opt/appserver/
        Environment=CATALINA_PID=/opt/appserver/temp/tomcat.pid
        Environment='CATALINA_OPTS=-Xms512M'
        ExecStart=/opt/appserver/bin/catalina.sh start
        ExecStop=/opt/appserver/bin/catalina.sh stop

        [Install]
        WantedBy=multi-user.target' > /etc/systemd/system/tomcat.service

sudo systemctl daemon-reload
sudo systemctl start tomcat
sudo systemctl enable tomcat

# nginx setup
 sudo yum install epel-release
 sudo yum install nginx
 sudo systemctl start nginx
 sudo systemctl status nginx
 sudo systemctl enable nginx


#Maria DB setup

sudo yum install mariadb-server.x86_64 -y
systemctl enable mariadb
systemctl start mariadb
systemctl status mariadb



# git clone https://gitlab.com/raghavabandaru854/student-app.git
 git clone https://gitlab.com/raghavabandaru854/static-project.git
# sudo yum install java-1.8.0-openjdk-devel.x86_64 -y


