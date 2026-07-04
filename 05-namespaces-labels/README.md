# 05 — Namespaces, Labels & Annotations

## Namespaces

Namespaces provide **logical isolation** for resources within a cluster. Think of them as virtual clusters inside your physical cluster.

```
Cluster
├── namespace: default       ← resources with no namespace specified go here
├── namespace: kube-system   ← Kubernetes system components
├── namespace: kube-public   ← public cluster info
└── namespace: aaik-prod     ← your app in production
└── namespace: aaik-dev      ← your app in development
```

> ⚠️ Namespaces do NOT provide network isolation. Pods across namespaces can communicate unless NetworkPolicies are applied.

### Default Namespaces
| Namespace | Purpose |
|-----------|---------|
| `default` | Default for user resources |
| `kube-system` | Kubernetes system components |
| `kube-public` | Cluster info readable by all |
| `kube-node-lease` | Node heartbeat objects |

## Labels

Labels are **key/value pairs** attached to objects. They are used for:
- **Organizing** resources logically
- **Selecting** resources with selectors
- **Grouping** for Services, ReplicaSets, Deployments

```yaml
metadata:
  labels:
    app: aaik           # application name
    tier: frontend      # layer in architecture
    env: production     # environment
    version: "2.1"     # app version
    release: stable     # release channel
```

### Label Key Syntax
- Optional prefix: `company.com/key` (DNS subdomain format)
- Name: max 63 chars, alphanumeric + `-`, `_`, `.`
- Value: max 63 chars, same rules

### Label Selectors
```bash
# Equality-based
app=aaik             # app equals aaik
env!=staging         # env is not staging

# Set-based
app in (aaik, quiz)  # app is aaik OR quiz
env notin (dev,test) # env is NOT dev or test
!canary              # key 'canary' does NOT exist
canary               # key 'canary' exists
```

## Recommended Labels (kubernetes.io standard)
```yaml
labels:
  app.kubernetes.io/name: aaik
  app.kubernetes.io/component: frontend
  app.kubernetes.io/version: "1.0.0"
  app.kubernetes.io/part-of: aaik-platform
  app.kubernetes.io/managed-by: helm
```

## Annotations

Annotations are key/value pairs for **non-identifying metadata** — not used for selection.

Use annotations to store:
- Build info (commit hash, pipeline URL)
- Contact info (team, owner)
- Tool-specific config (ingress nginx settings, etc.)

```yaml
metadata:
  annotations:
    kubernetes.io/change-cause: "Updated to v1.2.0"
    prometheus.io/scrape: "true"
    prometheus.io/port: "9090"
    git-commit: "abc123def456"
    deployed-by: "github-actions"
```

## Field Selectors

Filter objects by their field values (not labels):
```bash
kubectl get pods --field-selector status.phase=Running
kubectl get pods --field-selector spec.nodeName=worker-1
kubectl get pods --field-selector metadata.namespace=aaik-prod
```

## Next Steps
→ [06 — ConfigMaps & Secrets](../06-configmaps-secrets/)
