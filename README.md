# Kubernetes Automatic Deployment Strategies  
### Managed Multi‑Replica Node.js Application on Kubernetes / OpenShift

## Overview  
This project demonstrates how to deploy, update, and manage a multi‑replica containerized application using Kubernetes Deployment strategies. It highlights how Kubernetes ensures high availability, rolling updates, and zero‑downtime version upgrades.

The application used is a Node.js demo service (`do100-multi-version`) provided by Red Hat Training.

---

## Objectives  
This project showcases how to:

- Deploy a containerized application with multiple replicas  
- Inspect and understand a Deployment manifest  
- Perform a rolling update to a new application version  
- Add a readiness probe to control rollout behavior  
- Observe Kubernetes automatically replace old pods with new ones  
- Clean up resources after deployment  

---

## Prerequisites  
Before starting, ensure you have:

- A working Kubernetes or OpenShift cluster  
- `kubectl` (or `oc`) installed and authenticated  
- Permissions to deploy into your namespace  
- A valid Kubernetes context pointing to your working namespace  

Set your namespace:

```bash
kubectl config set-context --current --namespace=<your-namespace>
1. Deploy the Application (Version 1.0)
Create a Deployment with 5 replicas:

bash
kubectl create deployment do100-multi-version \
  --replicas=5 \
  --image=quay.io/redhattraining/do100-multi-version:v1-external
Verify pods are running:

bash
kubectl get pods
Check logs to confirm version:

bash
kubectl logs deploy/do100-multi-version
Expected output includes:

Code
do100-multi-version server running version 1.0
2. Update Deployment to Version 2.0 and Add a Readiness Probe
Confirm the Deployment strategy:

bash
kubectl describe deploy/do100-multi-version
You should see:

Code
StrategyType: RollingUpdate
Edit the Deployment:

bash
kubectl edit deployment/do100-multi-version
Update:

Image → v2-external

Add readiness probe

Example container spec:

yaml
containers:
- name: do100-multi-version
  image: quay.io/redhattraining/do100-multi-version:v2-external
  readinessProbe:
    httpGet:
      path: /ready
      port: 8080
    initialDelaySeconds: 2
    timeoutSeconds: 2
Save and exit.

3. Observe the Rolling Update
Watch pods as Kubernetes performs the rollout:

bash
kubectl get pods -w
You will see:

New pods created with the new version

Old pods terminated only after new pods become Ready

Stop watching with Ctrl+C.

Check logs again:

bash
kubectl logs deploy/do100-multi-version
Expected output:

Code
do100-multi-version server running version 2.0
Cleanup
Delete the Deployment:

bash
kubectl delete deploy/do100-multi-version
Kubernetes automatically removes all associated pods.

Project Structure
File	Description
deploy_do100_multi_version.yml	Deployment manifest used for versioned rollout
screenshots_exercise/	Reference screenshots from the guided exercise
README.md	Project documentation


Key Concepts Demonstrated
RollingUpdate Strategy
Ensures zero‑downtime upgrades by gradually replacing pods.

Readiness Probes
Guarantee that traffic is only routed to healthy containers.

Replica Management
Maintains application availability even during updates.

Declarative Deployment
Kubernetes continuously reconciles the desired state.

Credits
This project is based on training materials from:

Red Hat Training & Certification  
Managing Cloud‑Native Applications with Kubernetes (DO100)
