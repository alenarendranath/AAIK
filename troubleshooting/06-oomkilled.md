# OOMKilled — Out of Memory

## Symptoms
```bash
kubectl describe pod aaik-pod
# Last State:
#   Reason: OOMKilled
#   Exit Code: 137
```

## Diagnose
```bash
# Check current memory usage
kubectl top pods
kubectl top pods --containers      # per-container breakdown

# Check memory limits set
kubectl describe pod aaik-pod | grep -A10 Limits

# Check node memory
kubectl top nodes
kubectl describe node <node> | grep -A10 "Allocated resources"
```

## Fixes

### 1. Increase memory limit
```yaml
resources:
  requests:
    memory: "128Mi"
  limits:
    memory: "512Mi"    ← increase this
```

### 2. Fix memory leak in application
```bash
# If memory grows over time → likely a leak
# Profile your app, check for unclosed connections or unbounded caches
```

### 3. Use a VPA (Vertical Pod Autoscaler) for automatic right-sizing
```bash
# VPA recommends optimal resource requests based on actual usage
kubectl get vpa
```

### 4. Set JVM heap limits (Java apps)
```bash
# JVM doesn't respect container limits by default in older versions
env:
- name: JAVA_OPTS
  value: "-Xmx256m -Xms128m"    # set explicit heap limits
```
