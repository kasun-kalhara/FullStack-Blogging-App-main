🚀 FullStack Blogging App

A production-ready full-stack blogging platform designed with DevOps best practices in mind. This project integrates infrastructure as code, container orchestration, continuous deployment, and scalable architecture to enable a modern development lifecycle.


🌐 Live Demo

![alt text](</images/Screenshot 2025-05-24 140831.png>) ![alt text](</images/Screenshot 2025-05-24 140803.png>)


📌 Features

✍️ Full CRUD blog management
🔐 Secure user authentication
🐳 Dockerized microservice architecture
⚙️ Kubernetes deployment for scalable service orchestration
☁️ Infrastructure as Code (IaC) with Terraform (EKS)
🔄 CI/CD with Jenkins pipeline and GitHub integration



🛠️ Tech Stack


| Layer      | Technology                                        |
| ---------- | ------------------------------------------------- |
| Frontend   | HTML, CSS, JavaScript                             |
| Backend    | Java (Spring Boot), REST APIs                     |
| DevOps     | Docker, Kubernetes (EKS), Jenkins, GitHub Actions |
| IaC        | Terraform (EKS setup, VPC, IAM, Node Groups)      |
| Monitoring | \[Optional: Prometheus, Grafana if used]          |


⚙️ DevOps & Infrastructure

☁️ Infrastructure as Code (Terraform)

Terraform scripts under EKS_Terraform/ automate the following:

1.VPC creation
2.EKS Cluster provisioning
3.IAM roles and policies for worker nodes
4.Kubernetes node group setup

How to apply:

cd EKS_Terraform
terraform init
terraform apply


🐳 Containerization

The Spring Boot app is containerized with Docker using the provided Dockerfile.
Docker image is used in Kubernetes pods for scalable deployment.

docker build -t fullstack-blogging-app .

☸️ Kubernetes (EKS)

Uses AWS EKS for orchestrating the blogging app.
Kubernetes manifest (deployment-service.yml) deploys and exposes the application.

kubectl apply -f deployment-service.yml

🔄 CI/CD Pipeline

Uses Jenkins with Jenkinsfile for continuous integration and delivery.
Can be extended with GitHub Actions for build/test automation.

Pipeline includes:

  Code checkout
  Maven build & test
  Docker build & push
  Kubernetes deployment


![alt text](</images/Screenshot 2025-05-24 145627.png>) ![alt text](</images/Screenshot 2025-05-24 145614.png>)

🧪 Getting Started Locally

Prerequisites

Java 11+
Maven
Docker
AWS CLI
Terraform
kubectl
Jenkins (with Docker and Kubernetes plugins)


![alt text](</images/Screenshot 2025-05-24 145714.png>) ![alt text](</images/Screenshot 2025-05-24 141751.png>) ![alt text](</images/Screenshot 2025-05-24 141839.png>)
