# 🚀 GitOps-Driven Kubernetes Platform
## Multi-Environment Deployment with Kustomize, Argo CD, and Observability

---

## 🧩 Overview

This project demonstrates the design and implementation of a **GitOps-based Kubernetes platform** that enables consistent application deployments across multiple environments while allowing controlled configuration differences.

The platform separates **application definition from environment-specific behavior**, ensuring reproducibility, scalability, and operational visibility.

---

## 🎯 Objectives

- Eliminate duplication of Kubernetes manifests  
- Enable environment-specific customization  
- Ensure declarative and automated deployments  
- Introduce production-grade features progressively  
- Provide observability and failure visibility  

---

## 🏗️ Architecture

The system is structured into three layers to separate **definition, customization, and runtime behavior**:

### 🔹 Application Layer (Base)
Defines core services:
- Frontend (UI)
- Backend (API)
- PostgreSQL (Database)
- Prestart Job (Initialization)

### 🔹 Environment Layer (Overlays)
Applies environment-specific behavior:

| Environment | Characteristics |
|------------|----------------|
| **Dev** | Minimal setup, no scaling |
| **Stage** | HPA, PVC, Network Policies |
| **Prod** | Gateway API, Stateful DB, Monitoring |

### 🔹 Platform Layer
Provides operational capabilities:
- Argo CD (GitOps controller)
- Prometheus, Grafana, Alertmanager (observability)
- Gateway API (traffic routing)



The CI pipeline is responsible for building, testing, scanning, and packaging the application into a container image.

**Flow:**
- Code is pushed to GitHub  
- Jenkins triggers the pipeline  
- Static analysis (SonarQube) and dependency checks (OWASP) are performed  
- Filesystem and image scans (Trivy) ensure security  
- Docker image is built and pushed to Docker Hub  

👉 Ensures **code quality, security, and artifact readiness for deployment**

---

### 🔹 CD / GitOps Architecture (Argo CD + Kustomize)

The CD layer uses GitOps principles to manage deployments across environments.

**Flow:**
- Git acts as the source of truth (base + overlays)  
- Argo CD detects changes and syncs desired state  
- Kustomize builds environment-specific manifests  
- Applications are deployed to dev, stage, and prod  
- Gateway API manages traffic routing (prod)  
- Monitoring stack provides observability  

👉 Ensures **declarative control, environment isolation, and automated deployments**

---

> This architecture demonstrates a clear separation between **build (CI), deployment control (CD), and runtime behavior**, enabling independent evolution of each layer.

> The system separates **build concerns (CI)** from **deployment control (CD)**, enabling independent scaling and reliability.

---

## 📁 Repository Structure

The repository is organized to clearly separate **deployment control** from **application configuration**.

### 🔹 Argo CD (GitOps Control Layer)

`argocd/`

Defines how applications are deployed into the cluster using GitOps.

- `python-appset.yaml` → Deploys application across environments  
- `gateway-appset.yaml` → Manages Gateway API resources  
- `monitoring.yaml` → Deploys observability stack  

👉 Acts as the **deployment control plane**, ensuring cluster state matches Git.

---

### 🔹 Kubernetes Manifests (System Definition)

`k8s/`

Contains all Kubernetes manifests structured using Kustomize.

#### 🔸 Base (`k8s/base/`)
Defines core application components:
- `frontend/`, `backend/`, `database/`, `prestart/`  
- Common manifests reused across environments  

---

#### 🔸 Overlays (`k8s/overlays/`)
Defines environment-specific behavior:

- `dev/` → Minimal configuration  
- `stage/` → Adds HPA, PVC, Network Policies  
- `prod/` → Full production setup (Gateway, StatefulSet, Monitoring integration)  

Each overlay customizes:
- ConfigMaps & Secrets  
- Scaling (HPA)  
- Storage (PVC / StatefulSet)  
- Network policies  

---

#### 🔸 Monitoring (`k8s/monitoring/`)
Defines observability stack:

- Prometheus (metrics collection)  
- Grafana (dashboards)  
- Alertmanager (alerts)  
- Exporters (Postgres, Kubernetes events)  

👉 Provides **system visibility and alerting**.

---

## 🔁 System Flow:
   Code -> Git -> ArgoCD -> Kustomize -> Kubernetes -> Services -> Gateway ->
   Users -> Metrics -> Alerts


### Flow Explanation:

1. Application manifests are stored in Git (source of truth)
2. Argo CD detects changes and syncs cluster state
3. Kustomize builds environment-specific configurations
4. Kubernetes deploys application workloads
5. Traffic is routed via Gateway API (production)
6. Metrics are collected and visualized
7. Alerts are triggered on failures

---

## 🎛️ Control Model

System behavior is controlled declaratively through a layered control model, ensuring consistency, predictability, and elimination of manual drift:

| Layer | Responsibility |
|------|---------------|
| **Git** | Source of truth for all configurations |
| **Argo CD** | Continuous reconciliation of desired state |
| **Kustomize** | Environment-specific configuration management |
| **Kubernetes** | Runtime enforcement (scaling, health, resources) |
| **Gateway API** | Traffic routing control |
| **Observability** | Feedback and operational insight |

---

## ⚙️ Runtime Behavior

### 🔹 Pod Failure
- Kubernetes restarts failed pods (self-healing)
- Readiness probes prevent unhealthy pods from receiving traffic
- Service dependencies may cause partial degradation
- Recovery may introduce temporary load spikes due to lack of back-pressure mechanisms

---

### 🔹 Configuration Changes
- Managed via Git and applied through Argo CD
- Triggers rolling updates of pods
- Invalid configurations result in failed readiness checks
- Mitigated through version control and review processes

---

### 🔹 Traffic Scaling

| Environment | Behavior |
|------------|--------|
| **Dev** | No autoscaling |
| **Stage** | HPA based on CPU utilization |
| **Prod** | HPA combined with traffic routing and observability |

- Scaling is reactive (metric-based)
- Scale-out is fast; scale-in requires careful control

---

### 🔹 Deployment Behavior
- Git changes trigger Argo CD sync (~polling interval)
- Rolling updates ensure gradual deployment
- Prestart job executes initialization logic
- Failures are surfaced via health status and metrics

---

### 🔹 Database Behavior

| Environment | Implementation |
|------------|--------------|
| **Base** | Ephemeral database |
| **Stage** | Persistent Volume (PVC) |
| **Prod** | StatefulSet with stable storage |

- Backend communicates with database (not storage directly)
- No replication configured (single database instance)

---

## 📊 Observability

The platform includes a complete monitoring stack:

- **Prometheus** – Metrics collection
- **Grafana** – Visualization dashboards
- **Alertmanager** – Alerting (email notifications)
- **Exporters** – PostgreSQL and Kubernetes events

This enables:
- Real-time system visibility
- Performance monitoring
- Failure detection and alerting

---

## ⚖️ Design Trade-offs & Future Enhancements

The system is designed with simplicity and control as primary goals. As a result, the following trade-offs exist:

- **Scaling** is CPU-based and reactive; request-based scaling can improve responsiveness
- **Deployment strategy** uses rolling updates; canary or blue-green strategies can reduce risk
- **Configuration validation** is manual; automated validation can improve safety
- **Database** is a single instance; replication can improve availability
- **Traffic control** lacks rate limiting and back-pressure mechanisms

These areas provide clear opportunities for future enhancement as the system evolves.

---

## 💬 Summary

This project demonstrates a **GitOps-driven platform architecture** that ensures consistent deployments, controlled configuration, and observable system behavior across multiple environments.

It highlights how **declarative control, environment isolation, and runtime awareness** can be combined to build a reliable cloud-native system.

> The system is designed using a **Design → Control → Behavior** model, ensuring it remains stable under change and resilient under failure.
