# efk
Elasticsearch Fluentd Kibana EFK Stack Deployment

Deployment Structre 

efk-deployment/
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
│   └── kustomization.yaml    
├── overlays/
│   └── dev/
│       ├── kustomization.yaml    # Optional: adds labels, patches, image tags
