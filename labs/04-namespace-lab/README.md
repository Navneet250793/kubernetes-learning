# Namespace Lab

## Objective

Learn how Kubernetes Namespaces help organize and isolate resources inside a cluster.

This lab demonstrates:

* Namespace Creation
* Namespace Verification
* Resource Isolation
* Namespace Context Switching

---

## Namespace Manifest

File:

```text
namespace.yaml
```

---

## Create Namespace

```bash
kubectl apply -f namespace.yaml
```

---

## Verify Namespace

```bash
kubectl get ns
```

---

## View Namespace Details

```bash
kubectl describe namespace dev
```

---

## Create Pod in Namespace

```bash
kubectl run nginx --image=nginx -n dev
```

---

## Verify Pod

```bash
kubectl get pods -n dev
```

---

## Compare Namespaces

View Pods in default namespace:

```bash
kubectl get pods
```

View Pods in dev namespace:

```bash
kubectl get pods -n dev
```

---

## Switch Default Namespace

```bash
kubectl config set-context --current --namespace=dev
```

Verify:

```bash
kubectl config view --minify
```

---

## Useful Commands

```bash
kubectl get ns

kubectl describe namespace dev

kubectl get pods -n dev

kubectl get all -n dev

kubectl config set-context --current --namespace=dev

kubectl config view --minify
```

---

## Expected Result

Namespaces provide:

* Isolation
* Organization
* Multi-Team Support

Example:

```text
Cluster
│
├── default
├── kube-system
├── sre
└── dev
```

---

## Screenshots

### 01-namespace-created.png

```bash
kubectl get ns
```

### 02-namespace-describe.png

```bash
kubectl describe namespace dev
```

### 03-pod-in-dev.png

```bash
kubectl get pods -n dev
```

### 04-namespace-isolation.png

```bash
kubectl get pods

kubectl get pods -n dev
```

### 05-context-switch.png

```bash
kubectl config view --minify
```