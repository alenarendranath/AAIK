# 12 — ReplicaSets

## What is a ReplicaSet?

A **ReplicaSet** ensures a specified number of identical pod replicas are always running. It is the reconciliation engine behind Deployments.

```
ReplicaSet (desired: 3)
├── Pod aaik-abc12  ✅ Running
├── Pod aaik-def34  ✅ Running
└── Pod aaik-ghi56  ✅ Running

→ If aaik-abc12 crashes, ReplicaSet creates a new pod immediately
```

## How It Works: Reconciliation Loop

1. Controller checks: **actual replicas** vs **desired replicas**
2. If actual < desired → **create** new pods
3. If actual > desired → **delete** excess pods
4. Repeats continuously

## Label Selector

The ReplicaSet finds its pods via a **label selector**. Any pod matching the selector is owned by the RS.

> ⚠️ Use Deployments instead of ReplicaSets directly. Deployments manage ReplicaSets and enable rolling updates.

## Scale-Down Priority

When scaling down, Kubernetes deletes pods in this order:
1. Pods not yet scheduled (Pending)
2. Pods in earlier phases
3. Pods with more restarts
4. Youngest pods last

## Next Steps
→ [13 — Deployments](../13-deployments/)
