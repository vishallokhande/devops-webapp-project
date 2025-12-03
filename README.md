# DevOps WebApp Project – CI/CD on AWS (ECR + EKS + Jenkins + Monitoring)

This README documents the complete end‑to‑end DevOps pipeline you built using AWS services such as **EKS, EC2, ECR**, Jenkins-based CI/CD, Dockerized Web Application Deployment, and Monitoring using **Prometheus & Grafana**.

---

## 🚀 Project Overview

This project demonstrates a complete real‑world DevOps workflow:

- Build → Test → Dockerize → Push image to **Amazon ECR**
- Automate deployment to **Amazon EKS**
- Enable **end‑to‑end CI/CD** using Jenkins on EC2
- Implement AWS authentication for Jenkins using IAM user
- Deploy Monitoring Stack (**Prometheus + Grafana**) on EKS
- Visualize cluster metrics using Grafana dashboard

You also configured IAM, EC2, ECR, EKS, and Monitoring manually and validated everything.

---

## 🏛️ Architecture Diagram (High Level)

```
Local Machine → GitHub → Jenkins (EC2)
      └──── Docker Build → Push to ECR
             └──── Deploy to EKS
                    └──── Service (LoadBalancer)
                           └──── Users Access App
Prometheus ← Metrics ← EKS nodes → Grafana UI
```

---

## 🧰 Tech Stack

| Component     | Technology Used |
|--------------|------------------|
| CI/CD        | Jenkins on EC2 |
| Containers   | Docker |
| Registry     | Amazon ECR |
| Orchestration | Amazon EKS |
| Monitoring    | Prometheus + Grafana |
| Language/App  | Sample WebApp |
| Authentication | AWS IAM (dev_proj_EKS user) |

---

## 🔑 AWS Configuration

### 1️⃣ IAM Setup  
- Created user **dev_proj_EKS**
- Attached policies:
  - `AmazonEKSClusterPolicy`
  - `AmazonEC2FullAccess`
  - `AmazonECRFullAccess`
  - `IAMUserChangePassword`
- Generated ACCESS_KEY + SECRET_KEY for Jenkins authentication

### 2️⃣ EC2 Instances Used  
- **jenkins-ci** → Jenkins server  
- **dev_proj_EKS** → EC2 used for configuration  
- **devops-eks-standard-workers** → EKS worker nodes  

---

## 🧱 EKS Cluster Setup

### Cluster Components:
- 1 Control Plane  
- 2 Worker Nodes  
- Networking → AWS VPC CNI  
- Storage → EBS  

You successfully configured `kubectl` with:

```
aws eks update-kubeconfig --region ap-south-1 --name devops-eks
```

---

## 📦 Amazon ECR Repository

Example:

```
704444257628.dkr.ecr.ap-south-1.amazonaws.com/devops-sample-app
```

Images pushed automatically during the pipeline.

---

## 📌 Jenkins Pipeline Workflow

### Stages:
1. **Checkout Code**
2. **Build Docker Image**
3. **Authenticate to Amazon ECR**
4. **Push Image to ECR**
5. **Deploy to EKS**
6. **Verify Deployment**

---

## 📝 Jenkinsfile Used

```groovy
pipeline {
    agent any
    environment {
        AWS_REGION = "ap-south-1"
        ECR_REGISTRY = "704444257628.dkr.ecr.$AWS_REGION.amazonaws.com"
        ECR_REPO = "devops-sample-app"
        IMAGE_TAG = "${env.BUILD_NUMBER}"
    }
    stages {
        stage('Checkout') {
            steps { checkout scm }
        }
        stage('Build Docker') {
            steps {
                sh "docker build -t $ECR_REPO:$IMAGE_TAG ./app"
            }
        }
        stage('Authenticate ECR') {
            steps {
                sh '''
                    aws ecr get-login-password --region $AWS_REGION |                     docker login --username AWS --password-stdin $ECR_REGISTRY
                    docker tag $ECR_REPO:$IMAGE_TAG $ECR_REGISTRY/$ECR_REPO:$IMAGE_TAG
                '''
            }
        }
        stage('Push to ECR') {
            steps {
                sh "docker push $ECR_REGISTRY/$ECR_REPO:$IMAGE_TAG"
            }
        }
        stage('Deploy to EKS') {
            steps {
                sh '''
                    sed -i "s|latest|$IMAGE_TAG|g" k8s/deployment.yaml || true
                    kubectl apply -f k8s/deployment.yaml
                '''
            }
        }
    }
}
```

---

## 📊 Monitoring (Prometheus & Grafana)

You deployed monitoring stack in namespace:

```
kubectl get svc -n monitoring
```

Grafana LoadBalancer endpoint example:

```
a4be795aab...
```

Prometheus services running inside cluster:
- alertmanager  
- kube-state-metrics  
- node-exporter  
- prometheus-server  

---

## 🟢 Final Deployment Validation

```
kubectl get pods
kubectl get svc
kubectl get nodes
```

App accessible via LoadBalancer URL.

Grafana accessible using LB URL in namespace `monitoring`.

---

## 📌 Summary

You successfully built:

✔ Full CI/CD pipeline  
✔ Automated build → push → deploy  
✔ Secure IAM user integration  
✔ Real ECR + EKS deployment  
✔ Monitoring + visualization stack  
✔ Production‑grade Jenkins pipeline  

This project is **resume-ready** and **interview-ready**.

---

## 📬 Need ZIP file, SVG diagram, or PDF version?

Just ask — I can generate those instantly.
