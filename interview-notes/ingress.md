# Kubernetes Ingress - Interview Questions

## Q1. What is Ingress?

Ingress is a Kubernetes API object that manages external HTTP and HTTPS access to applications running inside a Kubernetes cluster.

It routes traffic to Kubernetes Services based on hostnames or URL paths.

---

## Q2. Why do we need Ingress?

Without Ingress, every application requires its own LoadBalancer Service.

Ingress allows multiple applications to share a single external Load Balancer.

This reduces cloud cost and simplifies traffic management.

---

## Q3. What is the difference between Service and Ingress?

| Service | Ingress |
|----------|----------|
| Exposes Pods | Routes traffic to Services |
| Layer 4 (TCP/UDP) | Layer 7 (HTTP/HTTPS) |
| One Service per Application | One Ingress can route to multiple Services |

---

## Q4. Can Ingress work without an Ingress Controller?

No.

Ingress is only a Kubernetes resource.

An Ingress Controller is required to watch Ingress resources and configure the underlying Load Balancer or Proxy.

---

## Q5. What are common Ingress Controllers?

- NGINX Ingress Controller
- AWS Load Balancer Controller
- Traefik
- HAProxy

---

## Q6. What routing methods does Ingress support?

### Path-Based Routing

Example

```
example.com/api

↓

API Service
```

### Host-Based Routing

Example

```
api.company.com

↓

API Service
```

---

## Q7. What is the difference between NodePort, LoadBalancer and Ingress?

| Feature | NodePort | LoadBalancer | Ingress |
|----------|----------|--------------|----------|
| External Access | Yes | Yes | Yes |
| Multiple Applications | No | No | Yes |
| HTTP Routing | No | No | Yes |
| Production Ready | Limited | Yes | Yes |

---

## Q8. How does Ingress work in AWS?

```
User

↓

Route53

↓

AWS ALB

↓

Listener

↓

Target Group

↓

Ingress

↓

Service

↓

Pods
```

The AWS Load Balancer Controller watches the Ingress resource and automatically creates:

- ALB
- Listener
- Target Groups
- Listener Rules

---

## Q9. Does Ingress create an AWS Load Balancer?

Not directly.

The AWS Load Balancer Controller watches the Ingress resource and communicates with AWS APIs to create and manage the Load Balancer.

---

## Q10. What are common reasons for Ingress not working?

- Ingress Controller not installed
- Wrong Ingress Class
- Incorrect Service Name
- Wrong Service Port
- Backend Pods not Running
- Incorrect Path Rules

---

## Q11. Why is Ingress preferred in production?

Because one Load Balancer can expose multiple applications using host-based or path-based routing, reducing infrastructure cost and simplifying management.

---

## Q12. Explain the production request flow using Ingress.

```
User

↓

Route53

↓

AWS ALB

↓

Listener

↓

Target Group

↓

Ingress

↓

Service

↓

Pods

↓

Application
```

Ingress receives the HTTP/HTTPS request and routes it to the appropriate Kubernetes Service based on the configured rules.

---

# Quick Revision

- Ingress is a Layer 7 routing resource.
- Ingress requires an Ingress Controller.
- Supports Path-Based and Host-Based Routing.
- One Ingress can expose multiple Services.
- AWS commonly uses the AWS Load Balancer Controller.
- Ingress reduces cloud costs by sharing one Load Balancer.