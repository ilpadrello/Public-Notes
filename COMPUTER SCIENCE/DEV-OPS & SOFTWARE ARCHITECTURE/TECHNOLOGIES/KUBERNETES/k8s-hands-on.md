---
title: K8S hands on
---
# Hands on in kubernetes
To lean kubernetes locally you need to be `kind` (Kubenetes In Docker) !!!
`kind` is the cleanest choise because it run kubernetes control plane and worker nodes as lightweight Docker containers on my machine.

### Install `kubectl` and `kind`
```bash
sudo pacman -S kubectl kind
sudo apt install kubectl kind
```

### Create cluster named k8s-voyage
```bash
kind create cluster --name k8s-voyage
```

### Verify your cluster is up and running:
```bash
kubectl cluster-info --context kind-k8s-voyage
```

1. **Check the Nodes:**
    
    Bash
    
    ```
    kubectl get nodes
    ```
    
2. **Inspect the Control Plane Pods** (see `kube-apiserver`, `etcd`, etc. in action):
    
    Bash
    
    ```
    kubectl get pods -n kube-system
    ```
    
3. **Deploy a quick test application (Nginx):**
    
    Bash
    
    ```
    kubectl create deployment nginx-test --image=nginx:alpine
    ```
    
4. **Check the status of your Deployment & Pod:**
    
    Bash
    
    ```
    kubectl get deployments
    kubectl get pods
    ```
