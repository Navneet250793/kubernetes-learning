# Kubernetes Architecture - Interview Questions & Answers

## Q1. What is Kubernetes?

Kubernetes is an open-source container orchestration platform used to deploy, manage, scale, and monitor containerized applications.

---

## Q2. What are the main Control Plane components?

The Control Plane consists of:

* API Server
* ETCD
* Controller Manager
* Scheduler

---

## Q3. What is API Server?

API Server is the main entry point of Kubernetes.

Responsibilities:

* Authentication
* Authorization
* Request validation
* Communication with ETCD

All cluster communication goes through the API Server.

---

## Q4. What is ETCD?

ETCD is a distributed key-value database used by Kubernetes.

It stores:

* Pods
* Deployments
* Services
* Secrets
* ConfigMaps
* Nodes
* Cluster state

ETCD is the source of truth for the cluster.

---

## Q5. What is Controller Manager?

Controller Manager continuously watches the cluster and ensures:

```text
Desired State = Current State
```

Examples:

* Deployment Controller
* ReplicaSet Controller
* Node Controller
* Job Controller

---

## Q6. What is Scheduler?

Scheduler selects the best worker node for a Pod.

Scheduler checks:

* CPU availability
* Memory availability
* Taints and Tolerations
* Affinity rules
* Resource availability

---

## Q7. What is kubelet?

kubelet is the node agent running on every worker node.

Responsibilities:

* Watches API Server
* Creates Pods
* Monitors Pods
* Reports Pod status

---

## Q8. What is kube-proxy?

kube-proxy is responsible for networking inside the cluster.

Responsibilities:

* Service networking
* Load balancing
* Maintaining network rules
* Pod-to-Pod communication

---

## Q9. What is containerd?

containerd is the container runtime.

Responsibilities:

* Pull images
* Create containers
* Start containers
* Stop containers

kubelet communicates with containerd to run containers.

---

## Q10. Explain Kubernetes Scheduling Flow.

```text
kubectl
  ↓
API Server
  ↓
ETCD
  ↓
Controller Manager
  ↓
ReplicaSet
  ↓
Pod
  ↓
Scheduler
  ↓
Worker Node
  ↓
kubelet
  ↓
containerd
  ↓
Running Pod
```

---

## Q11. What is Desired State?

Desired State is the state defined by the user.

Example:

```text
Replicas = 3
```

Kubernetes continuously tries to maintain this state.

---

## Q12. What is Self-Healing?

If current state differs from desired state, Kubernetes automatically restores the desired state.

Example:

```text
Desired Pods = 3
Current Pods = 2
```

ReplicaSet automatically creates:

```text
1 additional Pod
```

---

## Q13. Can Kubernetes components communicate directly with ETCD?

No.

All communication with ETCD must go through the API Server.

---

## Q14. Which component decides where a Pod will run?

Scheduler.

The Scheduler selects the most suitable worker node based on available resources and scheduling rules.

---

## Q15. What is the most critical component in Kubernetes?

ETCD.

Because it stores the complete cluster state and acts as the source of truth.

---

## Quick Revision

* API Server = Entry point of Kubernetes.
* ETCD = Cluster database and source of truth.
* Controller Manager = Maintains desired state.
* Scheduler = Selects worker nodes.
* kubelet = Node agent.
* kube-proxy = Networking component.
* containerd = Container runtime.
* Kubernetes works on Desired State vs Current State.
* Self-Healing automatically restores failed workloads.
* All cluster communication goes through the API Server.
