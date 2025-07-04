
# EKS Shopping Platform with Microservices, Observability & Service Mesh

This repository provisions a robust AWS EKS infrastructure and deploys a production-like shopping application with 11 microservices. The platform includes:

- A secure, highly-available **EKS cluster** with managed node groups
- Networking resources (VPC, subnets, security groups) configured for best practices
- Cluster autoscaling with **Karpenter**
- Istio **service mesh** for traffic management and observability
- Centralized monitoring with **Prometheus & Grafana**
- Distributed tracing with **Jaeger**
- Centralized logging with **Loki**
- **GitOps workflow** with Argo CD for continuous delivery

All infrastructure components are automated using **Terraform**, enabling reproducible and scalable deployments.

This setup provides a complete Kubernetes platform for running microservices with end-to-end observability, security, and CI/CD capabilities.

## Repository structure

```plaintext
.
├── .gitignore
├── .pre-commit-config.yaml
├── README.md
├── app
│   ├── __pycache__
│   │   └── main.cpython-311.pyc
│   ├── main.py
│   └── requirements.txt
├── aquasecurity.github.io_clustercompliancereports.yaml.txt
├── manifests
│   ├── argo-cd
│   │   ├── kube-prometheus-application.yaml
│   │   └── shopping-application.yaml
│   ├── istio
│   │   └── frontend-gateway.yaml
│   ├── jaeger
│   │   └── simplest.yaml
│   ├── shopping
│   │   └── shopping-all.yaml
│   └── website
│       ├── website-deployment.yaml
│       └── website-service.yaml
├── repo-structure.txt
├── terraform
│   ├── .gitignore
│   ├── .terraform.lock.hcl
│   ├── main.tf
│   ├── provider.tf
│   ├── terraform.tfstate
│   ├── terraform.tfstate.backup
│   ├── variables.tf
│   └── versions.tf
├── terraform.tfstate
└── test-secrets.env


## EKS Platform Deployment Guide

### Prerequisites

- **Terraform** >= 1.3.0  
- **kubectl**  
- **helm**  
- **AWS CLI** (with credentials configured)  

---

## Getting Started

### Clone the Repository

```bash
git clone https://github.com/KeerthanaGoka/website.git
cd website
```

### Provision AWS Infrastructure

```bash
cd terraform
terraform init
terraform apply
```

### Configure kubectl for EKS

```bash
aws eks --region <your-region> update-kubeconfig --name <eks-cluster-name>
```

### Install Istio

```bash
istioctl install --set profile=demo -y
```

### Deploy Prometheus & Grafana

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm install prometheus prometheus-community/kube-prometheus-stack -n monitoring --create-namespace
```

### Deploy Argo CD

```bash
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

### Deploy Your Website

```bash
kubectl apply -f manifests/shopping-deployment.yaml
```

---

## Observability Dashboards

### Grafana (Metrics Dashboard)

```bash
kubectl -n monitoring port-forward svc/prometheus-grafana 3000:80
```

Access Grafana at: [http://localhost:3000](http://localhost:3000)

---

### Jaeger (Tracing UI)

```bash
kubectl -n istio-system port-forward svc/jaeger 16686:16686
```

Access Jaeger at: [http://localhost:16686](http://localhost:16686)

---

### Argo CD (GitOps Dashboard)

```bash
kubectl -n argocd port-forward svc/argocd-server 8080:443
```

Access Argo CD at: [http://localhost:8080](http://localhost:8080)

---

## Security Considerations

Make sure EKS node security groups allow inbound traffic for ports required by Istio (e.g., 15012, 15017), Grafana (3000), Loki (3100), Jaeger (16686), etc.

Use IAM best practices with your Terraform AWS provider to avoid security risks.

---

## Contributing

Contributions are welcome! Feel free to open issues or submit pull requests to improve this project.

---

## License

This project is licensed under the **MIT License**.

---

## Author

**Keerthana Goka**  
[GitHub Profile](https://github.com/KeerthanaGoka)
