# Full Web App — End-to-End Example

A complete production-style web application using:
- **ConfigMap** — app configuration
- **PersistentVolumeClaim** — persistent storage
- **Deployment** — 3 replicas with rolling updates
- **HorizontalPodAutoscaler** — auto-scales on CPU
- **Service** — ClusterIP load balancing
- **Ingress** — external HTTP routing

## Apply All at Once
```bash
kubectl apply -f .
kubectl get all -l app=aaik-web -w
```

## Verify
```bash
kubectl get configmap aaik-web-config
kubectl get pvc aaik-web-pvc
kubectl get deployment aaik-web
kubectl get hpa aaik-web-hpa
kubectl get service aaik-web-svc
kubectl get ingress aaik-web-ingress
```

## Clean Up
```bash
kubectl delete -f .
```
