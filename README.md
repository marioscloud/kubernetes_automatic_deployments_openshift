# ☸️ Kubernetes Automated Deployment & Rollout Strategies

![Kubernetes](https://img.shields.io/badge/Kubernetes-%23326ce5.svg?style=for-the-badge&logo=kubernetes&logoColor=white)
![OpenShift](https://img.shields.io/badge/OpenShift-%23EE0000.svg?style=for-the-badge&logo=redhat&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=nodedotjs&logoColor=white)
![Lifecycle Management](https://img.shields.io/badge/App_Lifecycle-Zero_Downtime-success?style=for-the-badge)

> **Enterprise Container Orchestration:** A practical implementation of highly available, multi-replica containerized deployments featuring zero-downtime rolling updates and declarative health checks.

## 📖 Executive Summary

This project demonstrates advanced **Application Lifecycle Management (ALM)** on Kubernetes / Red Hat OpenShift. It focuses on how declarative configuration ensures **High Availability (HA)** and resilient infrastructure by automating version upgrades with zero service interruption. 

By leveraging native Kubernetes `Deployment` controllers and implementing strategic `readinessProbes`, this repository showcases the fundamental DevOps practices required to safely transition distributed microservices from v1.0 to v2.0 in a production-simulated environment.

---

## 🎯 Key DevOps Concepts Demonstrated

* **Zero-Downtime Upgrades:** Executing `RollingUpdate` strategies to seamlessly transition user traffic between application versions.
* **Infrastructure Reliability (Health Checks):** Implementing `readinessProbes` to guarantee the ingress controller only routes traffic to healthy, fully initialized containers.
* **Declarative State Management:** Utilizing Kubernetes controllers to continuously reconcile the desired state (Replica Management & Self-Healing).
* **Observability:** Monitoring pod lifecycles and rollout status in real-time.

---

## 🛠️ Technology Stack & Prerequisites

* **Orchestration:** Kubernetes or Red Hat OpenShift Cluster
* **CLI Tools:** `kubectl` (or `oc`)
* **Application:** Containerized Node.js Microservice (`do100-multi-version`)

**Cluster Preparation:**
Ensure you are authenticated to your cluster and have set your working namespace:
```bash
kubectl config set-context --current --namespace=<your-target-namespace>
