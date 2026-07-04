# 10 — Ingress

## What is Ingress?

**Ingress** is a Kubernetes resource that manages **external HTTP/HTTPS access** to services, typically from a single external IP.

```
Internet → LoadBalancer IP → Ingress Controller → Rules:
                                                  /app-a → Service A
                                                  /app-b → Service B
                                                  api.example.com → API Service
```

Without Ingress: each Service needs its own LoadBalancer (expensive).
With Ingress: one LoadBalancer routes to many services via host/path rules.

## Ingress Controller

An Ingress resource does nothing on its own. It needs an **Ingress Controller** (a reverse proxy) to implement the rules:

| Controller | Notes |
|------------|-------|
| **nginx** | Most popular; install via Helm or manifest |
| **Traefik** | Auto-discovers services |
| **HAProxy** | Enterprise-grade |
| **GKE Ingress** | Managed by Google Cloud |

```bash
# Install nginx ingress controller (generic)
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.10.0/deploy/static/provider/cloud/deploy.yaml
```

## Path Types

| Type | Behavior |
|------|----------|
| `Exact` | Matches exact path only (`/app` matches only `/app`) |
| `Prefix` | Matches path prefix (`/app` matches `/app`, `/app/v1`, etc.) |
| `ImplementationSpecific` | Controller decides (nginx uses regex) |

## IngressClass

When multiple controllers exist, use IngressClass to specify which one handles a given Ingress:
```yaml
spec:
  ingressClassName: nginx
```

## Next Steps
→ [11 — Gateway API](../11-gateway-api/)
