# Kubernetes Ingress

## What is Ingress?

Ingress is a Kubernetes API object that manages external HTTP and HTTPS access to applications running inside a Kubernetes cluster.

It routes incoming requests to different Kubernetes Services based on hostnames or URL paths.

Unlike a Service, an Ingress can expose multiple applications using a single external endpoint.

---

# Why Do We Need Ingress?

Suppose a company has three applications.

- Frontend
- Backend API
- Admin Portal

Without Ingress

```text
                 Internet
                     │
     ┌───────────────┼───────────────┐
     ▼               ▼               ▼
LoadBalancer     LoadBalancer    LoadBalancer
 Service           Service         Service
     │               │               │
     ▼               ▼               ▼
Frontend Pods    API Pods      Admin Pods
```

Problems

- Three AWS Load Balancers
- Higher Infrastructure Cost
- Difficult Management
- Multiple Public Endpoints

---

With Ingress

```text
                 Internet
                     │
                     ▼
                 Ingress
          ┌────────┼────────┐
          ▼        ▼        ▼
     Frontend   API      Admin
      Service  Service   Service
          │        │        │
          ▼        ▼        ▼
       Frontend  API      Admin
         Pods    Pods      Pods
```

Benefits

- Single Entry Point
- Lower AWS Cost
- Easy Traffic Management
- Path-Based Routing
- Host-Based Routing

---

# Architecture

```text
                    Internet
                        │
                        ▼
                    Ingress
                        │
         ┌──────────────┼──────────────┐
         ▼              ▼              ▼
   nginx-service   api-service   admin-service
         │              │              │
         ▼              ▼              ▼
      nginx Pods     API Pods      Admin Pods
```

---

# How Does It Work?

Step 1

User sends an HTTP/HTTPS request.

↓

Step 2

Ingress receives the request.

↓

Step 3

Ingress checks the configured rules.

↓

Step 4

Ingress selects the matching Kubernetes Service.

↓

Step 5

The Service forwards the request to one of the Pods.

↓

Step 6

The application processes the request and returns the response.

---

# Path-Based Routing

One domain can serve multiple applications using URL paths.

Example

```text
example.com/

↓

Frontend Service
```

```text
example.com/api

↓

API Service
```

```text
example.com/admin

↓

Admin Service
```

---

# Host-Based Routing

Different domains can point to different Services.

Example

```text
api.company.com

↓

API Service
```

```text
admin.company.com

↓

Admin Service
```

```text
shop.company.com

↓

Frontend Service
```

---

# Ingress Controller

Ingress is only a Kubernetes resource.

It cannot receive or process traffic by itself.

An Ingress Controller watches Ingress resources and configures the underlying Load Balancer or Reverse Proxy.

Popular Ingress Controllers

- NGINX Ingress Controller
- AWS Load Balancer Controller
- Traefik
- HAProxy

---

# Kubernetes on AWS

Production Flow

```text
User
    │
    ▼
Route53
    │
    ▼
AWS ALB
    │
    ▼
HTTPS Listener
    │
    ▼
Target Group
    │
    ▼
Ingress
    │
    ▼
Service
    │
    ▼
Pods
```

The AWS Load Balancer Controller watches the Ingress resource and automatically creates

- Application Load Balancer (ALB)
- Listener
- Listener Rules
- Target Groups

---

# Ingress vs LoadBalancer Service

| Feature | LoadBalancer Service | Ingress |
|----------|----------------------|----------|
| External Access | Yes | Yes |
| Multiple Applications | No | Yes |
| Path-Based Routing | No | Yes |
| Host-Based Routing | No | Yes |
| Layer | Layer 4 | Layer 7 |
| Cloud Cost | Higher | Lower |
| Production Usage | Small Applications | Microservices |

---

# Real-Time Example

Suppose a company hosts three applications.

```text
shop.company.com

↓

Frontend Service
```

```text
shop.company.com/api

↓

API Service
```

```text
shop.company.com/admin

↓

Admin Service
```

A single Ingress routes requests to the appropriate Service based on the request path.

---

# Production Use Cases

- Microservices Architecture
- REST APIs
- Multiple Web Applications
- Path-Based Routing
- Host-Based Routing
- HTTPS Applications
- Centralized Traffic Management

---

# Common Production Problems

## 404 Not Found

Possible Causes

- Wrong Path
- Incorrect Host Rule
- Wrong Ingress Rule

---

## ALB Not Created

Possible Causes

- AWS Load Balancer Controller not installed
- Wrong Ingress Class
- Missing IAM Permissions

---

## Service Not Reachable

Possible Causes

- Incorrect Service Name
- Wrong Service Port
- Label Selector Mismatch

---

## Backend Unhealthy

Possible Causes

- Pods Not Running
- Health Check Failure
- Wrong Target Port

---

## HTTPS Not Working

Possible Causes

- ACM Certificate Missing
- HTTPS Listener Not Configured
- Incorrect Ingress Annotations

---

# Key Takeaways

- Ingress is a Layer 7 routing resource.
- Ingress manages external HTTP and HTTPS traffic.
- One Ingress can expose multiple Kubernetes Services.
- Supports Path-Based and Host-Based Routing.
- Requires an Ingress Controller.
- AWS commonly uses the AWS Load Balancer Controller.
- Ingress reduces cloud infrastructure cost by sharing one Load Balancer across multiple applications.