# Node.js Application in Kubernetes

## Overview
This project demonstrates a simple Node.js application built with Express.js, containerized using Docker, and deployed on a Kubernetes cluster. The application runs locally on a Minikube cluster and can be extended to run on cloud-based Kubernetes environments.

## Features
- Node.js application built with Express.js.
- Dockerized for consistent deployment.
- Deployed in Kubernetes using Minikube for local testing.
- Scalable to cloud-based Kubernetes clusters.

## Project Structure

```bash
.
├── app.js                  # Your Node.js application entry point
├── package.json            # Node.js dependencies and scripts
├── Dockerfile              # Docker configuration for the Node.js app
├── deployment.yaml         # Kubernetes Deployment and Service definition
└── README.md               # Project documentation
 ```



## Prerequisites
To set up and run the project, ensure you have the following:
- **Node.js** installed (version 20 or later).
- **Docker** installed and running.
- **Kubernetes** cluster (local or cloud-based, e.g., Minikube or GKE).
- **kubectl** CLI tool installed and configured.

## Setup and Deployment

### 1. Clone the Repository
First, clone the project repository:
```bash
git clone <repository_url>
cd <project_directory>
```

### 2. Install Dependencies
   Install the required dependencies by running:

```bash
npm install
```
### 3. Build the Docker Image
   Before deploying to Kubernetes, build and optionally push the Docker image to DockerHub:

```bash
docker build -t <your_dockerhub_username>/nodejs-app .
```
(Optional) Push the image to DockerHub:

```bash
docker push <your_dockerhub_username>/nodejs-app
```

### 4. Deploy to Kubernetes
   Apply the Kubernetes deployment and service configuration using kubectl:

```bash
kubectl apply -f deployment.yaml
```
This will create a Kubernetes deployment with 3 replicas and expose the service via a LoadBalancer on port 80.

### 5. Access the Application
   If you're using Minikube, access the service by running:

```bash
minikube service node-k8s-app-service
```

This will open the application in your browser.

For cloud-based Kubernetes clusters, note the external IP of the LoadBalancer by running:

```bash
kubectl get services
``` 
Then, navigate to the external IP address in your browser.

### 6. Cleanup
   To clean up the Kubernetes resources, run:
```bash
kubectl delete -f deployment.yaml
```