# Service

## What is a Service?

A Service is a Kubernetes object that provides a stable network endpoint for accessing one or more Pods.

Pods are ephemeral, meaning they can be created, deleted, or recreated at any time. Since Pod IP addresses change, applications cannot reliably communicate using Pod IPs.

A Service solves this problem by providing a stable IP address and DNS name.

---

# Why Do We Need a Service?

Suppose a Deployment creates three Pods.

```text
Deployment
     │
     ├── Pod 1 (10.244.0.5)
     ├── Pod 2 (10.244.0.8)
     └── Pod 3 (10.244.0.12)
```

If Pod 2 crashes:

```text
ReplicaSet
      ↓
Creates New Pod
      ↓
Pod 2 (10.244.0.15)
```

The Pod IP changes.

Applications using the old IP will fail.

A Service provides one stable endpoint.

```text
Client
   │
   ▼
Service (Stable IP)
   │
   ▼
Pods
```

Applications always connect to the Service instead of individual Pods.

---

# Service Architecture

```text
User
  │
  ▼
Service
  │
  ▼
Deployment
  │
  ▼
ReplicaSet
  │
  ▼
Pods
```

The Service automatically forwards traffic to healthy Pods.

---

# How Does a Service Work?

A Service uses Labels and Selectors.

Pods:

```yaml
labels:
  app: nginx
```

Service:

```yaml
selector:
  app: nginx
```

The Service automatically discovers Pods with matching labels.

---

# Types of Services

Kubernetes provides four Service types.

## 1. ClusterIP (Default)

ClusterIP exposes an application only inside the Kubernetes cluster.

```text
Client (Inside Cluster)
        │
        ▼
ClusterIP Service
        │
        ▼
Pods
```

Use Case:

* Internal APIs
* Database access
* Microservice communication

---

## 2. NodePort

NodePort exposes the application on a port of every Kubernetes node.

```text
Browser
    │
    ▼
NodeIP:30080
    │
    ▼
Service
    │
    ▼
Pods
```

Default NodePort range:

```text
30000–32767
```

Use Case:

* Testing
* Development
* Small environments

---

## 3. LoadBalancer

Creates an external Load Balancer through the cloud provider.

```text
Internet
    │
    ▼
Load Balancer
    │
    ▼
Service
    │
    ▼
Pods
```

Use Case:

* Production applications
* Public websites
* APIs

---

## 4. ExternalName

Maps a Kubernetes Service to an external DNS name.

Example:

```text
database.company.com
```

Useful for integrating external services.

---

# Labels and Selectors

Pods:

```yaml
labels:
  app: nginx
```

Service:

```yaml
selector:
  app: nginx
```

Only Pods with matching labels receive traffic.

---

# Load Balancing

Suppose three Pods are running.

```text
Pod A
Pod B
Pod C
```

Requests arrive:

```text
Request 1 → Pod A

Request 2 → Pod B

Request 3 → Pod C
```

The Service distributes traffic across healthy Pods.

---

# Endpoints

A Service automatically creates Endpoints.

Endpoints contain the IP addresses of Pods behind the Service.

Verify:

```bash
kubectl get endpoints
```

---

# DNS

Every Service receives a DNS name.

Example:

```text
nginx-service.sre.svc.cluster.local
```

Applications inside the cluster can use this DNS name instead of IP addresses.

---

# Service vs Pod

| Pod                   | Service                 |
| --------------------- | ----------------------- |
| Temporary IP          | Stable IP               |
| Can disappear         | Always available        |
| No load balancing     | Built-in load balancing |
| Access individual Pod | Access group of Pods    |

---

# Production Use Cases

## Internal Microservices

```text
Frontend
     │
     ▼
ClusterIP
     │
     ▼
Backend API
```

---

## Public Web Applications

```text
Internet
     │
     ▼
LoadBalancer
     │
     ▼
Nginx Pods
```

---

## Development

```text
Browser
     │
     ▼
NodePort
     │
     ▼
Application
```

---

# Useful Commands

Create Service

```bash
kubectl apply -f service.yaml
```

List Services

```bash
kubectl get svc
```

Describe Service

```bash
kubectl describe svc <service-name>
```

List Endpoints

```bash
kubectl get endpoints
```

Delete Service

```bash
kubectl delete svc <service-name>
```

---

# Key Takeaways

* A Service provides a stable network endpoint for Pods.
* Pods are ephemeral; Services provide stable connectivity.
* Services use Labels and Selectors to identify Pods.
* ClusterIP is the default Service type.
* NodePort exposes applications on node ports.
* LoadBalancer exposes applications externally.
* Services provide built-in load balancing.
* Services automatically create Endpoints.
* Applications should communicate through Services instead of Pod IPs.
