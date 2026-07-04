# 11 — Gateway API

## What is the Gateway API?

The **Gateway API** is the next-generation successor to Ingress. It supports more protocols (HTTP, HTTPS, TCP, UDP, gRPC) and has a cleaner role separation.

## Key Differences: Ingress vs Gateway API

| Feature | Ingress | Gateway API |
|---------|---------|-------------|
| Protocols | HTTP/HTTPS only | HTTP, HTTPS, TCP, UDP, gRPC |
| Role separation | None | GatewayClass / Gateway / Route |
| Traffic splitting | Via annotations | Native (weight-based) |
| Header modification | Via annotations | Native |
| Cross-namespace | Limited | Full (via ReferenceGrant) |

## Resources

```
GatewayClass  ← created by infra admin (defines which controller)
    │
Gateway       ← created by cluster operator (defines listeners/ports)
    │
HTTPRoute     ← created by app developer (defines routing rules)
TLSRoute
TCPRoute
GRPCRoute
```

## Install Gateway API CRDs
```bash
kubectl apply -f https://github.com/kubernetes-sigs/gateway-api/releases/download/v1.1.0/standard-install.yaml
```

## Next Steps
→ [12 — ReplicaSets](../12-replicasets/)
