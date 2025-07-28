# 📊 - Elasticsearch Fluentd Kibana EFK Stack Deployment on Kubernetes

This repository contains Kubernetes manifests for deploying a complete **EFK (Elasticsearch, Fluentd, Kibana)** logging stack along with a sample **Spring Boot application**. The setup is structured for **best practices**, supports **persistent storage**, and is ready for **GitOps workflows** using tools like Kustomize, ArgoCD, or GitHub Actions.

## 📁 EFK Deployment Structure

```
├── base/                   
│   ├── elasticsearch/
│   │   ├── statefulset.yml
│   │   └── service.yml
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
│   ├── db/
│   │   ├── pv.yml
│   │   ├── pvc.yml
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
git clone https://github.com/MBA90/efk.git
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

---

## 📬 Interact with the Spring Boot API

This project includes a Spring Boot application that exposes a RESTful **User Management API**, backed by MongoDB.

To make testing easier, you can use the included **Postman collection** to interact with the API.

### 🔗 Download the Collection

You can find the Postman collection here:

**[🌐 User API Postman Collection](https://mohammadabdelhadi.postman.co/workspace/7c1783da-7b23-4b57-82fd-3a052edc7031/collection/45174298-a01dadf5-5c28-4273-abd5-0bced7d5136b?action=share&source=collection_link&creator=45174298)**

> Or use the file: `User API.postman_collection.json` provided in this repository (if available in the repo).

### 🚀 Sample Endpoints Included

| Method | Endpoint                                      | Description                        |
|--------|-----------------------------------------------|------------------------------------|
| GET    | `/api/users`                                  | List all users                     |
| POST   | `/api/users`                                  | Create a new user                  |
| GET    | `/api/users/{id}`                             | Get user by ID                     |
| GET    | `/api/users/by-username/{username}`           | Get user by username               |
| GET    | `/api/users/by-ref/{refNo}`                   | Get user by reference number       |
| GET    | `/api/users/by-gender/{gender}`               | Filter users by gender             |
| GET    | `/api/users/by-min-age/{age}`                 | Filter users by minimum age        |
| GET    | `/api/users/search?name={fullName}`           | Search users by full name          |
| PUT    | `/api/users/{id}`                             | Update user by ID                  |
| DELETE | `/api/users/by-ref/{refNo}`                   | Delete user by reference number    |

> All endpoints assume the app is accessible at `http://localhost:8080`. If you're using Minikube, see the port-forward instructions above.


## 📊 Verifying Logs in Kibana

Once your EFK stack is deployed and the Spring Boot app is running, you can verify that logs are successfully collected and visualized in **Kibana**.

### ✅ Steps to Test Logs in Kibana

1. **Access Kibana**

If you're using Minikube, use:

```bash
kubectl port-forward svc/kibana -n efk 5601:5601
```

Then open:
```
http://localhost:5601
```

---

2. **Create an Index Pattern**

Kibana needs an index pattern to display logs coming from Elasticsearch.

Steps:
- Go to **Kibana UI → Management → Stack Management → Index Patterns**
- Click **Create index pattern**
- Enter the index pattern: `logstash-*` or `fluentd-*` (depending on your Fluentd config)
- Select `@timestamp` as the time field
- Click **Create index pattern**

> If no indices are found, it means Fluentd hasn’t forwarded any logs yet — make sure the Spring Boot app has generated logs.

---

3. **Generate Application Logs**

You can trigger logs in your Spring Boot app by hitting its API. For example:

```bash
kubectl port-forward svc/efk-springboot-svc -n efk 8080:8080
```

Then call:

```bash
curl http://localhost:8080/api/users
```

This will generate logs like:

```json
📣 User created with userRefNo: USR-20250726-008 
```

These logs are collected by **Fluentd**, sent to **Elasticsearch**, and can be visualized in **Kibana**.

---

4. **View Logs in Kibana Discover Tab**

- Go to **Kibana → Discover**
- Select the index pattern you created (`logstash-*`)
- You should now see structured log entries coming from your Spring Boot app
- Use filters like `level.keyword`, `logger_name`, or `message` to drill into log details

---

### ✅ Troubleshooting

| Issue | Fix |
|-------|-----|
| No index pattern found | Ensure logs are reaching Elasticsearch (check Fluentd logs) |
| No logs displayed | Make sure app is generating logs, Fluentd is running, and `logstash_format` is `true` |
| Incorrect timestamps | Ensure Fluentd is parsing `@timestamp` correctly in its config |

---

