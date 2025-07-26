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

---

## 📦 What’s Included

| Component          | Description                                                                  |
|--------------------|------------------------------------------------------------------------------|
| **Elasticsearch**  | Stores logs with persistent storage (PVC configured)                         |
| **Kibana**         | Visualizes logs and dashboards via NodePort                                  |
| **Fluentd**        | Collects container logs and sends them to Elasticsearch                      |
| **Spring Boot App**| Sample application writing logs that are collected via Fluentd               |
| **RBAC**           | Service account and permissions for Fluentd                                  |
| **Kustomize**      | Base and overlays structure for clean, reusable, and environment-ready YAMLs |

---

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


---

## 🧪 Running in Minikube?

If you're running this stack inside **Minikube**, the services (like Kibana and the Spring Boot app) are exposed via `NodePort`. Depending on your environment (especially with WSL2 or Docker Desktop), these ports might not be directly accessible from your host machine.

🔁 In that case, use the following **`kubectl port-forward`** commands to access them locally:

```bash
# Access Kibana (default port 5601)
kubectl port-forward svc/kibana -n efk 5601:5601

# Access Spring Boot App (default port 8080)
kubectl port-forward svc/efk-springboot-svc -n efk 8080:8080
```

Then open your browser and navigate to:
- [http://localhost:5601](http://localhost:5601) → **Kibana UI**
- [http://localhost:8080](http://localhost:8080) → **Spring Boot Application**

> 💡 Tip: Alternatively, you can run `minikube tunnel` to access services via their NodePort, if your setup allows it.
