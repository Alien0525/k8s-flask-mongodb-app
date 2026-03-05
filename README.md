# Flask To-Do App - Kubernetes Deployment

A containerized To-Do web application deployed on Kubernetes with Docker, demonstrating container orchestration, scaling, and self-healing capabilities.

## Overview

This project demonstrates deploying a Flask-based To-Do application with MongoDB backend on Kubernetes. It covers:
- Docker containerization
- Multi-container orchestration with Docker Compose
- Kubernetes deployment on Minikube (local)
- Kubernetes deployment on AWS EKS (cloud)
- ReplicaSets and auto-scaling
- Rolling updates with zero downtime
- Health monitoring with liveness and readiness probes
- Alerting with Prometheus and Slack (optional)

## Tech Stack

- **Frontend/Backend:** Flask (Python)
- **Database:** MongoDB
- **Containerization:** Docker
- **Orchestration:** Kubernetes
- **Local Cluster:** Minikube
- **Cloud Platform:** AWS EKS
- **Container Registry:** Docker Hub

## Project Structure

```
.
├── app.py                      # Flask application
├── requirements.txt            # Python dependencies
├── Dockerfile                  # Docker image definition
├── docker-compose.yml          # Multi-container local setup
├── templates/                  # HTML templates
│   ├── index.html
│   ├── update.html
│   ├── searchlist.html
│   └── credits.html
├── static/                     # CSS, JS, images
│   ├── assets/
│   └── images/
└── k8s-configs/               # Kubernetes manifests
    ├── mongodb-pvc.yaml       # Persistent volume claim
    ├── mongodb-deployment.yaml # MongoDB deployment & service
    └── flask-deployment.yaml  # Flask deployment & service
```

## Quick Start

### Prerequisites

- Docker Desktop installed
- Minikube and kubectl installed
- Docker Hub account
- AWS account (for EKS deployment)

### 1. Local Development with Docker Compose

```bash
# Clone the repository
git clone https://github.com/Alien0525/k8s-flask-mongodb-app
cd k8s-flask-mongodb-app

# Start the application
docker-compose up --build

# Access at http://localhost:5001
```

### 2. Build and Push to Docker Hub

```bash
# Build the image
docker build -t DOCKERHUB_USERNAME/flask-todo-app:v1 .

# Login to Docker Hub
docker login

# Push the image
docker push DOCKERHUB_USERNAME/flask-todo-app:v1
```

### 3. Deploy on Minikube (Local Kubernetes)

```bash
# Start Minikube
minikube start

# Apply Kubernetes configurations
kubectl apply -f k8s-configs/mongodb-pvc.yaml
kubectl apply -f k8s-configs/mongodb-deployment.yaml
kubectl apply -f k8s-configs/flask-deployment.yaml

# Verify deployment
kubectl get pods
kubectl get services

# Access the application
minikube service flask-service --url
```

### 4. Deploy on AWS EKS (Cloud)

```bash
# Create EKS cluster (using AWS Console or eksctl)
eksctl create cluster --name todo-cluster --region us-east-1

# Configure kubectl
aws eks update-kubeconfig --name todo-cluster --region us-east-1

# Apply configurations (same as Minikube)
kubectl apply -f k8s-configs/mongodb-pvc.yaml
kubectl apply -f k8s-configs/mongodb-deployment.yaml
kubectl apply -f k8s-configs/flask-deployment.yaml

# Get external URL
kubectl get services flask-service
```

## Kubernetes Features Demonstrated

### ReplicaSets and Self-Healing

```bash
# Scale deployment
kubectl scale deployment flask-deployment --replicas=4

# Test self-healing - delete a pod
kubectl delete pod <pod-name>
kubectl get pods  # Watch new pod auto-created
```

### Rolling Updates

```bash
# Update to new version
kubectl set image deployment/flask-deployment flask-app=USERNAME/flask-todo-app:v2

# Monitor rollout
kubectl rollout status deployment/flask-deployment

# Rollback if needed
kubectl rollout undo deployment/flask-deployment
```

### Health Monitoring

Health probes are configured in the deployment manifest:
- **Liveness Probe:** Checks if container is alive
- **Readiness Probe:** Checks if container is ready to serve traffic

### View Logs

```bash
# View logs from a specific pod
kubectl logs <pod-name>

# Follow logs in real-time
kubectl logs -f <pod-name>
```

## Monitoring & Alerting 

### Install Prometheus

```bash
# Add Prometheus Helm repo
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update

# Install Prometheus
helm install prometheus prometheus-community/kube-prometheus-stack
```

### Configure Slack Alerts

Configure alertmanager to send notifications to Slack when:
- Pods fail health checks
- Resource limits are exceeded
- Deployment failures occur

## Shutdown Commands

```bash
# Stop Minikube
minikube stop

# Stop Docker Compose
docker-compose down

# Delete Kubernetes resources
kubectl delete -f k8s-configs/

# Completely remove Minikube cluster
minikube delete
```

## Screenshots

Will include screenshots demonstrating:
1. Application running locally (Docker Compose)
2. Application running on Minikube
3. Application running on AWS EKS
4. Pods auto-healing after deletion
5. Scaling demonstration
6. Rolling update in progress
7. Prometheus alerts (if implemented)

## Testing

```bash
# Check all pods are running
kubectl get pods

# Check services
kubectl get services

# Check deployments
kubectl get deployments

# Describe a pod for details
kubectl describe pod <pod-name>
```

## Learning Objectives

This project demonstrates:
- Container orchestration fundamentals
- Kubernetes resource management
- Declarative vs imperative deployment
- Service discovery and networking
- Persistent storage in Kubernetes
- High availability and fault tolerance
- Zero-downtime deployments
- Cloud-native application architecture

## Resources

- [Docker Documentation](https://docs.docker.com/)
- [Kubernetes Documentation](https://kubernetes.io/docs/)
- [Minikube Documentation](https://minikube.sigs.k8s.io/docs/)
- [AWS EKS Documentation](https://docs.aws.amazon.com/eks/)
- [Flask Documentation](https://flask.palletsprojects.com/)

## 👤 Author

Aman N S - Cloud Computing & Big Data Systems - Spring 2026

## License

This project is created for educational purposes as part of Cloud Computing Assignment 2.