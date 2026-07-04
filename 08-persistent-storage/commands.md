# Persistent Storage — kubectl Commands

## PersistentVolumes
```bash
kubectl get pv                                    # list PVs
kubectl get pv <name> -o yaml                     # full details
kubectl describe pv <name>
kubectl delete pv <name>
```

## PersistentVolumeClaims
```bash
kubectl get pvc                                   # list PVCs
kubectl get pvc <name> -o yaml
kubectl describe pvc <name>                       # shows bound PV, events
kubectl delete pvc <name>
```

## StorageClasses
```bash
kubectl get storageclass                          # list StorageClasses
kubectl get sc                                    # shorthand
kubectl get sc <name> -o yaml
kubectl describe sc <name>
```

## CSI Drivers
```bash
kubectl get csidrivers                            # list installed CSI drivers
kubectl describe csidriver <name>
```

## VolumeSnapshots
```bash
kubectl get volumesnapshots
kubectl get volumesnapshotclasses
kubectl describe volumesnapshot <name>
```

## Useful Combos
```bash
# Check PVC is bound
kubectl get pvc -w

# Check which PV a PVC is bound to
kubectl get pvc <name> -o jsonpath='{.spec.volumeName}'

# Check storage usage inside pod
kubectl exec <pod> -- df -h /data
```
