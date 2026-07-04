# Pod Stuck in Pending

## Symptoms
```bash
kubectl get pods
NAME        READY   STATUS    RESTARTS   AGE
aaik-pod    0/1     Pending   0          5m
```

## Diagnose
```bash
kubectl describe pod aaik-pod
# Look at: Events section at the bottom
```

## Common Causes & Fixes

### 1. Insufficient resources (most common)
**Event:** `0/3 nodes are available: 3 Insufficient cpu.`
```bash
# Check node capacity
kubectl describe nodes | grep -A5 "Allocated resources"
kubectl top nodes

# Fix: reduce resource requests or add nodes
# In your manifest, lower requests:
#   resources:
#     requests:
#       cpu: "50m"    ← lower this
```

### 2. No nodes match nodeSelector / affinity
**Event:** `0/3 nodes are available: 3 node(s) didn't match node selector.`
```bash
# Check what labels nodes have
kubectl get nodes --show-labels

# Fix: add the label to a node or remove the selector
kubectl label node <node-name> gpu=cuda
```

### 3. PVC not bound
**Event:** `persistentvolumeclaim "aaik-pvc" not found` or PVC is Pending
```bash
kubectl get pvc
kubectl describe pvc aaik-pvc
# Fix: ensure StorageClass exists and has a provisioner
kubectl get storageclass
```

### 4. Taints blocking scheduling
**Event:** `0/3 nodes are available: 3 node(s) had untolerated taint.`
```bash
kubectl describe nodes | grep Taints
# Fix: add toleration to pod spec, or remove taint
kubectl taint node <node-name> key=value:NoSchedule-    # remove taint
```
