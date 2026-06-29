# AWS Load Balancer

## What is a Load Balancer?

A Load Balancer distributes incoming client requests across multiple application instances.

Instead of users connecting directly to a server, they connect to the Load Balancer.

The Load Balancer forwards requests only to healthy targets.

---

# Why Do We Need a Load Balancer?

Suppose we have three application servers.

```text
               Users
                 │
                 ▼
        54.10.10.10
```

Without a Load Balancer:

```text
Users
   │
   ▼
EC2 Instance
```

Problems:

* Single Point of Failure
* No High Availability
* Poor Scalability

---

With a Load Balancer:

```text
Users
      │
      ▼
AWS Load Balancer
      │
 ┌────┴────┐
 ▼         ▼
EC2-1    EC2-2
             │
             ▼
          EC2-3
```

Benefits:

* High Availability
* Load Distribution
* Fault Tolerance
* Automatic Health Checks

---

# AWS Load Balancer Types

AWS provides three Load Balancers.

## 1. Application Load Balancer (ALB)

Works at:

```text
Layer 7 (HTTP/HTTPS)
```

Supports:

* Path-based Routing
* Host-based Routing
* SSL Termination
* Web Applications
* Kubernetes Ingress

Example:

```text
example.com/api

↓

Backend API
```

```text
example.com/web

↓

Frontend
```

---

## 2. Network Load Balancer (NLB)

Works at:

```text
Layer 4 (TCP/UDP)
```

Features:

* Very High Performance
* Static IP
* Low Latency

Use Cases:

* Gaming
* Financial Applications
* High Throughput APIs

---

## 3. Classic Load Balancer (CLB)

Legacy Load Balancer.

Rarely used in new deployments.

---

# Which Load Balancer is Used with Kubernetes?

Most Kubernetes environments use:

```text
ALB
```

Reasons:

* HTTP/HTTPS Routing
* Ingress Support
* Path-based Routing
* Host-based Routing

---

# Traffic Flow

```text
User

↓

AWS ALB

↓

Listener (80/443)

↓

Target Group

↓

Worker Node

↓

NodePort Service

↓

Pods
```

---

# Components

## Listener

A Listener waits for incoming requests.

Example:

```text
HTTP :80

HTTPS :443
```

The Listener evaluates rules and forwards traffic.

---

## Listener Rules

Example:

```text
/api/*

↓

API Target Group
```

```text
/web/*

↓

Frontend Target Group
```

---

## Target Group

A Target Group contains backend servers.

Targets can be:

* EC2 Instances
* IP Addresses
* Lambda Functions

Health Checks are performed against the Target Group.

---

# Health Checks

Before forwarding traffic, AWS checks whether the target is healthy.

Example:

```text
Health Check

↓

HTTP /health

↓

200 OK

↓

Healthy
```

If the application fails:

```text
503

↓

Unhealthy

↓

Traffic Stopped
```

---

# Kubernetes Integration

NodePort Service:

```text
Browser

↓

NodePort

↓

Pods
```

Production:

```text
Browser

↓

AWS ALB

↓

Target Group

↓

NodePort

↓

Service

↓

Pods
```

---

# Why Not Use NodePort in Production?

NodePort works but has limitations.

Problems:

* Exposes Node Ports
* No SSL Termination
* No Host-based Routing
* No Path-based Routing
* Limited Security

Production environments prefer:

```text
ALB

↓

Ingress

↓

Service

↓

Pods
```

---

# Real Production Example

```text
Internet

↓

Route53

↓

AWS ALB

↓

Listener 443

↓

Target Group

↓

Ingress

↓

Service

↓

Deployment

↓

ReplicaSet

↓

Pods
```

---

# Interview Questions

### Why is ALB preferred over NodePort?

Because ALB provides:

* SSL Termination
* Path Routing
* Host Routing
* Health Checks
* High Availability

---

### Which AWS Load Balancer is commonly used with Kubernetes?

Application Load Balancer (ALB).

---

### What is the role of a Listener?

A Listener accepts client requests and evaluates rules before forwarding traffic.

---

### What is the role of a Target Group?

A Target Group contains healthy backend targets and distributes requests to them.

---

# Key Takeaways

* A Load Balancer distributes traffic across multiple targets.
* ALB is commonly used with Kubernetes.
* ALB works at Layer 7.
* NLB works at Layer 4.
* ALB forwards requests using Listeners and Target Groups.
* Health Checks ensure traffic reaches only healthy applications.
* In production, ALB works with Ingress, Services, and Pods.
