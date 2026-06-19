# ReplicaSet

## What is a ReplicaSet?

A ReplicaSet ensures that a specified number of Pod replicas are running at all times.

If a Pod fails or is deleted, the ReplicaSet automatically creates a replacement Pod.

### Example

```text
Desired Pods = 3

Current Pods = 2

ReplicaSet creates:
1 additional Pod
```

This behavior is called:

```text
Self-Healing
```

---

# Why Do We Need ReplicaSets?

A standalone Pod has limitations.

Example:

```text
Pod Deleted
     ↓
Application Down
```

ReplicaSet solves this problem by automatically recreating failed Pods.

Benefits:

* Self-Healing
* High Availability
* Pod Management
* Scaling

---

# ReplicaSet Architecture

```text
ReplicaSet
     │
     ├── Pod 1
     ├── Pod 2
     └── Pod 3
```

ReplicaSet continuously monitors Pods and ensures the desired number is maintained.

---

# How ReplicaSet Works

Example:

```text
Desired Replicas = 3
Current Replicas = 3
```

Everything is healthy.

If one Pod is deleted:

```text
Desired Replicas = 3
Current Replicas = 2
```

ReplicaSet immediately creates:

```text
1 New Pod
```

Result:

```text
Desired Replicas = 3
Current Replicas = 3
```

---

# Labels and Selectors

ReplicaSet manages Pods using Labels and Selectors.

Example Label:

```yaml
labels:
  app: nginx
```

Example Selector:

```yaml
selector:
  matchLabels:
    app: nginx
```

ReplicaSet manages only Pods that match the selector.

---

# ReplicaSet Components

## Replicas

```yaml
replicas: 3
```

Specifies how many Pods should be running.

---

## Selector

```yaml
selector:
  matchLabels:
    app: nginx
```

Used to identify which Pods belong to the ReplicaSet.

---

## Template

```yaml
template:
```

Defines the blueprint for creating new Pods.

Includes:

* Labels
* Containers
* Images
* Volumes

---

# Scaling ReplicaSets

ReplicaSets can increase or decrease the number of Pods.

### Scale Up

```text
Replicas = 3
        ↓
Replicas = 5
```

Result:

```text
2 New Pods Created
```

---

### Scale Down

```text
Replicas = 5
        ↓
Replicas = 2
```

Result:

```text
3 Pods Removed
```

---

# Self-Healing

One of the most important features of ReplicaSet.

Example:

```text
Pod Deleted
      ↓
ReplicaSet Detects Missing Pod
      ↓
Creates New Pod
```

Users experience minimal disruption.

---

# ReplicaSet vs Pod

| Pod               | ReplicaSet             |
| ----------------- | ---------------------- |
| Runs a single Pod | Manages multiple Pods  |
| No self-healing   | Self-healing available |
| Manual recovery   | Automatic recovery     |
| Not scalable      | Scalable               |

---

# ReplicaSet vs Deployment

| ReplicaSet            | Deployment               |
| --------------------- | ------------------------ |
| Manages Pods          | Manages ReplicaSets      |
| Provides self-healing | Provides self-healing    |
| Provides scaling      | Provides scaling         |
| No rolling updates    | Supports rolling updates |
| No rollback           | Supports rollback        |

In production, Deployments are usually preferred over ReplicaSets.

---

# Production Use Cases

## Web Applications

```text
ReplicaSet
   ↓
3 nginx Pods
```

Ensures application availability.

---

## Microservices

```text
ReplicaSet
   ↓
Multiple API Pods
```

Handles failures automatically.

---

## High Availability

If one Pod crashes:

```text
ReplicaSet
      ↓
Creates Replacement Pod
```

Application remains available.

---

# Useful Commands

Create ReplicaSet

```bash
kubectl apply -f replicaset.yaml
```

List ReplicaSets

```bash
kubectl get rs
```

List Pods

```bash
kubectl get pods
```

Show Labels

```bash
kubectl get pods --show-labels
```

Describe ReplicaSet

```bash
kubectl describe rs <replicaset-name>
```

Delete Pod

```bash
kubectl delete pod <pod-name>
```

Delete ReplicaSet

```bash
kubectl delete rs <replicaset-name>
```

---

# Key Takeaways

* ReplicaSet maintains a desired number of Pods.
* ReplicaSet provides self-healing.
* ReplicaSet uses labels and selectors.
* ReplicaSet supports scaling.
* ReplicaSet automatically replaces failed Pods.
* Deployments manage ReplicaSets in production environments.
* ReplicaSet improves application availability.
