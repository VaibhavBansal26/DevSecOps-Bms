# DevSecOps-Bms


![dEVOPs](https://res.cloudinary.com/vaibhav-codexpress/image/upload/v1741824030/Screenshot_2025-03-12_at_7.57.18_PM_uiktvg.png)

**1. GitHub → Jenkins → SonarQube → Docker Build :**
    Code is pulled from GitHub, built by Jenkins, analyzed by SonarQube, and containerized via Docker.

**2. Argo CD, Helm, and EKS :**
    Argo CD (GitOps) or Helm charts are used to deploy and manage your application on Amazon EKS.

**3. Monitoring (Prometheus + Grafana) :**
    Prometheus collects metrics, and Grafana visualizes them.

**4. CloudFormation :**
    Used to provision and manage AWS infrastructure (e.g., the EKS cluster, networking, IAM roles, etc.).

# Jenkins
![Jenkins](https://res.cloudinary.com/vaibhav-codexpress/image/upload/v1741820659/Screenshot_2025-03-12_at_3.30.17_PM_rnphpr.png)

![Jenkins 1](https://res.cloudinary.com/vaibhav-codexpress/image/upload/v1741820659/Screenshot_2025-03-12_at_3.30.28_PM_oefslg.png)

# Sonar Qube
![Sonar Qube](https://res.cloudinary.com/vaibhav-codexpress/image/upload/v1741820661/Screenshot_2025-03-12_at_3.31.25_PM_elf0u3.png)

# Amazon EKS Dashboard
![Amazon EKS Dashboard](https://res.cloudinary.com/vaibhav-codexpress/image/upload/v1741820659/Screenshot_2025-03-12_at_3.32.32_PM_u4us9u.png)

![eks cluster](https://res.cloudinary.com/vaibhav-codexpress/image/upload/v1741820659/Screenshot_2025-03-12_at_3.32.14_PM_tm78fo.png)

# Prometheus
![Prometheus](https://res.cloudinary.com/vaibhav-codexpress/image/upload/v1741820661/Screenshot_2025-03-12_at_3.30.55_PM_medotr.png)

# Grafana
![Grafana](https://res.cloudinary.com/vaibhav-codexpress/image/upload/v1741820659/Screenshot_2025-03-12_at_3.29.51_PM_cfymkq.png)

![Grafana 1](https://res.cloudinary.com/vaibhav-codexpress/image/upload/v1741820661/Screenshot_2025-03-12_at_3.29.27_PM_b4ibmu.png)

# Agro CD
![Agro Cd](https://res.cloudinary.com/vaibhav-codexpress/image/upload/v1741820661/Screenshot_2025-03-12_at_3.22.27_PM_rgjwtq.png)

# Instance
![EC2](https://res.cloudinary.com/vaibhav-codexpress/image/upload/v1741821857/Screenshot_2025-03-12_at_7.19.48_PM_hlky3t.png)

# Security Group & Ports
![SG](https://res.cloudinary.com/vaibhav-codexpress/image/upload/v1741821857/Screenshot_2025-03-12_at_7.19.27_PM_qlkl2k.png)

# Cloud Formation
![cf](https://res.cloudinary.com/vaibhav-codexpress/image/upload/v1741821856/Screenshot_2025-03-12_at_7.20.13_PM_mjufmq.png)


# Project Instructions


**Step 1: AWS Console Login And EC2 Setup**

Sign Into AWS Console

Start an EC2 Instance (Ubuntu, t2.large, 16Gib volume)

Create a Key-value pair (pem file in case of linus/mac)

```
chmod 400 "bms.pem"
ssh -i "bms.pem" ubuntu@ec2-3-89-84-25.compute-1.amazonaws.com  # signing into ec2 instance
```

Connection with EC2 instance Established

**Step 2: Installations & Updattions**

```
sudo apt update # Update the package manager
```

**Install AWS Cli**

```
sudo apt install unzip -y
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
```

**Install Jenkins on Ubuntu**

```
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
```

**Install Docker on Ubuntu**

**Add Docker's official GPG key:**

```
sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc
```
**Add the repository to Apt sources:**

```
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
```

**Install Trivy on Ubuntu**

```
sudo apt-get install wget apt-transport-https gnupg
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | gpg --dearmor | sudo tee /usr/share/keyrings/trivy.gpg > /dev/null
echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb generic main" | sudo tee -a /etc/apt/sources.list.d/trivy.list
sudo apt-get update
sudo apt-get install trivy
```

```
docker login    -u   `Give Dockerhub credentials here`
curl -sSfL https://raw.githubusercontent.com/docker/scout-cli/main/install.sh | sh -s -- -b /usr/local/bin
```

**Configure Security Groups:**

Open ports: 9000,3000,443,80,9090,9100,8080

Open Jenkins at 3.89.84.25:8080

```
Run Sonarqube:  docker run -d --name sonar -p 9000:9000 sonarqube:lts-community
```
```
docker ps
```

**Jenkins - It is an automation server tool help automate parts of software development like building, testing, and deployment. It is for Continuous Integration & Continuous deployment**

For Jenkins - sudo cat /var/lib/jenkins/secrets/initialAdminPassword

Install Plugins - Assign Username and Password

Jenkins  Url - http://3.89.84.25:8080/

1. Jenkins Plugins
2. Eclipse Temurin installerVersion 1.7
3. SonarQube ScannerVersion 2.18
4. NodeJSVersion 1.6.4
5. OWASP Dependency-CheckVersion 5.6.0
6. Docker Version 1274.vc0203fdf2e74
7. Docker Commons Version 451.vd12c371eeeb_3
8. Docker Pipeline
9. Docker API
10. docker-build-step
11. Pipeline: Stage View
12. Pipeline Aggregator View
13. Prometheus metrics
14. Email Extension TemplateVersion


Go to Sonarqube -> administration -> users -> generate token to interact with jenkins
1. Login to Jenkins - Configure the tools
2. Install tools - Jdk,nodejs, sonarcube scanner, dependency check,docker
add sonar token to jenkins

3. Add 3 credentials in jenkins - sonar cred (seret text), docker cred, mail cred


   Manage jenkins - System Tools
1. Handle SMPT -smptp.gmail.com
2. Handle Notification
3. Handle Default Trigger
4. Handle Sonar Qube

**Create Webhook in Sonarqube then build**

**Complete Process (Monitoring)**

=============================================

# Jenkins Complete Pipeline

```
pipeline {
    agent any
    tools {
        jdk 'jdk17'
        nodejs 'node-16'
    }
    environment {
        SCANNER_HOME=tool 'sonar-scanner'
    }
    stages {
        stage ("clean workspace") {
            steps {
                cleanWs()
            }
        }
        stage ("Git checkout") {
            steps {
                git branch: 'main', url: 'https://github.com/VaibhavBansal26/DevSecOps-Bms.git'
            }
        }
        stage("Sonarqube Analysis "){
            steps{
                withSonarQubeEnv('sonar-server') {
                    sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=bms-prime \
                    -Dsonar.projectKey=bms-prime '''
                }
            }
        }
        stage("quality gate"){
           steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'Sonar-token' 
                }
            } 
        }
        stage("Install NPM Dependencies") {
            steps {
                sh "npm install"
            }
        }
        stage("OWASP FS Scan") {
            steps {
                script {
                    dependencyCheck additionalArguments: '--scan ./ --disableYarnAudit --disableNodeAudit --format XML --out dependency-check-report', odcInstallation: 'DP-Check'
                }
                dependencyCheckPublisher pattern: '**/dependency-check-report/dependency-check-report.xml'
            }
        }
        stage("Verify Dependency Check Report") {
            steps {
                sh 'ls -R $WORKSPACE'
            }
        }
        stage ("Trivy File Scan") {
            steps {
                sh "trivy fs . > trivy.txt"
            }
        }
        stage ("Build Docker Image") {
            steps {
                sh "docker build -t bms-prime ."
            }
        }
        stage ("Tag & Push to DockerHub") {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred') {
                        sh "docker tag bms-prime vaibhavbansal26/bms-prime:latest "
                        sh "docker push vaibhavbansal26/bms-prime:latest "
                    }
                }
            }
        }
        stage('Docker Scout Image') {
            steps {
                script{
                   withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker'){
                       sh 'docker-scout quickview vaibhavbansal26/bms-prime:latest'
                       sh 'docker-scout cves vaibhavbansal26/bms-prime:latest'
                       sh 'docker-scout recommendations vaibhavbansal26/bms-prime:latest'
                   }
                }
            }
        }
        stage("Deploy to Container") {
            steps {
                script {
                    sh '''
                    if [ $(docker ps -q -f name=bms-prime) ]; then
                        echo "Stopping existing container..."
                        docker stop bms-prime
                    fi
        
                    if [ $(docker ps -aq -f name=bms-prime) ]; then
                        echo "Removing existing container..."
                        docker rm bms-prime
                    fi
        
                    echo "Deploying new container..."
                    docker run -d --name bms-prime -p 3000:3000 vaibhavbansal26/bms-prime:latest
                    '''
                }
            }
        }
    }
    post {
    always {
        emailext attachLog: true,
            subject: "'${currentBuild.result}'",
            body: """
                <html>
                <body>
                    <div style="background-color: #FFA07A; padding: 10px; margin-bottom: 10px;">
                        <p style="color: white; font-weight: bold;">Project: ${env.JOB_NAME}</p>
                    </div>
                    <div style="background-color: #90EE90; padding: 10px; margin-bottom: 10px;">
                        <p style="color: white; font-weight: bold;">Build Number: ${env.BUILD_NUMBER}</p>
                    </div>
                    <div style="background-color: #87CEEB; padding: 10px; margin-bottom: 10px;">
                        <p style="color: white; font-weight: bold;">URL: ${env.BUILD_URL}</p>
                    </div>
                </body>
                </html>
            """,
            to: 'thesimpleguy03@gmail.com',
            mimeType: 'text/html',
            attachmentsPattern: 'trivy.txt'
        }
    }
}

```

**Phase 4: Monitoring**

1. **Install Prometheus and Grafana:**

   Set up Prometheus and Grafana to monitor your application.

   **Installing Prometheus:**

   First, create a dedicated Linux user for Prometheus and download Prometheus:

   ```bash
   sudo useradd --system --no-create-home --shell /bin/false prometheus
   wget https://github.com/prometheus/prometheus/releases/download/v2.47.1/prometheus-2.47.1.linux-amd64.tar.gz
   ```

   Extract Prometheus files, move them, and create directories:

   ```bash
   tar -xvf prometheus-2.47.1.linux-amd64.tar.gz
   cd prometheus-2.47.1.linux-amd64/
   sudo mkdir -p /data /etc/prometheus
   sudo mv prometheus promtool /usr/local/bin/
   sudo mv consoles/ console_libraries/ /etc/prometheus/
   sudo mv prometheus.yml /etc/prometheus/prometheus.yml
   ```

   Set ownership for directories:

   ```bash
   sudo chown -R prometheus:prometheus /etc/prometheus/ /data/
   ```

   Create a systemd unit configuration file for Prometheus:

   ```bash
   sudo nano /etc/systemd/system/prometheus.service
   ```

   Add the following content to the `prometheus.service` file:

   ```plaintext
   [Unit]
   Description=Prometheus
   Wants=network-online.target
   After=network-online.target

   StartLimitIntervalSec=500
   StartLimitBurst=5

   [Service]
   User=prometheus
   Group=prometheus
   Type=simple
   Restart=on-failure
   RestartSec=5s
   ExecStart=/usr/local/bin/prometheus \
     --config.file=/etc/prometheus/prometheus.yml \
     --storage.tsdb.path=/data \
     --web.console.templates=/etc/prometheus/consoles \
     --web.console.libraries=/etc/prometheus/console_libraries \
     --web.listen-address=0.0.0.0:9090 \
     --web.enable-lifecycle

   [Install]
   WantedBy=multi-user.target
   ```

   Here's a brief explanation of the key parts in this `prometheus.service` file:

   - `User` and `Group` specify the Linux user and group under which Prometheus will run.

   - `ExecStart` is where you specify the Prometheus binary path, the location of the configuration file (`prometheus.yml`), the storage directory, and other settings.

   - `web.listen-address` configures Prometheus to listen on all network interfaces on port 9090.

   - `web.enable-lifecycle` allows for management of Prometheus through API calls.

   Enable and start Prometheus:

   ```bash
   sudo systemctl enable prometheus
   sudo systemctl start prometheus
   ```

   Verify Prometheus's status:

   ```bash
   sudo systemctl status prometheus
   ```

   You can access Prometheus in a web browser using your server's IP and port 9090:

   `http://<your-server-ip>:9090`

   **Installing Node Exporter:**

   Create a system user for Node Exporter and download Node Exporter:

   ```bash
   sudo useradd --system --no-create-home --shell /bin/false node_exporter
   wget https://github.com/prometheus/node_exporter/releases/download/v1.6.1/node_exporter-1.6.1.linux-amd64.tar.gz
   ```

   Extract Node Exporter files, move the binary, and clean up:

   ```bash
   tar -xvf node_exporter-1.6.1.linux-amd64.tar.gz
   sudo mv node_exporter-1.6.1.linux-amd64/node_exporter /usr/local/bin/
   rm -rf node_exporter*
   ```

   Create a systemd unit configuration file for Node Exporter:

   ```bash
   sudo nano /etc/systemd/system/node_exporter.service
   ```

   Add the following content to the `node_exporter.service` file:

   ```plaintext
   [Unit]
   Description=Node Exporter
   Wants=network-online.target
   After=network-online.target

   StartLimitIntervalSec=500
   StartLimitBurst=5

   [Service]
   User=node_exporter
   Group=node_exporter
   Type=simple
   Restart=on-failure
   RestartSec=5s
   ExecStart=/usr/local/bin/node_exporter --collector.logind

   [Install]
   WantedBy=multi-user.target
   ```

   Replace `--collector.logind` with any additional flags as needed.

   Enable and start Node Exporter:

   ```bash
   sudo systemctl enable node_exporter
   sudo systemctl start node_exporter
   ```

   Verify the Node Exporter's status:

   ```bash
   sudo systemctl status node_exporter
   ```

   You can access Node Exporter metrics in Prometheus.

2. **Configure Prometheus Plugin Integration:**

   Integrate Jenkins with Prometheus to monitor the CI/CD pipeline.

   **Prometheus Configuration:**

   To configure Prometheus to scrape metrics from Node Exporter and Jenkins, you need to modify the `prometheus.yml` file. Here is an example `prometheus.yml` configuration for your setup:

   ```yaml
   global:
     scrape_interval: 15s

   scrape_configs:
     - job_name: 'node_exporter'
       static_configs:
         - targets: ['localhost:9100']

     - job_name: 'jenkins'
       metrics_path: '/prometheus'
       static_configs:
         - targets: ['<your-jenkins-ip>:<your-jenkins-port>']
   ```

   Make sure to replace `<your-jenkins-ip>` and `<your-jenkins-port>` with the appropriate values for your Jenkins setup.

   Check the validity of the configuration file:

   ```bash
   promtool check config /etc/prometheus/prometheus.yml
   ```

   Reload the Prometheus configuration without restarting:

   ```bash
   curl -X POST http://localhost:9090/-/reload
   ```

   You can access Prometheus targets at:

   `http://<your-prometheus-ip>:9090/targets`


####Grafana

**Install Grafana on Ubuntu 22.04 and Set it up to Work with Prometheus**

**Step 1: Install Dependencies:**

First, ensure that all necessary dependencies are installed:

```bash
sudo apt-get update
sudo apt-get install -y apt-transport-https software-properties-common
```

**Step 2: Add the GPG Key:**

Add the GPG key for Grafana:

```bash
wget -q -O - https://packages.grafana.com/gpg.key | sudo apt-key add -
```

**Step 3: Add Grafana Repository:**

Add the repository for Grafana stable releases:

```bash
echo "deb https://packages.grafana.com/oss/deb stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
```

**Step 4: Update and Install Grafana:**

Update the package list and install Grafana:

```bash
sudo apt-get update
sudo apt-get -y install grafana
```

**Step 5: Enable and Start Grafana Service:**

To automatically start Grafana after a reboot, enable the service:

```bash
sudo systemctl enable grafana-server
```

Then, start Grafana:

```bash
sudo systemctl start grafana-server
```

**Step 6: Check Grafana Status:**

Verify the status of the Grafana service to ensure it's running correctly:

```bash
sudo systemctl status grafana-server
```

**Step 7: Access Grafana Web Interface:**

Open a web browser and navigate to Grafana using your server's IP address. The default port for Grafana is 3000. For example:

`http://<your-server-ip>:3000`

## Monitor Kubernetes with Prometheus

Prometheus is a powerful monitoring and alerting toolkit, and you'll use it to monitor your Kubernetes cluster. Additionally, you'll install the node exporter using Helm to collect metrics from your cluster nodes.

### Install Node Exporter using Helm

To begin monitoring your Kubernetes cluster, you'll install the Prometheus Node Exporter. This component allows you to collect system-level metrics from your cluster nodes. Here are the steps to install the Node Exporter using Helm:

1. Add the Prometheus Community Helm repository:

    ```bash
    helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
    ```

2. Create a Kubernetes namespace for the Node Exporter:

    ```bash
    kubectl create namespace prometheus-node-exporter
    ```

3. Install the Node Exporter using Helm:

    ```bash
    helm install prometheus-node-exporter prometheus-community/prometheus-node-exporter --namespace prometheus-node-exporter
    ```

Add a Job to Scrape Metrics on nodeip:9001/metrics in prometheus.yml:

Update your Prometheus configuration (prometheus.yml) to add a new job for scraping metrics from nodeip:9001/metrics. You can do this by adding the following configuration to your prometheus.yml file:


```
  - job_name: 'k8s'
    metrics_path: '/metrics'
    static_configs:
      - targets: ['node1Ip:9100']
```

Replace 'your-job-name' with a descriptive name for your job. The static_configs section specifies the targets to scrape metrics from, and in this case, it's set to nodeip:9001.

Don't forget to reload or restart Prometheus to apply these changes to your configuration.

To deploy an application with ArgoCD, you can follow these steps, which I'll outline in Markdown format:

### Deploy Application with ArgoCD

1. **Install ArgoCD:**

   You can install ArgoCD on your Kubernetes cluster by following the instructions provided in the [EKS Workshop](https://archive.eksworkshop.com/intermediate/290_argocd/install/) documentation.

2. **Set Your GitHub Repository as a Source:**

   After installing ArgoCD, you need to set up your GitHub repository as a source for your application deployment. This typically involves configuring the connection to your repository and defining the source for your ArgoCD application. The specific steps will depend on your setup and requirements.

3. **Create an ArgoCD Application:**
   - `name`: Set the name for your application.
   - `destination`: Define the destination where your application should be deployed.
   - `project`: Specify the project the application belongs to.
   - `source`: Set the source of your application, including the GitHub repository URL, revision, and the path to the application within the repository.
   - `syncPolicy`: Configure the sync policy, including automatic syncing, pruning, and self-healing.

4. **Access your Application**
   - To Access the app make sure port 30007 is open in your security group and then open a new tab paste your NodeIP:30007, your app should be running.

**Phase 7: Cleanup**

1. **Cleanup AWS EC2 Instances:**
    - Terminate AWS EC2 instances that are no longer needed.


# Git Commands

```
git rm --cached .env
git reset --soft HEAD~1

```

