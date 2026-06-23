# Deployment Lab

## Objective

Learn how Deployments manage ReplicaSets and Pods.

This lab demonstrates:

* Deployment Creation
* ReplicaSet Creation
* Pod Creation
* Scaling
* Rolling Updates
* Rollback
* Self-Healing

---

## Deployment Architecture

```text
Deployment
     ↓
ReplicaSet
     ↓
Pods
```

Deployment manages ReplicaSets and ReplicaSets manage Pods.

---

## Deployment Manifest

File:

```text
deployment.yaml
```

---

## Create Deployment

```bash
kubectl apply -f deployment.yaml
```

---

## Verification Commands

Check Deployment:

```bash
kubectl get deploy -n sre
```

Check ReplicaSet:

```bash
kubectl get rs -n sre
```

Check Pods:

```bash
kubectl get pods -n sre
```

---

## Scaling Deployment

Scale to 5 replicas:

```bash
kubectl scale deployment nginx-deployment --replicas=5 -n sre
```

Verify:

```bash
kubectl get pods -n sre
```

---

## Rolling Update

Update image version:

```yaml
image: nginx:1.27
```

Apply:

```bash
kubectl apply -f deployment.yaml
```

Check rollout status:

```bash
kubectl rollout status deployment nginx-deployment -n sre
```

---

## Rollback

Rollback to previous version:

```bash
kubectl rollout undo deployment nginx-deployment -n sre
```

---

## Useful Commands

```bash
kubectl get deploy -n sre

kubectl get rs -n sre

kubectl get pods -n sre

kubectl describe deployment nginx-deployment -n sre

kubectl rollout history deployment nginx-deployment -n sre

kubectl rollout status deployment nginx-deployment -n sre

kubectl rollout undo deployment nginx-deployment -n sre
```

---

## Expected Result

Deployment creates:

```text
1 Deployment
     ↓
1 ReplicaSet
     ↓
3 Pods
```

Deployment provides:

* Self-Healing
* Scaling
* Rolling Updates
* Rollback

---

## Screenshots

### 01-deployment-created.png

```bash
kubectl get deploy -n sre
```

### 02-deployment-rs.png

```bash
kubectl get rs -n sre
```

### 03-deployment-pods.png

```bash
kubectl get pods -n sre
```

### 04-scale-up.png

```bash
kubectl scale deployment nginx-deployment --replicas=5 -n sre
```

### 05-rolling-update.png

```bash
kubectl rollout status deployment nginx-deployment -n sre
```

### 06-rollout-history.png

```bash
kubectl rollout history deployment nginx-deployment -n sre
```

### 07-rollback.png

```bash
kubectl rollout undo deployment nginx-deployment -n sre
```
