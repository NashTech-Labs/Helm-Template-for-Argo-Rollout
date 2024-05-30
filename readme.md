# Helm Chart with Argo Rollouts for Kubernetes Deployments

This repository provides a template for deploying applications to Kubernetes using Helm charts and Argo Rollouts. This setup allows for advanced deployment strategies such as canary releases and blue-green deployments.

## Directory Structure

Ensure your repository is structured as follows:

```plaintext
.
├── charts
│   └── my-app
│       ├── templates
│       │   ├── rollout.yaml
│       │   └── service.yaml
│       ├── Chart.yaml
│       └── values.yaml
├── README.md
└── .gitignore
```

## Prerequisites

- Kubernetes cluster
- Helm installed
- Argo Rollouts controller installed in the Kubernetes cluster
- Docker Hub account with repository

## Steps to Use the Template

### Install Argo Rollouts

Install the Argo Rollouts controller in your Kubernetes cluster:

```bash
kubectl create namespace argo-rollouts
kubectl apply -n argo-rollouts -f https://raw.githubusercontent.com/argoproj/argo-rollouts/stable/manifests/install.yaml
```
### Configure the Helm Chart
Update charts/my-app/values.yaml with your Docker image repository.

### Create a Dockerfile
Ensure you have a Dockerfile at the root of your repository to build your application image.

### Build and Push Docker Image
Build and push your Docker image to your repository:

```bash
docker build -t your-docker-username/my-app:latest .
docker push your-docker-username/my-app:latest
```

### Deploy the Helm Chart
Deploy your application using Helm:
```bash
helm repo add argo https://argoproj.github.io/argo-helm
helm install my-app charts/my-app --set image.repository=your-docker-username/my-app --set image.tag=latest --namespace default
```
### Update the Deployment
For subsequent updates, update the Docker image and then upgrade the Helm release:
```bash
docker build -t your-docker-username/my-app:new-tag .
docker push your-docker-username/my-app:new-tag
helm upgrade my-app charts/my-app --set image.repository=your-docker-username/my-app --set image.tag=new-tag --namespace default
```
