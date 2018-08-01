Creating the playbook for installing tomcat
------------------------------------------
sudo apt-get update
sudo apt-get install default-jdk
sudo groupadd tomcat
sudo useradd -s /bin/false -g tomcat -d /opt/tomcat tomcat
sudo mkdir /opt/tomcat
wget http://www-us.apache.org/dist/tomcat/tomcat-8/v8.5.32/bin/apache-tomcat-8.5.32.tar.gz 
tar xzf apache-tomcat-8.5.32.tar.gz -C /opt/tomcat/
cd /opt/tomcat/apache-tomcat-8.5.32
sudo chgrp -R tomcat /opt/tomcat/apache-tomcat-8.5.32/
sudo chmod -R g+r conf
sudo chmod g+x conf
chown -R tomcat webapps/ work/ temp/ logs/
 update-java-alternatives -l

#java- output    1.8.0-openjdk-amd64       1081       /usr/lib/jvm/java-1.8.0-openjdk-amd64
append jre at the end of  /usr/lib/jvm/java-1.8.0-openjdk-amd64

vi /etc/systemd/system/tomcat.service
------------------------------------------------------------------
[Unit]
Description=Apache Tomcat Web Application Container
After=network.target

[Service]
Type=forking

Environment=JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-amd64
Environment=CATALINA_PID=/opt/tomcat/temp/tomcat.pid
Environment=CATALINA_HOME=/opt/tomcat/apache-tomcat-8.5.32
Environment=CATALINA_BASE=/opt/tomcat/apache-tomcat-8.5.32
Environment='CATALINA_OPTS=-Xms512M -Xmx1024M -server -XX:+UseParallelGC'
Environment='JAVA_OPTS=-Djava.awt.headless=true -Djava.security.egd=file:/dev/./urandom'

ExecStart=/opt/tomcat/apache-tomcat-8.5.32/bin/startup.sh
ExecStop=/opt/tomcat/apache-tomcat-8.5.32/bin/shutdown.sh

User=tomcat
Group=tomcat
UMask=0007
RestartSec=10
Restart=always

[Install]
WantedBy=multi-user.target

-------------------------------------
systemctl daemon-reload

systemctl start tomcat