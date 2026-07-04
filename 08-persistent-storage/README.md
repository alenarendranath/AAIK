# 08 — Persistent Storage

## The Problem with Pod Volumes

Pod volumes (`emptyDir`, etc.) are tied to the pod's lifetime. When a pod is deleted:
- The volume is destroyed
- All data is lost

For **databases, file uploads, user data** — you need storage that outlives pods.

## The Solution: PersistentVolumes

```
Developer creates PVC          Admin creates PV (or StorageClass does it)
        │                                    │
        ▼                                    ▼
PersistentVolumeClaim  ←──binds──►  PersistentVolume
        │                                    │
        ▼                                    ▼
   Pod uses PVC                    Actual storage (disk, NFS, cloud)
```

## Key Resources

### PersistentVolume (PV)
A piece of storage in the cluster provisioned by an admin or dynamically by a StorageClass.

### PersistentVolumeClaim (PVC)
A request for storage by a user/app. Binds to a matching PV.

### StorageClass
Defines the **type** of storage and enables **dynamic provisioning** — automatically creates a PV when a PVC is created.

## Access Modes

| Mode | Short | Meaning |
|------|-------|---------|
| `ReadWriteOnce` | RWO | One node can read/write |
| `ReadOnlyMany` | ROX | Many nodes can read |
| `ReadWriteMany` | RWX | Many nodes can read/write |
| `ReadWriteOncePod` | RWOP | Only ONE pod can read/write (strictest) |

## Reclaim Policies

| Policy | Behavior when PVC is deleted |
|--------|------------------------------|
| `Retain` | PV kept, data preserved, must be manually reclaimed |
| `Delete` | PV and underlying storage automatically deleted |

## Dynamic vs Static Provisioning

| | Static | Dynamic |
|-|--------|---------|
| Who creates PV? | Admin manually | StorageClass automatically |
| How? | `kubectl apply -f pv.yaml` | PVC request triggers creation |
| Best for | On-premises, specific hardware | Cloud environments |

## Next Steps
→ [09 — Services & Networking](../09-services/)
