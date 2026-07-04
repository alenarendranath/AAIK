# 13 — Deployments

## What is a Deployment?

A **Deployment** manages ReplicaSets and enables **controlled updates** to your application with zero downtime.

```
Deployment (aaik-deploy)
├── ReplicaSet v1 (old) — scaled to 0 after update
└── ReplicaSet v2 (new) — scaled to 3 after update
```

Each time you update a Deployment, it creates a **new ReplicaSet** and gradually scales it up while scaling down the old one.

## Update Strategies

### RollingUpdate (default)
Gradually replaces old pods with new ones. No downtime.
- `maxSurge`: max pods ABOVE desired during update (e.g., 1 or 25%)
- `maxUnavailable`: max pods BELOW desired during update (e.g., 0 or 25%)

### Recreate
Terminates ALL old pods first, then starts new ones. Causes downtime.
Use when: old and new versions cannot run simultaneously.

## Rollback

Every update is tracked. You can roll back to any previous revision:
```bash
kubectl rollout undo deployment aaik           # roll back to previous
kubectl rollout undo deployment aaik --to-revision=2
```

## Deployment Strategies

| Strategy | How |
|----------|-----|
| **Canary** | Pause rollout at partial completion; test; resume or rollback |
| **Blue/Green** | Run two full environments; switch traffic instantly |
| **A/B Testing** | Route % of traffic to different versions via label selectors |

## Next Steps
→ [14 — StatefulSets](../14-statefulsets/)
