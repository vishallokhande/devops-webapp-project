# DevOps WebApp Project – Complete CI/CD on AWS (EKS, Jenkins, Terraform, Monitoring)

This README summarizes the complete project implemented using **AWS EKS, Jenkins CI/CD, Terraform, Docker, ECR**, and **Prometheus–Grafana monitoring**.

---

## 📌 Architecture Overview

Below is the architecture followed in your actual deployment:

![IAM Screenshot](0.1.png)
![EC2 Screenshot](0.2.png)
![Monitoring Screenshot](0.3.png)
![ECR Screenshot](0.4.png)

---

## 🚀 Project Workflow

### 1️⃣ Developer Workflow
- Developer pushes code → GitHub Repo  
- Webhook triggers Jenkins → Jenkins pulls latest code  

### 2️⃣ CI Pipeline – Jenkins
Jenkins executes:
1. Clone repository  
2. Build Docker image  
3. Tag image  
4. Push to Amazon ECR  
5. Update Kubernetes manifests  
6. Deploy to AWS EKS cluster  

### 3️⃣ CD Workflow – EKS Deployment
- Kubernetes applies new deployment  
- Rolling update is performed  
- Application becomes available on LoadBalancer  

### 4️⃣ Monitoring
Using Helm charts:
- Prometheus  
- Grafana  
- Node Exporter  
- Kube State Metrics  

---

## 🛠 Technologies Used

| Component | Technology |
|----------|------------|
| Compute | AWS EC2 |
| Container Orchestration | AWS EKS |
| CI/CD | Jenkins |
| IaC | Terraform |
| Container Registry | Amazon ECR |
| Monitoring | Prometheus + Grafana |
| SCM | GitHub |

---

## 📂 Project Structure

```
devops-webapp-project/
│── app/
│── infra/
│── Jenkinsfile
│── README.md
```

---

## 🔧 Jenkinsfile (High Level)

```
pipeline {
    agent any
    stages {
        stage("Checkout") { steps { checkout scm } }
        stage("Build Docker") { steps { sh 'docker build -t app .' } }
        stage("Push to ECR") { steps { sh 'docker push ...' } }
        stage("Deploy to EKS") { steps { sh 'kubectl apply -f deployment.yaml' } }
    }
}
```

---

## 🎯 Final Deliverables
✔ Full AWS Infrastructure  
✔ Jenkins CI/CD Pipeline  
✔ Dockerized Application  
✔ Live EKS Deployment  
✔ Monitoring with Grafana  
✔ Professional README  

---

If you want:
🔹 ZIP File  
🔹 SVG/PNG Architecture Diagram  
🔹 Full PPT deck  

Just say “Generate ZIP” or “Generate Diagram” or “Generate PPT”.
