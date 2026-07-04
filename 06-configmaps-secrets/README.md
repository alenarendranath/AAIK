# 06 — ConfigMaps & Secrets

## ConfigMaps

A **ConfigMap** stores **non-sensitive** configuration data as key-value pairs. It decouples configuration from container images.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: aaik-config
data:
  # Simple key-value pairs
  APP_ENV: "production"
  LOG_LEVEL: "info"
  MAX_CONNECTIONS: "100"
  # Multi-line value (file content)
  nginx.conf: |
    server {
      listen 80;
      location / { return 200 'AleK8s\n'; }
    }
```

### Ways to Inject ConfigMap into a Pod

1. **Individual env var** (`valueFrom.configMapKeyRef`)
2. **All keys as env vars** (`envFrom.configMapRef`)
3. **As files in a volume** (`volumes.configMap`)

## Secrets

A **Secret** stores **sensitive data** (passwords, tokens, TLS certs). Stored base64-encoded in etcd.

| Secret Type | Usage |
|-------------|-------|
| `Opaque` | Generic key-value (default) |
| `kubernetes.io/tls` | TLS certificate and key |
| `kubernetes.io/dockerconfigjson` | Docker registry credentials |
| `kubernetes.io/service-account-token` | Service account tokens |

> ⚠️ Secrets are NOT encrypted at rest by default. Consider enabling etcd encryption or using an external secrets manager.

## Downward API

Expose pod metadata (name, namespace, labels, resource limits) to containers via:
- **Environment variables**
- **Volume files**

```yaml
env:
- name: MY_POD_NAME
  valueFrom:
    fieldRef:
      fieldPath: metadata.name
- name: MY_NODE_NAME
  valueFrom:
    fieldRef:
      fieldPath: spec.nodeName
- name: MY_CPU_LIMIT
  valueFrom:
    resourceFieldRef:
      containerName: aaik
      resource: limits.cpu
```

## Next Steps
→ [07 — Volumes](../07-volumes/)
