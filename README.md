# DevSecOps-Bms
BMS

Step 1: AWS Console Login And EC2 Setup

Sign Into AWS Console
Start an EC2 Instance (Ubuntu, t2.large, 16Gib volume)
Create a Key-value pair (pem file in case of linus/mac)
chmod 400 "bms.pem"
ssh -i "bms.pem" ubuntu@ec2-3-89-84-25.compute-1.amazonaws.com  # signing into ec2 instance

Connection with EC2 instance Established

Step 2: 
sudo apt update # Update the package manager

Install AWS Cli

sudo apt install unzip -y
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

Install Jenkins on Ubuntu

#!/bin/bash
sudo apt update -y
wget -O - https://packages.adoptium.net/artifactory/api/gpg/key/public | sudo tee /etc/apt/keyrings/adoptium.asc
echo "deb [signed-by=/etc/apt/keyrings/adoptium.asc] https://packages.adoptium.net/artifactory/deb $(awk -F= '/^VERSION_CODENAME/{print$2}' /etc/os-release) main" | sudo tee /etc/apt/sources.list.d/adoptium.list
sudo apt update -y
sudo apt install temurin-17-jdk -y
/usr/bin/java --version
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update -y
sudo apt-get install jenkins -y
sudo systemctl start jenkins
sudo systemctl status jenkins

Install Docker on Ubuntu
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
sudo usermod -aG docker ubuntu
sudo chmod 777 /var/run/docker.sock
newgrp docker
sudo systemctl status docker

Install Trivy on Ubuntu
sudo apt-get install wget apt-transport-https gnupg
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | gpg --dearmor | sudo tee /usr/share/keyrings/trivy.gpg > /dev/null
echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb generic main" | sudo tee -a /etc/apt/sources.list.d/trivy.list
sudo apt-get update
sudo apt-get install trivy


docker login    -u   `Give Dockerhub credentials here`
curl -sSfL https://raw.githubusercontent.com/docker/scout-cli/main/install.sh | sh -s -- -b /usr/local/bin


Configure Security Groups:
Open ports: 9000,3000,443,80,9090,9100,8080

Open Jenkins at 3.89.84.25:8080
Run Sonarqube:  docker run -d --name sonar -p 9000:9000 sonarqube:lts-community

docker ps

Jenkins - It is an automation server tool help automate parts of software development like building, testing, and deployment. It is for Continuous Integration & Continuous deployment

For Jenkins - sudo cat /var/lib/jenkins/secrets/initialAdminPassword
Install Plugins - Assign Username and Password
Jenkins  Url - http://3.89.84.25:8080/

Jenkins Plugins
Eclipse Temurin installerVersion 1.7
SonarQube ScannerVersion 2.18
NodeJSVersion 1.6.4
OWASP Dependency-CheckVersion 5.6.0
Docker Version 1274.vc0203fdf2e74
Docker Commons Version 451.vd12c371eeeb_3
Docker Pipeline
Docker API
docker-build-step
Pipeline: Stage View
Pipeline Aggregator View
Prometheus metrics
Email Extension TemplateVersion

Go to Sonarqube -> administration -> users -> generate token to interact with jenkins
Login to Jenkins - Configure the tools
Install tools - Jdk,nodejs, sonarcube scanner, dependency check,docker
add sonar token to jenkins
Add 3 credentials in jenkins - sonar cred (seret text), docker cred, mail cred

Manage jenkins - System Tools
Handle SMPT -smptp.gmail.com
Handle Notification
Handle Default Trigger
Handle Sonar Qube

