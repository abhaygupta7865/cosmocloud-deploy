# cosmocloud-deploy
Cosmocloud Deploy - Helm Chart

This repository contains the Helm chart (cosmocloud-deploy) to deploy a simple web application stack consisting of a Backend, Frontend, and Redis database using Kubernetes. The goal of this project is to demonstrate the ability to deploy and manage an application stack using Kubernetes and Helm.

Table of Contents

Overview
Prerequisites
Installation
Deployment Instructions
Application Architecture
Services
Environment Variables
Testing the Application
Cleanup
Contributing

Overview-

This project deploys a full-stack application using Helm charts. The architecture includes:

Backend: A sample backend service that interacts with Redis.
Frontend: A simple frontend that interacts with the backend via HTTP.
Redis: A Redis instance used by the backend.
The application will be deployed as a set of Kubernetes resources using Helm, including deployments, services, and ingress resources.

Prerequisites-

Before you begin, make sure you have the following tools installed:

Kubernetes Cluster (Minikube or any cloud-based Kubernetes service)
kubectl: Command-line tool for Kubernetes management.
Helm: Package manager for Kubernetes.
Docker (for building any custom images, if needed)
You can follow the official documentation to install and set up these tools:

Kubernetes Installation Guide
Helm Installation Guide
Installation

1. Clone the Repository
git clone https://github.com/abhaygupta7865/cosmocloud-deploy.git
cd cosmocloud-deploy
2. Install Helm Dependencies
Run the following command to install any dependencies:

helm dep update
This step ensures that any external charts used in your Helm chart (if any) are downloaded.

Deployment Instructions-

1. Install the Helm Chart
To deploy the application stack (backend, frontend, Redis), use the following Helm command:

helm install testapp ./cosmocloud-deploy --atomic --timeout 2m
testapp: This is the release name for your deployment.
./cosmocloud-deploy: Path to the Helm chart directory.
--atomic: Ensures the deployment is rolled back if anything fails.
--timeout 2m: Specifies a timeout of 2 minutes for the installation process.
2. Check Deployment Status
You can check if all pods are running with:

kubectl get pods
To verify that all services are running, use:

kubectl get svc
Application Architecture

The application stack consists of three services:

Backend:
A simple backend service exposed via a Kubernetes deployment.
Communicates with Redis for caching or other storage purposes.
Frontend:
A React frontend that interacts with the backend service to display data.
Redis:
A Redis service used as an in-memory data store by the backend.
The services are connected through the following environment variables:

Backend uses REDIS_URI=redis://redis-svc:6379.
Frontend uses BACKEND_URL=http://backend-svc:8000.
Services

1. Backend Service (backend-svc)
Type: ClusterIP
Port: 8000
Purpose: Internal communication within the Kubernetes cluster. The backend service interacts with the frontend and Redis.
2. Frontend Service (frontend-svc)
Type: NodePort
Port: 5173
NodePort: 31000
Purpose: Exposes the frontend externally on port 31000, which you can access in your browser.
3. Redis Service (redis-svc)
Type: ClusterIP
Port: 6379
Purpose: Used internally for communication with the backend.
Environment Variables

1. Backend Service (backend container):
REDIS_URI: The URI for connecting to the Redis service (e.g., redis://redis-svc:6379).
2. Frontend Service (frontend container):
BACKEND_URL: The URL for connecting to the backend service (e.g., http://backend-svc:8000).
These environment variables allow the services to communicate with each other and work seamlessly.

Testing the Application-

After deploying the stack, you can access the frontend service by visiting http://localhost:31000 in your web browser. The frontend should be able to communicate with the backend and display data accordingly.

Check Application Logs
To troubleshoot or check the logs of a specific pod, you can use the following command:

kubectl logs <pod-name>
You can get the pod names using:

kubectl get pods
Cleanup-

To delete the deployed resources and clean up your Kubernetes environment, use the following command:

helm uninstall testapp
This will remove all deployed resources associated with the release testapp.


