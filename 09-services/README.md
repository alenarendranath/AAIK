# 09 — Services & Networking

## Why Services?

Pods are **ephemeral** — they can be replaced, rescheduled, and their IPs change. A **Service** provides a **stable virtual IP and DNS name** for a group of pods.

```
Clients → Service (stable IP: 10.96.0.1) → Pod A (10.1.0.1)
                                          → Pod B (10.1.0.2)
                                          → Pod C (10.1.0.3)
```

## Service Types

| Type | Access | Use Case |
|------|--------|----------|
| `ClusterIP` | Inside cluster only | Internal microservice communication |
| `NodePort` | Via any node's IP:port | Dev/testing, bare-metal clusters |
| `LoadBalancer` | Via external cloud LB | Production external exposure |
| `ExternalName` | DNS alias | Route to external services |

## How Services Work

A Service uses a **label selector** to find its backend pods:
```yaml
selector:
  app: aaik    # routes to all pods with label app=aaik
```

kube-proxy watches the API and programs iptables/IPVS rules on each node to route traffic.

## DNS for Services

Every Service gets a DNS name automatically:
```
<service-name>.<namespace>.svc.cluster.local
```

Examples:
- `aaik` — from same namespace
- `aaik.default` — from any namespace
- `aaik.default.svc.cluster.local` — fully qualified

## Headless Services (`clusterIP: None`)

No virtual IP is created. DNS returns the **actual pod IPs** directly. Used for:
- StatefulSets (stable per-pod DNS)
- Direct pod-to-pod communication

## Endpoints & EndpointSlices

Behind every Service is an Endpoints/EndpointSlice object that lists the ready pod IPs. Kubernetes keeps this in sync with pod readiness.

```bash
kubectl get endpoints aaik-service
kubectl get endpointslices -l kubernetes.io/service-name=aaik-service
```

## Next Steps
→ [10 — Ingress](../10-ingress/)
