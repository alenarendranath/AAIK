# 07 — Volumes

## Why Volumes?

Containers have an ephemeral filesystem — when a container restarts, all data is lost. Volumes solve two problems:
1. **Persist data** across container restarts within a pod
2. **Share data** between containers in the same pod

> Note: Pod-level volumes are tied to the pod's lifetime. For data that survives pod deletion, use PersistentVolumes (topic 08).

## Volume Types

| Type | Lifecycle | Use Case |
|------|-----------|----------|
| `emptyDir` | Pod lifetime | Temp storage, sharing between containers |
| `hostPath` | Node lifetime | Access node filesystem (log collection, agents) |
| `configMap` | Pod lifetime | Mount config files from ConfigMap |
| `secret` | Pod lifetime | Mount sensitive files from Secret |
| `downwardAPI` | Pod lifetime | Expose pod metadata as files |
| `projected` | Pod lifetime | Combine multiple sources into one mount |
| `persistentVolumeClaim` | PVC lifetime | Durable storage (databases, file storage) |

## emptyDir

- Created when pod is assigned to a node
- Deleted when the pod is removed
- Shared between all containers in the pod
- `medium: Memory` uses RAM (tmpfs) — faster but counts against memory limit

## hostPath

- Mounts a directory from the **node's filesystem**
- Data persists even if the pod is deleted (lives on the node)
- ⚠️ Breaks pod portability — pod must always run on same node
- Use only for node-level agents (log collectors, monitoring)

## configMap Volume

- Mounts ConfigMap entries as **files** in the container
- Updates to the ConfigMap propagate automatically (~1 min delay)
- Uses atomic symlink swap for safe updates

## secret Volume

- Like configMap but mounted as **tmpfs** (in-memory)
- More secure than env vars (not logged by default)
- `defaultMode: 0400` — read-only by owner

## projected Volume

Combines **multiple sources** into a **single mountPath**:
- ConfigMap
- Secret
- Downward API
- ServiceAccount Token

## Next Steps
→ [08 — Persistent Storage](../08-persistent-storage/)
