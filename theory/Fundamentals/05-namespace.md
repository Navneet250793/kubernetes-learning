# Namespace

## What is a Namespace?

A Namespace is a logical partition within a Kubernetes cluster.

Namespaces help organize and isolate resources inside the same cluster.

Example:

```text
Kubernetes Cluster
│
├── default
├── kube-system
├── kube-public
└── sre
```

Resources in one namespace are logically separated from resources in another namespace.

---

# Why Do We Need Namespaces?

Without namespaces:

```text
Cluster
│
├── Pod
├── Service
├── Deployment
├── Pod
├── Service
└── Deployment
```

Everything exists together and becomes difficult to manage.

With namespaces:

```text
Cluster
│
├── Development Namespace
├── Testing Namespace
├── Production Namespace
└── SRE Namespace
```

Resources are organized and easier to manage.

---

# Benefits of Namespaces

* Resource Isolation
* Better Organization
* Multi-Team Support
* Access Control
* Resource Quotas

---

# Default Namespaces

## default

Default namespace for user workloads.

Example:

```bash
kubectl get pods
```

Resources are created in the default namespace if no namespace is specified.

---

## kube-system

Contains Kubernetes system components.

Examples:

* kube-apiserver
* etcd
* kube-scheduler
* kube-controller-manager
* CoreDNS

View:

```bash
kubectl get pods -n kube-system
```

---

## kube-public

Readable by all users.

Used for cluster-wide information.

---

## kube-node-lease

Stores node heartbeat information.

Helps Kubernetes detect node availability.

---

# Creating a Namespace

Create using command:

```bash
kubectl create namespace sre
```

Verify:

```bash
kubectl get namespaces
```

---

# Working with Namespaces

Create Deployment in namespace:

```bash
kubectl apply -f deployment.yaml -n sre
```

List Pods:

```bash
kubectl get pods -n sre
```

List Deployments:

```bash
kubectl get deploy -n sre
```

---

# Namespace Isolation

Example:

Namespace:

```text
dev
```

Pod:

```text
nginx
```

Namespace:

```text
prod
```

Pod:

```text
nginx
```

Both Pods can exist because they belong to different namespaces.

---

# Switching Namespace Context

Check current context:

```bash
kubectl config view --minify
```

Set default namespace:

```bash
kubectl config set-context --current --namespace=sre
```

Verify:

```bash
kubectl config view --minify
```

---

# Namespace YAML Example

```yaml
apiVersion: v1
kind: Namespace

metadata:
  name: sre
```

Apply:

```bash
kubectl apply -f namespace.yaml
```

---

# Production Use Cases

## Environment Separation

```text
dev
test
uat
prod
```

---

## Team Separation

```text
frontend
backend
devops
sre
```

---

## Multi-Tenant Clusters

Different teams share the same cluster while keeping resources isolated.

---

# Useful Commands

List Namespaces

```bash
kubectl get ns
```

Create Namespace

```bash
kubectl create namespace sre
```

Delete Namespace

```bash
kubectl delete namespace sre
```

View Resources in Namespace

```bash
kubectl get all -n sre
```

Set Default Namespace

```bash
kubectl config set-context --current --namespace=sre
```

---

# Key Takeaways

* Namespace is a logical partition in Kubernetes.
* Namespaces help organize and isolate resources.
* Multiple teams can share the same cluster.
* Resources with the same name can exist in different namespaces.
* Kubernetes provides default namespaces.
* Namespaces improve cluster management and security.
