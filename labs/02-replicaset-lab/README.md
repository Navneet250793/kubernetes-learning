# ReplicaSet Lab

## Objective

Learn how ReplicaSet maintains a desired number of Pods and provides self-healing.

## ReplicaSet Manifest

File:

```text
replicaset.yaml
```

## Key Concepts

* ReplicaSet
* Labels
* Selectors
* Scaling
* Self-Healing

## Deployment

```bash
kubectl apply -f replicaset.yaml
```

## Verification Commands

```bash
kubectl get rs -n sre

kubectl get pods -n sre

kubectl get pods -n sre --show-labels

kubectl describe rs nginx-rs -n sre
```

## Expected Result

ReplicaSet creates and maintains 3 nginx Pods.

If a Pod is deleted, ReplicaSet automatically creates a replacement Pod.
