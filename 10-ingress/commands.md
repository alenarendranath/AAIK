# Ingress — kubectl Commands

```bash
kubectl get ingress                               # list ingresses
kubectl get ing                                   # shorthand
kubectl get ing -o yaml
kubectl describe ing <name>
kubectl delete ing <name>

# IngressClass
kubectl get ingressclasses
kubectl describe ingressclass nginx

# TLS Secret for Ingress
kubectl create secret tls aaik-tls --cert=tls.crt --key=tls.key

# Watch ingress get an address
kubectl get ing -w
```
