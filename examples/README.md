# Examples — End-to-End Applications

Real-world application examples combining multiple Kubernetes concepts.

## Available Examples

| Example | Concepts Used |
|---------|--------------|
| [full-web-app/](./full-web-app/) | Deployment, Service, Ingress, ConfigMap, PVC, HPA |
| [stateful-app/](./stateful-app/) | StatefulSet, Headless Service, PVC |

---

## How to Use

```bash
# Apply an entire example
kubectl apply -f examples/full-web-app/

# Watch everything come up
kubectl get all -l app=aaik-web -w

# Clean up
kubectl delete -f examples/full-web-app/
```
