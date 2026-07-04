# Troubleshooting Guide

Common Kubernetes failures, how to diagnose them, and how to fix them.

## Quick Diagnostic Commands

```bash
kubectl get events --sort-by=.metadata.creationTimestamp
kubectl get events -n <namespace> --field-selector type=Warning
kubectl describe pod <pod>          # always start here
kubectl logs <pod>                  # app logs
kubectl logs <pod> --previous       # logs from crashed container
```

## Failure Scenarios

| Scenario | File |
|----------|------|
| Pod stuck in Pending | [01-pod-pending.md](./01-pod-pending.md) |
| Pod stuck in CrashLoopBackOff | [02-crashloopbackoff.md](./02-crashloopbackoff.md) |
| Pod stuck in ImagePullBackOff | [03-imagepullbackoff.md](./03-imagepullbackoff.md) |
| Service not reaching pods | [04-service-not-reaching-pods.md](./04-service-not-reaching-pods.md) |
| PVC stuck in Pending | [05-pvc-pending.md](./05-pvc-pending.md) |
| OOMKilled | [06-oomkilled.md](./06-oomkilled.md) |
