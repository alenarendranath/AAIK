# 17 — Security: RBAC, ServiceAccounts & PodSecurity

## Overview

Kubernetes security has three main pillars:

```
User/Pod wants to do something
       ↓
  Authentication   → Who are you?
       ↓
  Authorization    → Are you allowed? (RBAC)
       ↓
  Admission        → Does it follow policy? (PodSecurity)
```

---

## ServiceAccounts

Every pod runs as a **ServiceAccount** (SA). The SA's token is automatically mounted at:
`/var/run/secrets/kubernetes.io/serviceaccount/token`

```
Pod → uses → ServiceAccount → bound to → Role/ClusterRole via RoleBinding
```

- `default` SA exists in every namespace but has no permissions by default
- Always create a dedicated SA for each app (least privilege)

---

## RBAC

**Role-Based Access Control** — defines WHO can do WHAT to WHICH resources.

### Key Objects

| Object | Scope | Purpose |
|--------|-------|---------|
| `Role` | Namespace | Permissions within one namespace |
| `ClusterRole` | Cluster-wide | Permissions across all namespaces or non-namespaced resources |
| `RoleBinding` | Namespace | Grants Role or ClusterRole to a subject in a namespace |
| `ClusterRoleBinding` | Cluster-wide | Grants ClusterRole to a subject cluster-wide |

### Subjects (who)
- `User` — human user (managed externally, e.g., certificates)
- `Group` — group of users
- `ServiceAccount` — pod identity

### Verbs (what)

| Verb | HTTP |
|------|------|
| get, list, watch | GET |
| create | POST |
| update, patch | PUT/PATCH |
| delete, deletecollection | DELETE |

---

## PodSecurity Admission

Enforces security standards at the **namespace level** via labels:

```yaml
labels:
  pod-security.kubernetes.io/enforce: restricted   # blocks non-compliant pods
  pod-security.kubernetes.io/warn: restricted       # warns but allows
  pod-security.kubernetes.io/audit: restricted      # logs violations
```

### Security Levels

| Level | What it allows |
|-------|---------------|
| `privileged` | Everything (for system components) |
| `baseline` | Prevents known privilege escalations |
| `restricted` | Hardened (no root, no host network, drop ALL caps) |

---

## Next Steps
→ [18 — Extending Kubernetes](../18-extending-kubernetes/)
