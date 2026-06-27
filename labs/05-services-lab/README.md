# Service Lab

## Objective

Learn how Kubernetes Services provide a stable network endpoint for Pods.

This lab demonstrates:

* Service Creation
* ClusterIP
* Labels & Selectors
* Endpoints
* NodePort
* Browser Access
* Load Balancing

---

## Architecture

```text
Browser
    │
    ▼
Service
    │
    ▼
Deployment
    │
    ▼
Pods
```

---

## Prerequisites

Deployment should already be running.

Verify:

```bash
kubectl get deploy -n sre
kubectl get pods -n sre
```

---

## Create ClusterIP Service

```bash
kubectl apply -f service.yaml
```

---

## Verify Service

```bash
kubectl get svc -n sre
```

---

## Verify Endpoints

```bash
kubectl get endpoints -n sre
```

---

## Describe Service

```bash
kubectl describe svc nginx-service -n sre
```

---

## Convert to NodePort

Edit:

```yaml
type: NodePort
```

Apply:

```bash
kubectl apply -f service.yaml
```

---

## Verify NodePort

```bash
kubectl get svc -n sre
```

---

## Browser Test

```text
http://<EC2-Public-IP>:<NodePort>
```

You should see the Nginx Welcome Page.

---

## Useful Commands

```bash
kubectl get svc -n sre

kubectl describe svc nginx-service -n sre

kubectl get endpoints -n sre

kubectl get pods -n sre --show-labels

kubectl edit svc nginx-service -n sre
```

---

## Expected Result

```text
Client
    │
    ▼
Service
    │
    ▼
3 Nginx Pods
```

The Service distributes traffic across all Pods.

---

## Screenshots

### 01-service-created.png

```bash
kubectl get svc -n sre
```

### 02-service-describe.png

```bash
kubectl describe svc nginx-service -n sre
```

### 03-endpoints.png

```bash
kubectl get endpoints -n sre
```

### 04-pod-labels.png

```bash
kubectl get pods -n sre --show-labels
```

### 05-nodeport.png

```bash
kubectl get svc -n sre
```

### 06-browser-access.png

Open:

```text
http://<EC2-Public-IP>:<NodePort>
```

### 07-service-details.png

```bash
kubectl get svc,ep,pods -n sre
```
