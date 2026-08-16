---
title: Core Mental Model
aliases:
  - K8S-Model
---

# THE CORE MENTAL MODEL
There are a lot of names and concepts to understand when talking about k8s, and we need to know them

## The Control plane:
- This is a "layer" where sit the brain of your kubernetes cluster.
- The control plane can be divided in multiple servers, so that you always have a "brain" even when one server is down.
- The control plane is not a single monolithic program, but a team of specialized background services:
	- **`kube-apiserver`:** The front door. Every component (kubelets, CLI tools, other control plane services) talks **exclusively** to the API Server.
	- **`kube-scheduler`:** The matchmaker. When you ask to run a Pod, it looks at available Nodes and decides which Node has enough CPU/RAM to host it.
	- **`kube-controller-manager`:** The engine behind the control loops. It compares the actual state of the cluster with your desired state and triggers actions to fix differences (e.g., if a Pod dies, it orders a replacement).
	- `etcd`: the database where all the informations are store. For example, k8s stores secrets, machine status etc.
	- Othere stuff that we discuss later.
## The Pod:
The smallest atomic unit in Kubernetes. 
Not a Dockerfile/Compose, but where containers live. You can have multiple containers in one Pod, but only if tightly coupled.

-  Containers inside the **same Pod** share the **same network namespace (IP address and `localhost`)** and can share storage volumes.
- If Container A is listening on port 8080 inside a Pod, Container B in the _same_ Pod can talk to it via `localhost:8080`.
- This co-located container pattern is commonly called the **Sidecar Pattern** (e.g., a main web app container paired with a logging/metrics sidecar container).

## The Woker:
The worker is the VM or physical server that run the application that you want to deploy. This is where your web servers, databases, background queues, and microservices live and consume CPU/RAM.
In order for a worker to be able to function in a kubernetes cluster, it also run : 
- kubelet
- kube-proxy
- a container runtime like containerd
- your application containers (Pods)
## The Node:
Represents a single VM or physical server (or a Docker container in local test toolings like `kind`).
A node can be of two types: 

### Control Plane Node :
A node dedicated to the control plane job, and do NOT run your actual applications or workoads.

### Worker Node : 
This is where your application services runs inside pods ! Usually represent a single VM or physical machine.

![[Pasted image 20260808182422.png]]

## Kubelets
The `kubelets` is a low-level background deamon running directly  on the host opearting system of a specific Worker Node.

- **Its focus is local:** It talks directly to the local container runtime (`containerd`).
- **Health checks:** It monitors the local container processes on _its_ node. If the `nginx` process inside a Pod crashes, `kubelet` immediately restarts that local container.
- **Limitation:** The `kubelet` only knows about its own node. If the **entire server/VM dies** (e.g., motherboard failure, network cable unplugged), the `kubelet` dies with it and cannot help.
## Deployments (Cluster-Wide Controller)
A Deployment is a high-level **logic loop** running in the Control Plane (`kube-controller-manager`).

- **Its focus is cluster-wide:** It doesn't care about individual processes on a single server; it looks at the overall desire for your application across all nodes.
- **Resilience:** If Worker Node #1 completely catches fire and dies, the `Deployment` controller notices: _"I asked for 1 replica of Nginx, but Worker Node #1 is missing, so now I have 0."_ It immediately schedules a brand-new Pod onto **Worker Node #2**.

## Endpoints:
An **Endpoint** in Kubernetes is an automatically updated address book of the actual, live IP addresses and ports of the Pods backing a Service.

When you create a Service([[k8s-services|k8s-service]]) with a `selector` (like `app: apache-app`), Kubernetes does **not** route traffic using the selector text at runtime.

Instead, a controller inside Kubernetes continuously watches for Pods matching that selector. Whenever a matching Pod is born, dies, or changes IP, Kubernetes updates a dedicated record in `etcd` called an **`Endpoints` object**.

Think of it this way:

- **Service:** The stable front door / virtual phone number (e.g., `10.96.44.19:80`). It never changes IP.
- **Endpoints:** The actual list of backend workers currently sitting behind that door (e.g., `10.244.0.9:80`, `10.244.0.10:80`, `10.244.0.11:80`).
- **Pods:** The actual containers holding the IP addresses.

# Imperative VS Declarative :
### Declarative:
Usually kubernetes uses a declarative way: you design everything in a yaml file and kubernetes exectute (like a docker compose file).

### Imperative:
But sometimes, you need to do stuff to debug or solve problems in an Imperative way (creating new pod, exposing ports etc etc), like you would do using the docker command like directly.

