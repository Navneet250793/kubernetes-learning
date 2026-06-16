# Pod Lab

## Objective

Deploy a single nginx pod.

## Create Pod

```bash
kubectl apply -f pod.yaml
```

## Verify

```bash
kubectl get pods -n sre
```

## Delete

```bash
kubectl delete -f pod.yaml
```
### Pod Created

![Pod Created](../../screenshots/01-pod-lab/01-pod-created.png)

### Pod Running

![Pod Running](../../screenshots/01-pod-lab/02-pod-running.png)

### Scheduler Selected Node

![Pod Wide](../../screenshots/01-pod-lab/03-pod-wide.png)

### Pod Events

![Pod Describe](../../screenshots/01-pod-lab/04-pod-describe.png)