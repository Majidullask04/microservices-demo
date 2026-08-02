# End-to-End DevSecOps & GitOps Microservices Project

## Overview

This project demonstrates a complete DevSecOps and GitOps pipeline for deploying a cloud-native microservices application on Kubernetes.

The project includes:

* Jenkins CI/CD Pipeline
* Docker Containerization
* Docker Hub Registry
* SonarQube Code Quality Analysis
* Gitleaks Secret Scanning
* Trivy Vulnerability Scanning
* K3s Kubernetes Cluster
* Argo CD GitOps Deployment
* Prometheus Monitoring
* Grafana Visualization

The application deployed is the Google Online Boutique Microservices Demo.

---

## Architecture

GitHub → Jenkins → Docker Build → Docker Hub → Argo CD → K3s Kubernetes → Prometheus → Grafana

---

## Tools Used

| Category         | Tools            |
| ---------------- | ---------------- |
| Source Control   | Git, GitHub      |
| CI/CD            | Jenkins          |
| Security         | Gitleaks, Trivy  |
| Code Quality     | SonarQube        |
| Containerization | Docker           |
| Registry         | Docker Hub       |
| Orchestration    | Kubernetes (K3s) |
| GitOps           | Argo CD          |
| Monitoring       | Prometheus       |
| Visualization    | Grafana          |

---

## Microservices

* Frontend
* Ad Service
* Cart Service
* Checkout Service
* Currency Service
* Email Service
* Payment Service
* Product Catalog Service
* Recommendation Service
* Shipping Service
* Redis Cart

---

## CI/CD Pipeline Stages

### 1. Source Code Checkout

Jenkins pulls source code from GitHub.

### 2. Secret Detection

Gitleaks scans repository for exposed credentials and secrets.

### 3. Static Code Analysis

SonarQube performs code quality and security analysis.

### 4. Vulnerability Scanning

Trivy scans source code and container images.

### 5. Docker Build

Container images are built for all microservices.

### 6. Docker Push

Images are pushed to Docker Hub.

### 7. Kubernetes Deployment

Application is deployed to K3s Kubernetes Cluster.

### 8. GitOps Synchronization

Argo CD continuously syncs Kubernetes manifests from GitHub.

---

## Kubernetes Namespace

```bash
kubectl create namespace microservices
```

Deployment:

```bash
kubectl apply -n microservices -k kubernetes-manifests
```

---

## Monitoring

### Prometheus

Collects:

* CPU Usage
* Memory Usage
* Pod Health
* Kubernetes Metrics

### Grafana

Visualizes:

* Cluster Health
* Namespace Metrics
* Application Performance

---

## Argo CD

Application deployment is managed using GitOps.

Features:

* Automatic Sync
* Self Healing
* Drift Detection
* Rollback Support

---

## Learning Outcomes

* CI/CD Pipeline Design
* Kubernetes Administration
* Docker Image Management
* GitOps Workflows
* Security Scanning
* Monitoring and Observability
* Troubleshooting Production Deployments

---

## Author

Majidulla SK

GitHub: https://github.com/Majidullask04

# 3-Tier Microservices DevSecOps Platform

## Overview
End-to-end CI/CD pipeline for a 10+ microservices application deployed on K3s 
with automated security scanning, GitOps delivery, and full observability 
(Prometheus + Grafana + Loki).

## Architecture
![Architecture Diagram](<./archictur diagram.png>)

## Tech Stack
| Layer | Tools |
|-------|-------|
| Orchestration | K3s, kubectl |
| CI/CD | Jenkins (Declarative Pipeline) |
| GitOps | ArgoCD |
| Security | SonarQube, Trivy, Gitleaks |
| Monitoring | Prometheus, Grafana, Loki |
| Registry | Docker Hub |

## Screenshots & Dashboard Evidence
- **Architecture**: ![Architecture Diagram](<./archictur diagram.png>)
- **Jenkins CI/CD Pipeline**: ![Jenkins Pipeline](<./jenkins-pipeline.png>)
- **SonarQube Analysis**: ![SonarQube Analysis](<./sonar-qube analysis.png>)
- **ArgoCD GitOps Tree**: ![ArgoCD Tree](<./argocd-tree.png>)
- **K3s Kubernetes Manifests & Pods**: ![Kubernetes Manifests](<./kubernetes-manifests.png>)
- **Grafana Monitoring Dashboard**: ![Grafana Dashboard](<./grafana-dashboard.png>)
- **Loki Log Aggregation**: ![Loki Logs](<./loki monotering log .png>)
- **Jaeger Distributed Tracing**: ![Jaeger Tracing](<./jaeger.png>)
- **Local Microservices Application**: ![Local Microservice Running](<./local micro-service running .png>)


## Pipeline Stages
1. Git Checkout → 2. Secret Scan (Gitleaks) → 3. SAST (SonarQube) 
→ 4. Quality Gate → 5. FS Scan (Trivy) → 6. Build & Push Docker Images 
→ 7. Deploy to K3s

## What I Learned
- Debugging K3s networking and ArgoCD sync issues
- Integrating security gates without breaking delivery speed
- Setting up Loki log aggregation for multi-service debugging

## Quick Start
[Keep this short — 5 commands max]
        

# End-to-End DevSecOps & GitOps Architecture

```mermaid
graph TD
    %% Styling
    classDef default fill:#f9f9f9,stroke:#cfd8dc,stroke-width:2px,color:#263238,rx:5,ry:5;
    classDef primary fill:#e3f2fd,stroke:#1976d2,stroke-width:2px,color:#0d47a1,rx:8,ry:8;
    classDef gitops fill:#e1f5fe,stroke:#0288d1,stroke-width:2px,color:#01579b,rx:8,ry:8;
    classDef security fill:#f3e5f5,stroke:#8e24aa,stroke-width:2px,color:#4a148c,rx:8,ry:8;
    classDef k8s fill:#e8f5e9,stroke:#388e3c,stroke-width:2px,color:#1b5e20,rx:8,ry:8;
    classDef observability fill:#fff3e0,stroke:#f57c00,stroke-width:2px,color:#e65100,rx:8,ry:8;
    classDef registry fill:#fff8e1,stroke:#ffc107,stroke-width:2px,color:#ff6f00,rx:8,ry:8;
    classDef svc fill:#ffffff,stroke:#4caf50,stroke-width:2px,color:#1b5e20,rx:5,ry:5;
    classDef userNode fill:#ffebee,stroke:#d32f2f,stroke-width:2px,color:#c62828,rx:20,ry:20;

    %% 1. Source Control
    subgraph Source_Control ["📁 1. Source Control"]
        style Source_Control fill:#f8f9fa,stroke:#cfd8dc,stroke-width:2px,rx:10,ry:10
        GH["🐙 GitHub"]:::primary
    end

    %% 2. Jenkins CI Pipeline
    subgraph CI_Pipeline ["⚙️ 2. Jenkins CI Pipeline"]
        style CI_Pipeline fill:#f8f9fa,stroke:#cfd8dc,stroke-width:2px,rx:10,ry:10
        direction TB
        Jenkins("🤵 Jenkins"):::primary
        Checkout("📥 Checkout"):::primary
        Gitleaks("🔍 Gitleaks<br/>Secret Scanning"):::security
        SonarQube("🔬 SonarQube<br/>Static Analysis"):::security
        Trivy("🛡️ Trivy<br/>Vuln Scanning"):::security
        Build("🐳 Docker Build"):::primary
        Push("🚀 Docker Push"):::primary
        
        Jenkins --> Checkout --> Gitleaks --> SonarQube --> Trivy --> Build --> Push
    end

    %% 3. Image Registry
    subgraph Image_Registry ["📦 3. Image Registry"]
        style Image_Registry fill:#f8f9fa,stroke:#cfd8dc,stroke-width:2px,rx:10,ry:10
        DockerHub("🐳 Docker Hub"):::registry
    end

    %% 4. GitOps
    subgraph GitOps ["🦑 4. GitOps"]
        style GitOps fill:#f8f9fa,stroke:#cfd8dc,stroke-width:2px,rx:10,ry:10
        ArgoCD("🐙 Argo CD<br/>Auto Sync & Drift Detection"):::gitops
    end

    %% End Users & External
    Users(("👤 End Users")):::userNode

    %% 5. Kubernetes Cluster (K3s)
    subgraph K3s_Cluster ["☸️ 5. Kubernetes Cluster (K3s)"]
        style K3s_Cluster fill:#f1f8e9,stroke:#aed581,stroke-width:2px,rx:10,ry:10
        direction TB
        
        subgraph Platform ["Platform Services"]
            style Platform fill:#e8f5e9,stroke:#81c784,stroke-width:1.5px,rx:8,ry:8
            Ingress("🌐 NGINX Ingress"):::k8s
            CoreDNS("🧭 CoreDNS"):::k8s
            Metrics("📈 Metrics Server"):::k8s
        end

        subgraph Microservices ["Namespace: microservices"]
            style Microservices fill:#e8f5e9,stroke:#81c784,stroke-width:1.5px,rx:8,ry:8
            Frontend("🖥️ Frontend"):::svc
            Catalog("📋 Product Catalog"):::svc
            Cart("🛒 Cart Service"):::svc
            CheckoutSvc("💳 Checkout Service"):::svc
            Payment("💰 Payment Service"):::svc
            Shipping("🚚 Shipping Service"):::svc
            Recommendation("✨ Recommendation"):::svc
            Currency("💱 Currency Service"):::svc
            Email("📧 Email Service"):::svc
        end

        subgraph Observability ["Observability Stack"]
            style Observability fill:#fff3e0,stroke:#ffb74d,stroke-width:1.5px,rx:8,ry:8
            Prom("📊 Prometheus"):::observability
            Grafana("📉 Grafana"):::observability
            Loki("📜 Loki"):::observability
            Promtail("🔗 Promtail"):::observability
        end
        
        subgraph Additional_Tools ["Security & Tracing"]
            style Additional_Tools fill:#f3e5f5,stroke:#ba68c8,stroke-width:1.5px,rx:8,ry:8
            Jaeger("🕵️ Jaeger Tracing"):::security
            Falco("🦅 Falco Runtime Security"):::security
        end
    end
    
    %% Core Flow Connections
    GH -->|Webhook/Poll| Jenkins
    Push -->|Push Images| DockerHub
    DockerHub -.->|Pull Images| ArgoCD
    ArgoCD ==>|Deploy & Sync| K3s_Cluster
    
    %% Internal K8s Connections
    Users ==>|Access Application| Ingress
    Ingress -.->|Routes Traffic| Microservices

    %% Link styles
    linkStyle 0 stroke:#1976d2,stroke-width:2px;
    linkStyle 1,2,3,4,5,6 stroke:#1976d2,stroke-width:2px;
    linkStyle 7 stroke:#ffc107,stroke-width:2px;
    linkStyle 8 stroke:#0288d1,stroke-width:2px,stroke-dasharray: 5 5;
    linkStyle 9 stroke:#388e3c,stroke-width:3px;
    linkStyle 10 stroke:#d32f2f,stroke-width:3px;
    linkStyle 11 stroke:#4caf50,stroke-width:2px,stroke-dasharray: 5 5;
```