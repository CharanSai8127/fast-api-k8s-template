# 🚀 GitOps-Driven Kubernetes Platform

Production-grade Kubernetes platform enabling multi-environment deployments using GitOps principles with Argo CD and Kustomize.

---

## 🧠 Problem

Managing Kubernetes deployments across environments often leads to:
- Configuration duplication  
- Inconsistent environments  
- Manual deployment errors  

---

## ✅ Solution

Designed a GitOps-driven platform that:
- Separates application definition from environment configuration  
- Uses Argo CD for continuous reconciliation  
- Enables scalable and consistent deployments across dev, stage, and prod  

---

## 🏗️ Architecture Overview

The system is structured into three layers:

### 🔹 Application Layer
- Frontend (UI)
- Backend (API)
- PostgreSQL
- Prestart Job

### 🔹 Environment Layer (Kustomize Overlays)

| Environment | Features |
|------------|--------|
| Dev | Minimal setup |
| Stage | HPA, PVC, Network Policies |
| Prod | Gateway API, Monitoring, Stateful DB |

### 🔹 Platform Layer
- Argo CD (GitOps controller)
- Prometheus, Grafana (observability)
- Gateway API (traffic routing)

---

## 🔁 System Flow
Code → Git → Argo CD → Kustomize → Kubernetes → Services → Gateway → Users → Metrics → Alerts


---

## ⚙️ Key Implementations

### 🔹 GitOps Deployment
- Git as the single source of truth  
- Argo CD ensures desired state is maintained  

### 🔹 Environment Separation
- Kustomize overlays for dev, stage, prod  
- No duplication of manifests  

### 🔹 Observability
- Prometheus for metrics  
- Grafana dashboards  
- Alertmanager for alerts  

### 🔹 Networking
- Gateway API for production traffic routing  
- Network Policies for controlled communication  

### 🔹 Storage
- PVC in stage  
- StatefulSet in production  

---

## 📁 Repository Structure

---

## 🎛️ Control Model

| Layer | Responsibility |
|------|---------------|
| Git | Source of truth |
| Argo CD | State reconciliation |
| Kustomize | Environment config |
| Kubernetes | Runtime execution |
| Gateway API | Traffic routing |
| Observability | Feedback |

---

## 📊 Runtime Behavior

- Self-healing via Kubernetes restart policies  
- Rolling deployments via Argo CD  
- HPA-based scaling in stage and prod  
- Metrics-driven observability and alerting  

---

## ⚖️ Trade-offs & Improvements

- Reactive scaling (CPU-based)  
- Single-instance database  
- No canary deployments  
- Limited traffic control (no rate limiting)  

---

## 🎯 Outcome

- Consistent multi-environment deployments  
- Fully automated GitOps workflow  
- Observable and controlled system behavior  
- Clear separation between build, deploy, and runtime layers  

---

## 💬 Summary

This project demonstrates how GitOps, environment isolation, and observability can be combined to build a reliable and scalable Kubernetes platform.
