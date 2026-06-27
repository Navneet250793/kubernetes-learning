# Service - Interview Questions & Answers

## Q1. What is a Service?

A Service is a Kubernetes object that provides a stable network endpoint for accessing one or more Pods.

---

## Q2. Why do we need a Service?

Pods are ephemeral.

Their IP addresses change whenever they are recreated.

A Service provides:

* Stable IP
* Stable DNS
* Load Balancing

---

## Q3. How does a Service find Pods?

A Service uses Labels and Selectors.

Example:

Pod:

```yaml
labels:
  app: nginx
```

Service:

```yaml
selector:
  app: nginx
```

Only matching Pods receive traffic.

---

## Q4. What are the types of Services?

Kubernetes supports four Service types:

* ClusterIP
* NodePort
* LoadBalancer
* ExternalName

---

## Q5. What is ClusterIP?

ClusterIP is the default Service type.

It exposes an application only inside the Kubernetes cluster.

Use Cases:

* Internal APIs
* Backend Services
* Database Communication

---

## Q6. What is NodePort?

NodePort exposes an application on a port of every Kubernetes node.

Example:

```text
http://NodeIP:30080
```

Default NodePort range:

```text
30000–32767
```

---

## Q7. What is LoadBalancer?

LoadBalancer creates an external cloud load balancer.

Use Cases:

* Production Applications
* Public Websites
* APIs

---

## Q8. What is ExternalName?

ExternalName maps a Kubernetes Service to an external DNS name.

Example:

```text
database.company.com
```

---

## Q9. What is the default Service type?

ClusterIP.

---

## Q10. What are Endpoints?

Endpoints are the list of Pod IP addresses behind a Service.

Verify:

```bash
kubectl get endpoints
```

---

## Q11. How do you create a Service?

```bash
kubectl apply -f service.yaml
```

---

## Q12. How do you list Services?

```bash
kubectl get svc
```

or

```bash
kubectl get services
```

---

## Q13. How do you describe a Service?

```bash
kubectl describe svc <service-name>
```

---

## Q14. How do you verify that a Service is connected to Pods?

```bash
kubectl get endpoints
```

If Endpoints are empty, the selector is probably incorrect.

---

## Q15. What happens if a Pod dies?

ReplicaSet creates a new Pod.

The Service automatically routes traffic to the new Pod because it matches the selector.

No manual update is required.

---

## Q16. Does a Service create Pods?

No.

Deployment creates ReplicaSets.

ReplicaSets create Pods.

Service only exposes Pods over the network.

---

## Q17. Does a Service perform Load Balancing?

Yes.

Traffic is distributed across healthy Pods selected by the Service.

---

## Q18. Can multiple Pods be behind one Service?

Yes.

One Service can route traffic to many Pods.

---

## Q19. What is the relationship between Deployment and Service?

```text
Deployment
     ↓
ReplicaSet
     ↓
Pods
     ↑
Service
```

Deployment manages Pods.

Service provides network access to Pods.

---

## Q20. Why is a Service preferred over Pod IPs?

Because Pod IPs are temporary.

A Service provides:

* Stable IP
* Stable DNS
* Automatic Load Balancing
* Automatic Pod Discovery

---

# Quick Revision

* Service provides a stable network endpoint.
* Pods are ephemeral; Services are stable.
* Services use Labels and Selectors.
* ClusterIP is the default Service type.
* NodePort exposes applications on node ports.
* LoadBalancer exposes applications externally.
* Services automatically create Endpoints.
* Services provide built-in load balancing.
* Services do not create Pods; they expose Pods.
