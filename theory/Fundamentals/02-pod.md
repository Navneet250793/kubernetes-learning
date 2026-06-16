What is a Pod?

A Pod is the smallest deployable unit in Kubernetes.

Kubernetes does not deploy containers directly. Instead, containers run inside Pods.

A Pod can contain:

Single container
Multiple containers

Containers inside the same Pod:

Share network namespace
Share IP address
Share storage volumes
Communicate using localhost
Example
Pod
 └── nginx container
Why Pods?

Pods provide:

Networking
Storage sharing
Lifecycle management
Container grouping
Pod Lifecycle
Pending
   ↓
Running
   ↓
Succeeded
   ↓
Failed
Pending

Pod created but waiting for:

Scheduling
Image download
Resource allocation
Running

Container started successfully.

Succeeded

Work completed successfully.

Failed

Container terminated with error.

Multi-Container Pods

A Pod can run multiple containers.

Example:

Pod
 ├── Application Container
 └── Logging Container

Use cases:

Log collection
Monitoring
Service mesh sidecars
Init Containers

Init Containers run before application containers.

Flow:

Init Container
      ↓
Success
      ↓
Application Container

Common uses:

Database checks
Dependency validation
Configuration setup
Static Pods

Static Pods are managed directly by kubelet.

Manifest location:

/etc/kubernetes/manifests

Common examples:

kube-apiserver
etcd
kube-scheduler
controller-manager
Sidecar Pattern

A helper container running alongside the main application.

Example:

Application
      +
Fluent Bit

Benefits:

Logging
Monitoring
Security
Service mesh
Important Commands
kubectl get pods

kubectl describe pod <pod-name>

kubectl logs <pod-name>

kubectl exec -it <pod-name> -- /bin/bash

kubectl delete pod <pod-name>