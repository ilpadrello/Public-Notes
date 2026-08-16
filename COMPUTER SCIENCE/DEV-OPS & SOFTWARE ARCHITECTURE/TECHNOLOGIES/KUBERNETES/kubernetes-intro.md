---
title: Kubernetes Introduction
aliases:
  - k8s
  - kubernetes
---
TLDR : [[TLDR|k8s-tldr]]
# Why Kubernetes, and what problem it solves ?
Before Kubernetes, deploying software usually meant running applications directly on physical or virtual machines. As applications grew into microservices, managing them manually or with simple scripts created major operational headaches.

Kubernetes (K8s) was created by Google (inspired by their internal system, Borg) to solve the core challenges of running containerized applications at scale.
## The Big Picture Takeaway

Kubernetes is essentially an **operating system for a cluster of machines**. Just as a desktop OS manages CPU, RAM, and storage across local hardware processes, Kubernetes abstracts a fleet of servers into a single compute pool and handles scheduling, networking, and resilience automatically.
## 1. The Core Problems Kubernetes Solves

### The "Pet vs. Cattle" Infrastructure Problem

- **Without K8s:** Servers were treated like "pets." If Server A crashed, an engineer had to ssh in, fix dependencies, or manually redeploy the application.
- **With K8s:** Infrastructure is treated like "cattle." Nodes are interchangeable. If a server dies, Kubernetes automatically reschedules your containers onto a healthy node without human intervention.
### Declarative State vs. Imperative Management

- **Without K8s:** You wrote scripts to deploy, restart, or scale apps (_"Do step 1, then step 2, then step 3"_). If step 2 failed midway, your environment was left in a broken, unpredictable state.
- **With K8s:** You define the **desired state** in YAML (_"I want 3 instances of my API running on port 8080"_). K8s runs a perpetual control loop: it constantly compares actual state to desired state and fixes discrepancies automatically.
### Resource Utilization Efficiency

- **Without K8s:** Applications were often over-provisioned on dedicated VMs to handle peak traffic, wasting CPU and memory during downtime.
- **With K8s:** It packs containers tightly onto nodes based on CPU/RAM requests and limits, maximizing hardware usage and cutting infrastructure costs.

## 2. Key Capabilities Out of the Box
|**Feature**|**What It Does**|
|---|---|
|**Self-Healing**|Automatically restarts failed containers, replaces killed nodes, and hides unhealthy instances from traffic.|
|**Service Discovery & Load Balancing**|Assigns internal DNS names and IP addresses to containers, balancing traffic across healthy instances.|
|**Automated Rollouts & Rollbacks**|Updates application code gradually with zero downtime; rolls back instantly if health checks fail.|
|**Storage Orchestration**|Dynamically mounts local storage, public cloud storage (AWS EBS, GCP PD), or network storage (NFS, Ceph).|
|**Secret & Config Management**|Separates passwords, tokens, and configuration files from container image builds.|
