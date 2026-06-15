# Kubernetes Architecture

## What is Kubernetes?

Kubernetes (K8s) is an open-source container orchestration platform used to automate the deployment, scaling, management, and monitoring of containerized applications.

### Why Kubernetes?

Without Kubernetes:

* Containers must be managed manually.
* Scaling is difficult.
* Recovery from failures is manual.
* High availability is hard to achieve.

With Kubernetes:

* Automatic deployment
* Auto scaling
* Self healing
* Load balancing
* Rolling updates
* High availability

---

# Kubernetes Architecture

```text
                    Control Plane
+--------------------------------------------------+
| API Server                                       |
| ETCD                                             |
| Controller Manager                               |
| Scheduler                                        |
+--------------------------------------------------+
                    |
                    |
                    ▼
+--------------------------------------------------+
|                  Worker Node                     |
|                                                  |
|  kubelet                                         |
|  kube-proxy                                      |
|  containerd                                      |
|                                                  |
|  Pod                                             |
|  Pod                                             |
+--------------------------------------------------+
```

---

# Control Plane Components

The Control Plane is the brain of Kubernetes.

It manages the entire cluster and makes decisions.

---

## API Server

The API Server is the main entry point of Kubernetes.

Every request goes through the API Server.

### Responsibilities

* Authentication
* Authorization
* Validation
* Admission Control
* Communication with ETCD

### Example

When we run:

```bash
kubectl get pods
```

or

```bash
kubectl create deployment nginx --image=nginx
```

The request goes to:

```text
kubectl
   ↓
API Server
```

### Interview Question

**Q. Can Kubernetes components directly communicate with ETCD?**

**Answer:**

No.

All communication must go through the API Server.

---

## ETCD

ETCD is the distributed key-value database of Kubernetes.

It stores the entire cluster state.

### Stores Information About

* Deployments
* ReplicaSets
* Pods
* Services
* Secrets
* ConfigMaps
* Nodes
* Cluster Configuration

### Important

ETCD is the source of truth for the cluster.

Without ETCD:

```text
❌ Cluster state is lost
```

### Interview Question

**Q. What is the most critical component of Kubernetes?**

**Answer:**

ETCD because it stores the complete cluster state.

---

## Controller Manager

Controller Manager continuously watches the cluster.

Its job is to ensure:

```text
Desired State = Current State
```

### Example

Desired Replicas:

```text
5
```

Current Replicas:

```text
4
```

Controller Manager automatically creates:

```text
1 additional Pod
```

This is called:

```text
Self Healing
```

### Controllers

* Deployment Controller
* ReplicaSet Controller
* Node Controller
* Job Controller
* Endpoint Controller

---

## Scheduler

Scheduler decides:

```text
Which worker node should run a Pod?
```

### Scheduler Checks

* CPU availability
* Memory availability
* Taints and Tolerations
* Node Affinity
* Resource requests
* Node conditions

### Example

```text
Pod Created
      ↓
Scheduler
      ↓
worker-node-1 selected
```

Scheduler updates Pod information through API Server.

---

# Worker Node Components

Worker Nodes run actual application workloads.

---

## kubelet

kubelet is the agent running on every worker node.

It continuously watches the API Server.

### Responsibilities

* Receives Pod assignments
* Creates Pods
* Monitors Pods
* Reports status back to API Server

### Example

```text
Scheduler assigns Pod
         ↓
kubelet detects assignment
         ↓
Creates Pod
```

---

## kube-proxy

kube-proxy handles networking on worker nodes.

### Responsibilities

* Service networking
* Pod communication
* Load balancing
* Maintaining networking rules

### Example

```text
Service
   ↓
kube-proxy
   ↓
Pod1
Pod2
Pod3
```

Traffic gets distributed across Pods.

---

## containerd

containerd is the container runtime.

It is responsible for:

* Pulling container images
* Creating containers
* Starting containers
* Stopping containers

### Example

```text
kubelet
   ↓
containerd
   ↓
Pull nginx image
   ↓
Start container
```

### Important

kubelet does NOT run containers directly.

It uses containerd.

---

# Complete Kubernetes Scheduling Flow

Suppose we run:

```bash
kubectl create deployment nginx --image=nginx
```

---

## Step 1

```text
kubectl
   ↓
API Server
```

kubectl sends request to API Server.

---

## Step 2

```text
API Server
   ↓
ETCD
```

Deployment object stored in ETCD.

At this stage:

```text
❌ Pod not running
```

Only desired state exists.

---

## Step 3

Controller Manager notices:

```text
Deployment exists
ReplicaSet missing
```

Creates ReplicaSet.

---

## Step 4

ReplicaSet notices:

```text
Desired Replicas = 1
Current Replicas = 0
```

Creates Pod object.

Again stored in ETCD.

At this stage:

```text
❌ Pod not running
```

---

## Step 5

Scheduler detects:

```text
Pod exists
Node not assigned
```

Selects:

```text
worker-node-1
```

Updates Pod information.

---

## Step 6

kubelet on worker node detects:

```text
Pod assigned to me
```

Starts Pod creation workflow.

---

## Step 7

kubelet asks:

```text
containerd
```

to:

```text
Pull image
Create container
Start container
```

---

## Step 8

containerd pulls:

```text
docker.io/library/nginx
```

Starts nginx container.

---

## Step 9

kubelet updates API Server:

```text
Pod Status = Running
```

API Server stores status in ETCD.

Now:

```bash
kubectl get pods
```

Output:

```text
NAME            READY   STATUS
nginx-xxxxx     1/1     Running
```

---

# Complete Production Flow Diagram

```text
kubectl
  ↓
API Server
  ↓
etcd stores Deployment
  ↓
Deployment Controller
  ↓
ReplicaSet created
  ↓
Pod created
  ↓
Scheduler selects node
  ↓
Pod assigned to worker node
  ↓
kubelet detects assignment
  ↓
containerd pulls image
  ↓
Container starts
  ↓
Pod Running
```

---

# Most Important Kubernetes Concept

Kubernetes continuously tries to match:

```text
Desired State
      ==
Current State
```

Controllers keep watching the cluster and automatically correct deviations.

This is the foundation of:

* Self Healing
* Auto Recovery
* High Availability

---

# Interview Answer

**Q. What happens internally when you run a Deployment?**

**Answer:**

kubectl sends a request to the API Server. The API Server validates the request and stores the desired state in ETCD. The Deployment Controller creates a ReplicaSet, and the ReplicaSet creates Pod objects. The Scheduler assigns Pods to worker nodes. kubelet on the selected node receives the assignment and asks containerd to pull the image and start the container. Once the container starts successfully, kubelet updates the Pod status to Running through the API Server.
