# 14 — StatefulSets

## What is a StatefulSet?

A **StatefulSet** manages stateful applications where each pod needs:
- A **stable, unique network identity** (same hostname after restart)
- **Stable, persistent storage** (same PVC after restart)
- **Ordered deployment and scaling**

## StatefulSet vs Deployment

| Feature | Deployment | StatefulSet |
|---------|------------|-------------|
| Pod names | Random suffix (`pod-abc12`) | Ordinal (`pod-0`, `pod-1`) |
| Storage | Shared PVC | One PVC per pod (via `volumeClaimTemplates`) |
| Scaling | Simultaneous | Ordered (0→1→2 up, 2→1→0 down) |
| Identity | Interchangeable | Unique, stable |
| Use case | Stateless apps | Databases, queues, clustered apps |

## Stable Network Identity

Each pod gets a predictable DNS name:
```
<pod-name>.<governing-service>.<namespace>.svc.cluster.local

Example: aaik-db-0.aaik-db-headless.default.svc.cluster.local
         aaik-db-1.aaik-db-headless.default.svc.cluster.local
```

## volumeClaimTemplates

Each pod gets its own PVC automatically:
- `aaik-db-0` gets PVC `data-aaik-db-0`
- `aaik-db-1` gets PVC `data-aaik-db-1`
- PVCs survive pod deletion (data preserved)

## Pod Management Policies

| Policy | Behavior |
|--------|----------|
| `OrderedReady` (default) | Create/delete pods in order; wait for each to be ready |
| `Parallel` | Create/delete all pods simultaneously |

## Next Steps
→ [15 — DaemonSets](../15-daemonsets/)
