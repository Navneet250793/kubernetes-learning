# Deployment

## What is a Deployment?

A Deployment is a Kubernetes object used to manage applications running in Pods.

A Deployment manages ReplicaSets, and ReplicaSets manage Pods.

### Architecture

```text
Deployment
     ↓
ReplicaSet
     ↓
Pods
```

In production environments, Deployments are the most commonly used workload object.

---

# Why Do We Need Deployments?

ReplicaSets provide:

* Self-Healing
* Scaling

Deployments add:

* Rolling Updates
* Rollbacks
* Version Management
* Controlled Application Updates

Without Deployments:

```text
ReplicaSet
      ↓
Manual Updates
```

With Deployments:

```text
Deployment
      ↓
Automatic Updates
      ↓
Rollback Support
```

---

# Deployment Architecture

```text
Deployment
     │
     └── ReplicaSet
              │
              ├── Pod 1
              ├── Pod 2
              └── Pod 3
```

Deployment does not create Pods directly.

Deployment creates ReplicaSets, and ReplicaSets create Pods.

---

# How Deployment Works

Example:

```text
Deployment
     ↓
ReplicaSet
     ↓
3 Pods
```

If a Pod fails:

```text
ReplicaSet
      ↓
Creates New Pod
```

If the application version changes:

```text
Deployment
      ↓
Creates New ReplicaSet
      ↓
Gradually Replaces Pods
```

---

# Rolling Updates

One of the most important Deployment features.

Suppose the current image is:

```text
nginx:1.25
```

You update to:

```text
nginx:1.27
```

Deployment performs:

```text
Old Pod
    ↓
New Pod Created
    ↓
Old Pod Removed
```

Users experience little or no downtime.

---

# Rollback

If a new version causes issues:

```text
Version 1
     ↓
Version 2
     ↓
Application Failure
```

Deployment allows:

```text
Rollback
     ↓
Version 1 Restored
```

This is one of the biggest advantages of Deployments.

---

# Scaling Deployments

Deployments support scaling.

### Scale Up

```text
Replicas = 3
      ↓
Replicas = 5
```

Result:

```text
2 Additional Pods Created
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

# Deployment Strategy

Default strategy:

```text
RollingUpdate
```

Benefits:

* Minimal downtime
* Gradual replacement
* Safer upgrades

---

# Deployment vs ReplicaSet

| ReplicaSet        | Deployment          |
| ----------------- | ------------------- |
| Manages Pods      | Manages ReplicaSets |
| Self-Healing      | Self-Healing        |
| Scaling           | Scaling             |
| No Rollback       | Rollback Support    |
| No Rolling Update | Rolling Updates     |

---

# Production Use Cases

## Web Applications

```text
Deployment
     ↓
Multiple Nginx Pods
```

---

## Microservices

```text
Deployment
     ↓
API Pods
```

---

## Zero-Downtime Upgrades

```text
Old Version
      ↓
New Version
```

Users continue accessing the application during upgrades.

---

# Useful Commands

Create Deployment

```bash
kubectl apply -f deployment.yaml
```

List Deployments

```bash
kubectl get deploy
```

List ReplicaSets

```bash
kubectl get rs
```

List Pods

```bash
kubectl get pods
```

Describe Deployment

```bash
kubectl describe deployment <deployment-name>
```

Scale Deployment

```bash
kubectl scale deployment <deployment-name> --replicas=5
```

Check Rollout Status

```bash
kubectl rollout status deployment <deployment-name>
```

Rollback Deployment

```bash
kubectl rollout undo deployment <deployment-name>
```

---

# Key Takeaways

* Deployment is the most commonly used Kubernetes workload.
* Deployment manages ReplicaSets.
* ReplicaSets manage Pods.
* Deployments provide rolling updates.
* Deployments provide rollback capability.
* Deployments support scaling.
* Deployments help achieve near zero-downtime upgrades.
* Deployments are preferred over ReplicaSets in production.
