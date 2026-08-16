# Fasi apprendimento

Here is how we can map out this journey, depending on your preferred pacing and goals:

- **Phase 1: The Core Mental Model** — Understanding why K8s exists, the Control Plane (API Server, `etcd`, Scheduler, Controller Manager), Worker Nodes (`kubelet`, `kube-proxy`), and the fundamental object: the **Pod**.
- **Phase 2: Hands-On Local Setup** — Getting a lightweight local cluster running (`minikube`, `kind`, or `k3s`) and mastering `kubectl` to inspect, apply, and troubleshoot resources.
- **Phase 3: Workloads & Traffic** — Deployments, ReplicaSets, StatefulSets, Services (ClusterIP, NodePort, LoadBalancer), and Ingress routing.
- **Phase 4: State, Configs & Operations** — ConfigMaps, Secrets, Persistent Volumes (PV/PVC), Helm package management, and basic cluster monitoring.
  
  
  1. **Application Configuration:** Injecting environment variables and files without rebuilding Docker images (**ConfigMaps** and **Secrets**).
    
2. **Zero-Downtime Updates & Health:** Updating code smoothly and making sure Kubernetes knows when an app is ready to receive traffic (**Rolling Updates**, **Liveness & Readiness Probes**).
    
3. **Persisting Data:** Giving Pods storage that survives restarts (**PersistentVolumeClaims**).