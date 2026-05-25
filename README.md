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
```

## 🚀 Deployment Runbook

### Phase 1: Initializing the HA Deployment (v1.0)
Provision the initial infrastructure state by deploying 5 replicas of the application to ensure high availability.

```bash
kubectl create deployment do100-multi-version \
  --replicas=5 \
  --image=quay.io/redhattraining/do100-multi-version:v1-external
```

**Verification:** Confirm the pods have reached a `Running` state and inspect the logs to verify the active version.

```bash
kubectl get pods
kubectl logs deploy/do100-multi-version
# Expected output: "do100-multi-version server running version 1.0"
```

### Phase 2: Zero-Downtime Upgrade & Health Check Implementation (v2.0)
To simulate a production upgrade, we update the application image and introduce a Readiness Probe. This prevents Kubernetes from terminating old pods until the new pods are strictly ready to accept traffic.

Confirm the rollout strategy is set to `RollingUpdate`:
```bash
kubectl describe deploy/do100-multi-version | grep StrategyType
```

Edit the deployment manifest on the fly:
```bash
kubectl edit deployment/do100-multi-version
```

**Required Manifest Modifications:**
Update the image tag to `v2-external` and append the `readinessProbe` block to the container spec:
```yaml
    containers:
    - name: do100-multi-version
      image: quay.io/redhattraining/do100-multi-version:v2-external
      readinessProbe:
        httpGet:
          path: /ready
          port: 8080
        initialDelaySeconds: 2
        timeoutSeconds: 2
```

### Phase 3: Rollout Telemetry & Verification
Monitor the Kubernetes scheduler as it orchestrates the rolling update. You will observe the controller terminating v1.0 pods only after v2.0 pods pass their HTTP health checks.

```bash
# Watch the rollout in real-time
kubectl get pods -w
```
*(Press `Ctrl+C` to exit the watch stream once all new pods are running).*

**Final Validation:** Ensure traffic is now being served by the upgraded microservice.
```bash
kubectl logs deploy/do100-multi-version
# Expected output: "do100-multi-version server running version 2.0"
```

### Phase 4: Teardown & Resource Cleanup
Maintain cluster hygiene by removing the deployment. The controller will automatically terminate all orphaned pods.
```bash
kubectl delete deploy/do100-multi-version
```

---

## 📂 Repository Structure

| File / Directory | Description |
| :--- | :--- |
| `deploy_do100_multi_version.yml` | Declarative YAML manifest for versioned infrastructure rollout. |
| `screenshots_exercise/` | Visual documentation and terminal outputs from the rollout process. |
| `README.md` | Comprehensive project documentation and execution runbook. |

---

## 🏆 Acknowledgments & Credits

This architectural exercise is adapted from enterprise training materials provided by:
* **Red Hat Training & Certification:** *Managing Cloud‑Native Applications with Kubernetes (DO100)*
