# DevOps
# DevOps Project: CI/CD Pipeline with Docker, Kubernetes, Minikube, and Jenkins

This project demonstrates how to build, containerize, and deploy a full-stack application using modern DevOps tools. The pipeline automates the process of building, testing, and deploying an Angular frontend and a Spring Boot backend.

---

## Features
- **Frontend**: Angular application served via NGINX.
- **Backend**: Spring Boot application with a MySQL database.
- **CI/CD Pipeline**: Automates build, test, and deployment using Jenkins.
- **Containerization**: Dockerized frontend and backend services.
- **Orchestration**: Deployed on Kubernetes using Minikube.

---

## Project Structure
```plaintext
.
├── Angular_Gestion_Foyer/   # Frontend Angular project
├── myFirstProject/          # Backend Spring Boot project
├── k8s/                     # Kubernetes manifests
│   ├── mysql.yml            # MySQL deployment and service
│   ├── backend-deployment.yml # Backend deployment and service
│   ├── frontend-deployment.yml # Frontend deployment and service
│   ├── configmap.yaml       # Configurations for the application
├── Jenkinsfile              # Jenkins CI/CD pipeline script
└── README.md                # Project documentation
Tools Used
Docker:

Containerized both frontend and backend applications.
Optimized image sizes using multi-stage builds.
Kubernetes:

Managed deployments, services, and configurations.
Used Minikube for local development.
Jenkins:

Automated the CI/CD pipeline.
Configured to trigger builds on every push to the main branch.
Minikube:

Set up a local Kubernetes cluster to deploy the application.
Setup Instructions
Prerequisites
Docker
Kubernetes CLI (kubectl)
Minikube
Jenkins
Java JDK (for backend)
Node.js (for frontend)
Steps to Run Locally
Clone the Repository:

bash
Copier le code
git clone https://github.com/marwensouissi/Test_Devops.git
cd Test_Devops
Build Docker Images:

Frontend:
bash
Copier le code
docker build -t marwensouissi/angular:latest ./Angular_Gestion_Foyer
Backend:
bash
Copier le code
docker build -t marwensouissi/springboot:latest ./myFirstProject
Run with Kubernetes:

Start Minikube:
bash
Copier le code
minikube start
Apply Kubernetes Manifests:
bash
Copier le code
kubectl apply -f k8s/
Port Forward Services:

Frontend:
bash
Copier le code
kubectl port-forward svc/frontend 4200:80
Backend:
bash
Copier le code
kubectl port-forward svc/backend 8089:8089
Access the Application:

Frontend: http://localhost:4200
Backend: http://localhost:8089
CI/CD Pipeline
The Jenkinsfile automates the following steps:

Checkout Code: Clones the GitHub repository.
Build Frontend: Uses npm to build the Angular app.
Build Backend: Uses Maven to build the Spring Boot app.
Build Docker Images: Containerizes the applications and pushes them to Docker Hub.
Deploy to Kubernetes: Applies the Kubernetes manifests.
Port Forward Services: Exposes services for local testing.
Challenges Faced and Solutions
Disk Space Issues in Minikube:

Resized the virtual disk and optimized Docker images.
Credential Management:

Stored Docker Hub credentials securely in Jenkins.
Pipeline Compatibility:

Adjusted scripts for Windows (PowerShell) and Linux environments.
Future Improvements
Integrate automated testing into the pipeline.
Add monitoring and logging for Kubernetes pods.
Deploy to a cloud Kubernetes provider like AWS EKS or Google GKE.
Contact
Author: Marwen Souissi
GitHub: marwensouissi
Docker Hub: marwensouissi

Thank you for checking out this project! Feel free to contribute or reach out with any questions.

