# Gateway API — kubectl Commands

```bash
# Install Gateway API CRDs (standard channel)
kubectl apply -f https://github.com/kubernetes-sigs/gateway-api/releases/latest/download/standard-install.yaml

# GatewayClass
kubectl get gatewayclass
kubectl describe gatewayclass <name>

# Gateway
kubectl get gateway
kubectl get gateway -n <namespace>
kubectl describe gateway <name>

# HTTPRoute
kubectl get httproute
kubectl get httproute -n <namespace>
kubectl describe httproute <name>

# Check status
kubectl get gateway <name> -o jsonpath='{.status.conditions[*]}'
kubectl get httproute <name> -o jsonpath='{.status.parents[*]}'

# List all Gateway API resources
kubectl get gatewayclass,gateway,httproute,grpcroute,tlsroute -A
```
