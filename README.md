# efk
Elasticsearch Fluentd Kibana EFK Stack Deployment

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
