# Deployment - Interview Questions & Answers

## Q1. What is a Deployment?

A Deployment is a Kubernetes object used to manage applications running in Pods.

Deployment manages ReplicaSets, and ReplicaSets manage Pods.

---

## Q2. Why do we use Deployments?

Deployments provide:

* Self-Healing
* Scaling
* Rolling Updates
* Rollback
* Version Management

Deployments are the preferred way to run applications in Kubernetes.

---

## Q3. What is the relationship between Deployment, ReplicaSet, and Pod?

```text
Deployment
     ↓
ReplicaSet
     ↓
Pods
```

Deployment manages ReplicaSets, and ReplicaSets manage Pods.

---

## Q4. Does Deployment create Pods directly?

No.

Deployment creates ReplicaSets, and ReplicaSets create Pods.

---

## Q5. What is a Rolling Update?

Rolling Update is the process of updating application Pods gradually without downtime.

Example:

```text
Old Version
      ↓
New Version
      ↓
Pods Replaced Gradually
```

---

## Q6. What is Rollback?

Rollback restores the previous working version of an application.

Example:

```text
Version 1
     ↓
Version 2
     ↓
Issue Found
     ↓
Rollback
     ↓
Version 1 Restored
```

---

## Q7. How do you create a Deployment?

```bash
kubectl apply -f deployment.yaml
```

---

## Q8. How do you view Deployments?

```bash
kubectl get deployments
```

or

```bash
kubectl get deploy
```

---

## Q9. How do you describe a Deployment?

```bash
kubectl describe deployment <deployment-name>
```

---

## Q10. How do you scale a Deployment?

```bash
kubectl scale deployment <deployment-name> --replicas=5
```

---

## Q11. How do you check rollout status?

```bash
kubectl rollout status deployment <deployment-name>
```

---

## Q12. How do you perform a Rollback?

```bash
kubectl rollout undo deployment <deployment-name>
```

---

## Q13. How do you view rollout history?

```bash
kubectl rollout history deployment <deployment-name>
```

---

## Q14. What is the default Deployment strategy?

Default strategy:

```text
RollingUpdate
```

Benefits:

* Minimal downtime
* Gradual replacement of Pods
* Safer application upgrades

---

## Q15. Difference between ReplicaSet and Deployment?

| ReplicaSet         | Deployment          |
| ------------------ | ------------------- |
| Manages Pods       | Manages ReplicaSets |
| Self-Healing       | Self-Healing        |
| Scaling            | Scaling             |
| No Rollback        | Rollback Support    |
| No Rolling Updates | Rolling Updates     |

---

## Q16. What happens when a Pod in a Deployment is deleted?

ReplicaSet automatically creates a replacement Pod.

This is called:

```text
Self-Healing
```

---

## Q17. Can a Deployment manage multiple ReplicaSets?

Yes.

During updates, Deployment creates a new ReplicaSet and gradually replaces Pods from the old ReplicaSet.

---

## Q18. Why are Deployments preferred in Production?

Because they provide:

* Self-Healing
* Scaling
* Rolling Updates
* Rollback
* High Availability

---

## Q19. How do you list Deployments, ReplicaSets, and Pods together?

```bash
kubectl get deploy,rs,pods
```

---

## Q20. What is the most common Kubernetes workload object?

Deployment.

Most production applications are deployed using Deployments.

---

## Quick Revision

* Deployment manages ReplicaSets.
* ReplicaSet manages Pods.
* Deployment provides Rolling Updates.
* Deployment provides Rollback.
* Deployment supports Scaling.
* Deployment supports Self-Healing.
* RollingUpdate is the default strategy.
* Deployment is the most commonly used workload in Kubernetes.
