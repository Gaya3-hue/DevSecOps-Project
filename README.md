# Netflix Clone — End-to-End DevSecOps Deployment on AWS!

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/a05054e7-36af-48b6-9a6f-fc9678b73c09" />

<img width="1905" height="978" alt="image" src="https://github.com/user-attachments/assets/d91c8aae-d695-4513-b224-99f1da4d5261" />


## 📌 Project Overview
This project demonstrates a **production-grade DevSecOps pipeline** for deploying a Netflix-style web application on AWS.  
It covers the complete lifecycle — **containerization**, **security scanning**, **CI/CD automation**, **monitoring**, and **Kubernetes deployment** — using industry-standard tools and best practices.

---

## 🚀 Key Features
- **Containerization:** Dockerized Node.js application with TMDB API integration for movie data.
- **Secure Development:** Integrated **SonarQube** for static code analysis and **Trivy** for container vulnerability scanning.
- **Automated CI/CD:** Jenkins pipeline for build, test, scan, image push to Docker Hub, and deployment.
- **Monitoring & Alerting:** Prometheus for metrics collection, Grafana dashboards for EC2, Jenkins, and Kubernetes monitoring.
- **GitOps Deployment:** Kubernetes deployment managed with **Argo CD** and Helm charts.
- **Notifications:** Gmail SMTP integration for pipeline build alerts.

---

## 🛠️ Tech Stack & Tools
**Languages & Frameworks:** Node.js, TypeScript, React  
**Containerization & Orchestration:** Docker, Kubernetes (EKS)  
**CI/CD & Automation:** Jenkins, Argo CD, Helm  
**Security:** SonarQube, Trivy  
**Monitoring:** Prometheus, Grafana  
**Cloud Platform:** AWS EC2, EKS, Elastic IP, Security Groups  

---

## 🗂️ Project Workflow
1. **AWS Setup**
   - Launch EC2 instances for Jenkins, SonarQube, and application.
   - Configure Elastic IPs and security groups.
2. **Application Deployment**
   - Clone repo, build Docker image, integrate TMDB API, run on EC2.
3. **Security Integration**
   - Configure SonarQube quality gates.
   - Run Trivy scans on Docker images.
4. **CI/CD Automation**
   - Jenkins pipeline stages: Code checkout → Build → Scan → Docker push → Deploy.
5. **Monitoring Setup**
   - Install Prometheus, Node Exporter, and Grafana.
   - Add dashboards for EC2, Jenkins, Kubernetes nodes.
6. **Kubernetes Deployment**
   - Create EKS cluster and node group.
   - Deploy app via Argo CD with Helm charts.
7. **Notifications & Cleanup**
   - Email notifications for pipeline events.
   - Delete unused resources to avoid AWS costs.

---

## 📊 Monitoring Dashboards
- **EC2 Instance Metrics**
- **Jenkins Build Performance**
- **Kubernetes Cluster Health**
- **Application Performance Metrics**

---

## 📧 Notifications
- Jenkins pipeline configured with Gmail SMTP for **real-time build status alerts**.

---

## 🔒 Security Highlights
- Early vulnerability detection with **SonarQube** and **Trivy**.
- Quality gates to prevent insecure deployments.

---


## 🏆 Learning Outcomes
- Build and secure CI/CD pipelines for real-world apps.
- Deploy containerized applications to Kubernetes with GitOps.
- Monitor and troubleshoot cloud infrastructure.
- Apply DevSecOps practices from code commit to production.

---

## 📜 License
This project is open-source and available under the [MIT License](LICENSE).

---
