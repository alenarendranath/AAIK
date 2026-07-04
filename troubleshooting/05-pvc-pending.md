# PVC Stuck in Pending

## Symptoms
```bash
kubectl get pvc
NAME        STATUS    VOLUME   CAPACITY   ACCESS MODES   STORAGECLASS   AGE
aaik-pvc    Pending                                      standard       5m
```

## Diagnose
```bash
kubectl describe pvc aaik-pvc
# Look at Events section
```

## Common Causes & Fixes

### No StorageClass / default StorageClass
```bash
kubectl get storageclass
# Fix: create a StorageClass or specify one in PVC
#   storageClassName: standard
```

### StorageClass has no provisioner (local cluster)
```bash
# In minikube: kubectl apply -f https://raw.githubusercontent.com/rancher/local-path-provisioner/master/deploy/local-path-storage.yaml
# In kind:     kind already provides local-path provisioner
```

### AccessMode not supported
```bash
# ReadWriteMany not supported by most StorageClasses (except NFS, EFS, etc.)
# Fix: use ReadWriteOnce for single-node or block storage
```

### Requested size too large
```bash
# No PV available with sufficient capacity
kubectl get pv                   # check available PVs
# Fix: reduce requested storage or create a PV manually
```
