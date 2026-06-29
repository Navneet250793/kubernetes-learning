# AWS Target Groups

## What is a Target Group?

A Target Group is a collection of backend resources that receive traffic from an AWS Load Balancer.

The Load Balancer never sends requests directly to EC2 instances or Pods.

Instead, it forwards requests to a Target Group.

---

# Why Do We Need Target Groups?

Suppose we have an Application Load Balancer.

```text
Users
   │
   ▼
Application Load Balancer
```

Without Target Groups:

```text
Users
   │
   ▼
Load Balancer
   │
   ▼
???
```

The Load Balancer would not know where to send traffic.

Target Groups solve this problem.

---

# Architecture

```text
Users
   │
   ▼
AWS ALB
   │
   ▼
Target Group
   │
   ▼
EC2 Instances
```

---

# Components

## Load Balancer

Receives requests from users.

---

## Listener

Accepts traffic on:

```text
80

443
```

Listener evaluates rules.

---

## Target Group

Contains backend targets.

The Listener forwards traffic to a Target Group.

---

## Targets

Targets can be:

* EC2 Instances
* IP Addresses
* Lambda Functions

---

# Health Checks

Before forwarding traffic, AWS verifies that targets are healthy.

Example:

```text
ALB

↓

Health Check

↓

HTTP GET /health

↓

200 OK

↓

Healthy
```

If a target fails:

```text
503

↓

Unhealthy

↓

Traffic Stopped
```

AWS automatically removes unhealthy targets from rotation.

---

# Healthy vs Unhealthy Targets

Healthy:

```text
EC2-1

✓ Healthy
```

Receives traffic.

Unhealthy:

```text
EC2-2

✗ Unhealthy
```

Receives no traffic.

---

# Why Multiple Target Groups?

One ALB can serve multiple applications.

Example:

```text
Internet
      │
      ▼
Application Load Balancer
      │
 ┌────┴─────────────┐
 ▼                  ▼
TG-Frontend      TG-Backend
 │                  │
 ▼                  ▼
Frontend EC2     Backend EC2
```

Each application has its own Target Group.

---

# Path-Based Routing

Example:

```text
example.com/

↓

Frontend Target Group
```

```text
example.com/api

↓

API Target Group
```

Listener Rules determine which Target Group receives traffic.

---

# Host-Based Routing

Example:

```text
app.company.com

↓

Application Target Group
```

```text
admin.company.com

↓

Admin Target Group
```

---

# Kubernetes Integration

NodePort Lab:

```text
Browser

↓

NodePort

↓

Pods
```

Production Kubernetes:

```text
Browser

↓

AWS ALB

↓

Target Group

↓

NodePort Service

↓

Pods
```

The Target Group forwards requests to the Kubernetes worker nodes.

---

# Target Types

## Instance

Targets are EC2 instances.

Traffic Flow:

```text
ALB

↓

Worker Node

↓

NodePort

↓

Service

↓

Pods
```

---

## IP

Targets are Pod IPs.

Traffic Flow:

```text
ALB

↓

Pod IP

↓

Application
```

This is commonly used with the AWS Load Balancer Controller in Kubernetes.

---

# Common Health Check Settings

Protocol:

```text
HTTP
```

Path:

```text
/health
```

Port:

```text
Traffic Port
```

Healthy Threshold:

```text
5
```

Unhealthy Threshold:

```text
2
```

Timeout:

```text
5 seconds
```

Interval:

```text
30 seconds
```

---

# Common Production Issues

## Unhealthy Targets

Possible causes:

* Application not running
* Wrong Health Check path
* Security Group blocked
* NodePort unavailable
* Pod not Ready

---

## HTTP 502 Bad Gateway

Possible causes:

* Backend application crashed
* Incorrect Target Group port
* Pod terminated
* Service selector incorrect

---

## HTTP 503 Service Unavailable

Possible causes:

* No healthy targets
* Deployment has zero Pods
* Health Check failing

---

# SRE Responsibilities

Monitor:

* Healthy Target Count
* Unhealthy Target Count
* Response Time
* HTTP 4xx
* HTTP 5xx
* Target Registration
* Health Check Failures

---

# Interview Questions

## What is a Target Group?

A Target Group is a collection of backend targets that receive traffic from a Load Balancer.

---

## Can one ALB have multiple Target Groups?

Yes.

Different Listener Rules can forward requests to different Target Groups.

---

## Why are Health Checks important?

Health Checks ensure traffic is sent only to healthy backend applications.

---

## What are the Target Types?

* Instance
* IP
* Lambda

---

## Why are Target Groups important in Kubernetes?

Target Groups route traffic from the ALB to Kubernetes Services or Pods, depending on the deployment architecture.

---

# Key Takeaways

* A Target Group is the backend destination for an ALB.
* ALB sends traffic to Target Groups, not directly to applications.
* Health Checks determine whether targets receive traffic.
* One ALB can use multiple Target Groups.
* Kubernetes commonly integrates Target Groups with Services or Pod IPs.
* Monitoring Target Group health is a key SRE responsibility.
