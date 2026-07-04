# Pod Stuck in CrashLoopBackOff

## Symptoms
```bash
NAME        READY   STATUS             RESTARTS   AGE
aaik-pod    0/1     CrashLoopBackOff   5          3m
```

## Diagnose
```bash
# Get current logs
kubectl logs aaik-pod

# Get logs from the PREVIOUS (crashed) container
kubectl logs aaik-pod --previous

# Describe for events and last state
kubectl describe pod aaik-pod
# Look at: Last State → Exit Code
```

## Common Causes & Fixes

### Exit Code 1 — Application error
```bash
kubectl logs aaik-pod --previous    # read the actual error
# Fix: fix application bug or configuration
```

### Exit Code 127 — Command not found
```bash
# Your command or entrypoint doesn't exist in the image
# Fix: check image name and command
kubectl describe pod aaik-pod | grep -A3 "Command"
```

### Exit Code 137 — OOMKilled (out of memory)
```bash
kubectl describe pod aaik-pod | grep -A5 "Last State"
# Reason: OOMKilled
# Fix: increase memory limits
#   limits:
#     memory: "256Mi"    ← increase this
```

### Exit Code 0 — Container exits immediately
```bash
# Container's main process ran to completion
# Fix: add a long-running process or use sleep infinity for debugging
command: ["/bin/sh", "-c", "nginx -g 'daemon off;'"]
```

### Missing ConfigMap / Secret
```bash
# Event: "configmap aaik-config not found"
kubectl get configmap
kubectl get secret
# Fix: apply the ConfigMap/Secret before the pod
```
