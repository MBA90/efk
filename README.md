# 📊 - Elasticsearch Fluentd Kibana EFK Stack Deployment on Kubernetes

This repository contains Kubernetes manifests for deploying a complete **EFK (Elasticsearch, Fluentd, Kibana)** logging stack along with a sample **Spring Boot application**. The setup is structured for **best practices**, supports **persistent storage**, and is ready for **GitOps workflows** using tools like Kustomize, ArgoCD, or GitHub Actions.

## 📁 EFK Deployment Structure

```
├── base/                   
│   ├── elasticsearch/
│   │   ├── deployment.yml
│   │   ├── service.yml
│   │   └── pvc.yml
│   ├── kibana/
│   │   ├── deployment.yml
│   │   └── service.yml
│   ├── fluentd/
│   │   ├── configmap.yml
│   │   ├── daemonset.yml
│   │   ├── serviceaccount.yml
│   │   ├── clusterrole.yml
│   │   └── clusterrolebinding.yml
│   ├── app/
│   │   ├── configmap.yml
│   │   ├── deployment.yml
│   │   └── service.yml
│   ├── namespace.yaml
└── └── kustomization.yaml    
```

## 🚀 How to Deploy
 
 Clone the repository

```bash
git clone https://github.com/MBA90/ef.git
cd efk
```

## 🚀 Deploying the EFK Stack

To deploy the base EFK stack to your Kubernetes cluster, run:

```bash
kubectl apply -k base/
```

This command uses **Kustomize** to apply all manifests defined in the `base/` directory, including:
- Elasticsearch
- Kibana
- Fluentd
- Spring Boot Application

Make sure your current context is set to the correct Kubernetes cluster.
