DevSecOps Project – Deploy Netflix Clone on Cloud using Jenkins
YouTube Video
Step-by-step demonstration available here:
Video Tutorial

Overview
This project demonstrates how to deploy a Netflix Clone application on the cloud using Jenkins, Docker, SonarQube, Trivy, Prometheus, and Grafana, implementing full DevSecOps practices. It includes security scanning, CI/CD automation, monitoring, and Kubernetes deployment.

Project Phases
Phase 1: Initial Setup and Deployment
Step 1: Launch EC2 (Ubuntu 22.04)

Provision an EC2 instance with Ubuntu 22.04.

Connect via SSH.

Step 2: Clone the Code

bash
Copy
Edit
sudo apt-get update && sudo apt-get upgrade -y
git clone https://github.com/N4si/DevSecOps-Project.git
Step 3: Install Docker & Run Application

bash
Copy
Edit
sudo apt-get install docker.io -y
sudo usermod -aG docker $USER
newgrp docker
sudo chmod 777 /var/run/docker.sock

docker build -t netflix .
docker run -d --name netflix -p 8081:80 netflix:latest
Note: API key is required to run the application successfully.

Step 4: Get API Key from TMDB

Create an account at TMDB.

Generate a V3 API key.

Rebuild the Docker image with:

bash
Copy
Edit
docker build --build-arg TMDB_V3_API_KEY=<your-api-key> -t netflix .
Phase 2: Security
Install SonarQube

bash
Copy
Edit
docker run -d --name sonar -p 9000:9000 sonarqube:lts-community
Access at: http://<public-ip>:9000 (default: admin/admin)

Install Trivy

bash
Copy
Edit
sudo apt-get install wget apt-transport-https gnupg lsb-release -y
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -
echo deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main | sudo tee /etc/apt/sources.list.d/trivy.list
sudo apt-get update
sudo apt-get install trivy -y
To scan an image:

bash
Copy
Edit
trivy image <imageid>
Phase 3: CI/CD Setup
Install Jenkins

bash
Copy
Edit
sudo apt update
sudo apt install fontconfig openjdk-17-jre -y
java -version

wget -O /usr/share/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins -y
sudo systemctl start jenkins
sudo systemctl enable jenkins
Access Jenkins at: http://<public-ip>:8080

Install Jenkins Plugins:

Eclipse Temurin Installer

SonarQube Scanner

NodeJS Plugin

Email Extension Plugin

OWASP Dependency-Check

Docker Plugins (Docker, Docker Commons, Docker Pipeline, Docker API, docker-build-step)

Configure Global Tools:

JDK 17

NodeJS 16

Sonar Scanner

Dependency-Check

Example Jenkins Pipeline

groovy
Copy
Edit
pipeline {
    agent any
    tools {
        jdk 'jdk17'
        nodejs 'node16'
    }
    environment {
        SCANNER_HOME = tool 'sonar-scanner'
    }
    stages {
        stage('Clean Workspace') { steps { cleanWs() } }
        stage('Checkout') {
            steps { git branch: 'main', url: 'https://github.com/N4si/DevSecOps-Project.git' }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar-server') {
                    sh '''$SCANNER_HOME/bin/sonar-scanner \
                    -Dsonar.projectName=Netflix \
                    -Dsonar.projectKey=Netflix'''
                }
            }
        }
        stage('Quality Gate') {
            steps {
                script { waitForQualityGate abortPipeline: false, credentialsId: 'Sonar-token' }
            }
        }
        stage('Install Dependencies') { steps { sh "npm install" } }
        stage('OWASP FS Scan') {
            steps {
                dependencyCheck additionalArguments: '--scan ./ --disableYarnAudit --disableNodeAudit', odcInstallation: 'DP-Check'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        stage('Trivy FS Scan') { steps { sh "trivy fs . > trivyfs.txt" } }
        stage('Docker Build & Push') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker', toolName: 'docker') {
                        sh "docker build --build-arg TMDB_V3_API_KEY=<yourapikey> -t netflix ."
                        sh "docker tag netflix <your-dockerhub-username>/netflix:latest"
                        sh "docker push <your-dockerhub-username>/netflix:latest"
                    }
                }
            }
        }
        stage('Trivy Image Scan') {
            steps { sh "trivy image <your-dockerhub-username>/netflix:latest > trivyimage.txt" }
        }
        stage('Deploy Container') {
            steps { sh "docker run -d --name netflix -p 8081:80 <your-dockerhub-username>/netflix:latest" }
        }
    }
}
Phase 4: Monitoring
Prometheus: Installed and configured to scrape metrics from Node Exporter and Jenkins.

Grafana: Installed and connected to Prometheus for dashboards.

Phase 5: Notification
Configure email notifications in Jenkins for build status updates.

Phase 6: Kubernetes Deployment
Create Kubernetes cluster with node groups.

Install Node Exporter via Helm for metrics.

Deploy application with ArgoCD.

Monitor Kubernetes with Prometheus & Grafana.

Phase 7: Cleanup
Terminate unused AWS EC2 instances.

Prerequisites
AWS Account

GitHub Account

Docker Hub Account

Basic knowledge of Jenkins, Docker, and Kubernetes

License
This project is for educational purposes only. All credits to the original author: N4si/DevSecOps-Project.

If you want, I can also add a table of contents to this README so it’s easier to navigate on GitHub. That will make it look even more professional.
Do you want me to add that?
