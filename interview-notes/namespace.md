# Namespace - Interview Questions & Answers

## Q1. What is a Namespace?

A Namespace is a logical partition within a Kubernetes cluster used to organize and isolate resources.

---

## Q2. Why do we need Namespaces?

Namespaces provide:

* Resource Isolation
* Better Organization
* Multi-Team Support
* Access Control
* Resource Quotas

---

## Q3. What are the default Namespaces in Kubernetes?

Kubernetes provides:

* default
* kube-system
* kube-public
* kube-node-lease

---

## Q4. What is the default Namespace?

The default namespace is where resources are created if no namespace is specified.

Example:

```bash
kubectl get pods
```

---

## Q5. What is kube-system Namespace?

The kube-system namespace contains Kubernetes system components.

Examples:

* kube-apiserver
* etcd
* kube-scheduler
* kube-controller-manager
* CoreDNS

---

## Q6. How do you list all Namespaces?

```bash
kubectl get ns
```

or

```bash
kubectl get namespaces
```

---

## Q7. How do you create a Namespace?

```bash
kubectl create namespace sre
```

---

## Q8. How do you delete a Namespace?

```bash
kubectl delete namespace sre
```

---

## Q9. How do you view resources in a Namespace?

```bash
kubectl get all -n sre
```

---

## Q10. Can two Pods have the same name in Kubernetes?

Yes.

If they belong to different namespaces.

Example:

```text
Namespace: dev
Pod: nginx

Namespace: prod
Pod: nginx
```

Both Pods can exist because they are in different namespaces.

---

## Q11. How do you create a resource in a specific Namespace?

```bash
kubectl apply -f deployment.yaml -n sre
```

or define the namespace inside the YAML:

```yaml
metadata:
  namespace: sre
```

---

## Q12. How do you list Pods in a specific Namespace?

```bash
kubectl get pods -n sre
```

---

## Q13. How do you list Deployments in a specific Namespace?

```bash
kubectl get deploy -n sre
```

---

## Q14. How do you set a default Namespace for the current context?

```bash
kubectl config set-context --current --namespace=sre
```

---

## Q15. How do you check the current Namespace?

```bash
kubectl config view --minify
```

---

## Q16. What is Namespace Isolation?

Namespace Isolation means resources in one namespace are logically separated from resources in another namespace.

Example:

```text
dev namespace
     ↓
Pods
Services
Deployments

prod namespace
     ↓
Pods
Services
Deployments
```

---

## Q17. What are common Namespace naming conventions?

Examples:

```text
dev
test
uat
prod
sre
monitoring
logging
```

---

## Q18. Can a Namespace span multiple clusters?

No.

A Namespace exists only within a single Kubernetes cluster.

---

## Q19. What is the benefit of using Namespaces in Production?

Namespaces help:

* Separate environments
* Separate teams
* Improve security
* Simplify management
* Apply resource quotas

---

## Q20. What is the difference between a Cluster and a Namespace?

Cluster:

```text
Physical/Logical Kubernetes Environment
```

Namespace:

```text
Logical Partition Inside a Cluster
```

Example:

```text
Cluster
│
├── dev Namespace
├── test Namespace
├── prod Namespace
└── sre Namespace
```

---

## Quick Revision

* Namespace is a logical partition inside a cluster.
* Namespaces provide isolation and organization.
* Default namespaces include default, kube-system, kube-public, and kube-node-lease.
* Resources can be separated by team or environment.
* Multiple resources with the same name can exist in different namespaces.
* Namespaces improve cluster management and security.
* Namespaces are widely used in production environments.
