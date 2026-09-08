# AWS Kubernetes DevOps Project 🚀

A production-style DevOps project demonstrating Infrastructure as Code, configuration management, containerization, Kubernetes deployment, and CI/CD automation on AWS.

## 🏗️ Architecture

```text
Developer
   │
   ▼
GitHub Repository
   │
   ▼
GitHub Actions
   │
   ├── Build Docker Image
   │
   └── Push Image
           │
           ▼
      Docker Hub
           │
           ▼
        AWS EC2
           │
           ▼
          k3s
           │
           ▼
     Kubernetes Deployment
           │
           ▼
     Kubernetes Service
           │
           ▼
        Nginx App
```

## 🛠️ Technologies

* AWS EC2
* AWS VPC
* Terraform
* Ansible
* Docker
* Kubernetes (k3s)
* GitHub Actions
* Docker Hub
* Nginx
* Metrics Server
* Linux / Ubuntu

## ☁️ AWS Infrastructure

The infrastructure includes:

* Custom VPC
* Public Subnet
* Private Subnet
* Internet Gateway
* Public Route Table
* Security Group
* EC2 Instance
* SSH Key Pair

### Network Configuration

```text
VPC:              10.0.0.0/16
Public Subnet:    10.0.1.0/24
Private Subnet:   10.0.2.0/24
Region:           eu-central-1
```

The EC2 instance runs Ubuntu and hosts the Kubernetes cluster.

## 🐳 Docker

The application is containerized using Docker and Nginx.

Docker image:

```text
omarhazem13/production-devops-app:latest
```

The Docker image is automatically built and pushed to Docker Hub through GitHub Actions.

## ☸️ Kubernetes

Kubernetes is deployed using **k3s** on the AWS EC2 instance.

### Deployment

The application is deployed using:

```text
kubernetes/app-deployment.yml
```

The deployment contains:

* 1 application replica
* Nginx container
* CPU requests and limits
* Memory requests and limits

Example resources:

```yaml
requests:
  cpu: "50m"
  memory: "32Mi"

limits:
  cpu: "200m"
  memory: "64Mi"
```

### Service

The application is exposed through a Kubernetes NodePort.

```text
Service Port: 8080
Target Port: 80
NodePort: 30193
```

Traffic flow:

```text
Internet
   ↓
AWS Security Group
   ↓
EC2 :30193
   ↓
Kubernetes NodePort
   ↓
Service :8080
   ↓
Pod :80
   ↓
Nginx
```

## 🔄 CI/CD Pipeline

GitHub Actions automates the deployment process.

Pipeline:

```text
Git Push
   ↓
GitHub Actions
   ↓
Docker Login
   ↓
Docker Build
   ↓
Docker Push
   ↓
SSH to EC2
   ↓
Update Kubernetes Deployment
   ↓
Kubernetes Rollout
```

The workflow is located at:

```text
.github/workflows/docker.yml
```

The deployment is updated automatically using:

```bash
kubectl set image
```

and the rollout is verified using:

```bash
kubectl rollout status
```

## ⚙️ Ansible

Ansible is used for server configuration and automation.

Ansible files:

```text
ansible/
├── inventory.ini
└── setup.yml
```

The playbook performs tasks such as:

* Updating the package cache
* Installing required packages
* Ensuring Docker is installed and running
* Enabling Docker at boot

Example:

```bash
ansible-playbook -i ansible/inventory.ini ansible/setup.yml
```

## 📊 Monitoring

Kubernetes Metrics Server is enabled to monitor cluster resource usage.

Example:

```bash
k3s kubectl top node
```

Example output:

```text
NAME           CPU(cores)   CPU(%)   MEMORY(bytes)   MEMORY(%)
ip-10-0-1-76   331m         16%      588Mi           64%
```

Pod resource usage can also be monitored:

```bash
k3s kubectl top pods -A
```

Prometheus and Grafana are planned as future monitoring improvements.

## 🔐 Security

Security practices implemented in this project include:

* AWS Security Groups
* SSH key-based authentication
* Docker Hub authentication using GitHub Secrets
* EC2 deployment through SSH
* Kubernetes resource limits
* Private subnet architecture
* No credentials stored directly in the Git repository

> For a production environment, SSH access should be restricted to trusted IP addresses and HTTPS/TLS should be enabled.

## 📁 Project Structure

```text
production-devops-project/
│
├── .github/
│   └── workflows/
│       └── docker.yml
│
├── ansible/
│   ├── inventory.ini
│   └── setup.yml
│
├── kubernetes/
│   └── app-deployment.yml
│
├── terraform/
│   └── main.tf
│
├── Dockerfile
├── index.html
└── README.md
```

## 🚀 Deployment

### 1. Build Docker Image

```bash
docker build -t omarhazem13/production-devops-app:latest .
```

### 2. Push Image

```bash
docker push omarhazem13/production-devops-app:latest
```

### 3. Apply Kubernetes Deployment

```bash
k3s kubectl apply -f kubernetes/app-deployment.yml
```

### 4. Check Pods

```bash
k3s kubectl get pods
```

### 5. Check Services

```bash
k3s kubectl get svc
```

### 6. Check Deployment

```bash
k3s kubectl get deployments
```

### 7. Check Rollout

```bash
k3s kubectl rollout status deployment/devops-app
```

## 🧪 Verification

The application can be tested using:

```bash
curl http://<EC2_PUBLIC_IP>:30193
```

Expected response:

```text
DevOps Application
Deployed using Docker on AWS EC2
Version: 1.0
```

## 💡 Skills Demonstrated

This project demonstrates practical experience with:

* Cloud Infrastructure
* AWS
* Infrastructure as Code
* Terraform
* Configuration Management
* Ansible
* Docker
* Containerization
* Kubernetes
* k3s
* CI/CD
* GitHub Actions
* Docker Hub
* Linux Administration
* Networking
* Security Groups
* SSH
* Monitoring
* Deployment Automation

## 🔮 Future Improvements

Possible improvements include:

* Prometheus monitoring
* Grafana dashboards
* HTTPS / TLS
* Kubernetes Ingress
* Centralized logging
* Kubernetes Secrets
* Terraform modules
* Terraform remote state
* Automated rollback
* Versioned Docker images using Git commit SHA
* High Availability Kubernetes cluster
* Automated infrastructure provisioning

## 👨‍💻 Author

**Omar Hazem**

Electrical, Communications & Electronics Engineering Graduate

DevOps | Cloud | Kubernetes | AWS | Cybersecurity

---

⭐ This project was built as a practical DevOps portfolio project demonstrating an end-to-end cloud deployment workflow.
