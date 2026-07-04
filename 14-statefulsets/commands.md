# StatefulSets — kubectl Commands

```bash
# View
kubectl get statefulsets
kubectl get sts
kubectl get sts -o wide
kubectl describe sts <name>

# Scale
kubectl scale sts <name> --replicas=5

# Ordered pod names
kubectl get pods -l app=<sts-label>      # shows pod-0, pod-1, pod-2 ...

# Pod DNS (from inside cluster)
# <pod-name>.<headless-service>.<namespace>.svc.cluster.local
# e.g.: aaik-db-0.aaik-db-headless.default.svc.cluster.local

# Exec into specific pod
kubectl exec -it aaik-db-0 -- /bin/sh
kubectl exec -it aaik-db-1 -- /bin/sh

# View PVCs created by volumeClaimTemplates
kubectl get pvc                           # one per pod
kubectl get pvc -l app=<sts-label>

# Delete StatefulSet (PVCs survive by default)
kubectl delete sts <name>
kubectl delete sts <name> --cascade=orphan   # keep pods

# Rollout
kubectl rollout status sts/<name>
kubectl rollout history sts/<name>
kubectl rollout undo sts/<name>

# Update image
kubectl set image sts/<name> <container>=<new-image>
```
