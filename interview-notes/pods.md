# Pods - Interview Questions & Answers

## Q1. What is a Pod?

A Pod is the smallest deployable unit in Kubernetes that contains one or more containers.

---

## Q2. Does Kubernetes deploy containers directly?

No.

Kubernetes deploys Pods, and Pods contain containers.

---

## Q3. Can a Pod contain multiple containers?

Yes.

Containers inside a Pod share:

* Network
* Storage
* IP Address
* Lifecycle

---

## Q4. Difference between a Pod and a Container?

| Container                                 | Pod                                    |
| ----------------------------------------- | -------------------------------------- |
| Runs an application                       | Wraps one or more containers           |
| Has its own filesystem                    | Provides networking and storage        |
| Cannot be deployed directly in Kubernetes | Smallest deployable unit in Kubernetes |

---

## Q5. What is Pod Lifecycle?

A Pod can be in the following phases:

```text
Pending
Running
Succeeded
Failed
Unknown
```

---

## Q6. What is a Static Pod?

A Pod managed directly by kubelet instead of the API Server.

Manifest location:

```text
/etc/kubernetes/manifests
```

---

## Q7. What is an Init Container?

A container that runs before the main application container starts.

Common uses:

* Database checks
* Dependency validation
* Configuration setup

---

## Q8. What is a Sidecar Container?

A helper container running alongside the main application container in the same Pod.

Examples:

* Fluent Bit
* Log collectors
* Monitoring agents

---

## Q9. How do containers communicate inside the same Pod?

Using:

```text
localhost
```

because they share the same network namespace.

---

## Q10. What happens when a Pod dies?

If managed by:

```text
Deployment
ReplicaSet
```

Kubernetes automatically creates a replacement Pod.

This feature is called:

```text
Self-Healing
```

---

## Q11. Can a Pod have multiple IP addresses?

No.

A Pod gets a single IP address that is shared by all containers inside the Pod.

---

## Q12. Why are Pods used instead of deploying containers directly?

Pods provide:

* Shared networking
* Shared storage
* Lifecycle management
* Container grouping

---

## Quick Revision

* Pod = Smallest deployable unit.
* Kubernetes deploys Pods, not containers.
* Containers inside a Pod share IP and storage.
* Static Pods are managed by kubelet.
* Init Containers run before application containers.
* Sidecars provide supporting functionality.
* ReplicaSet/Deployment provides self-healing.
* Containers inside a Pod communicate using localhost.
