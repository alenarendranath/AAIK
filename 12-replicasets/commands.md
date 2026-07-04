# ReplicaSets — kubectl Commands

```bash
kubectl get rs                                    # list ReplicaSets
kubectl get replicasets                           # long form
kubectl get rs -o wide                            # with selector info
kubectl describe rs <name>
kubectl delete rs <name>
kubectl delete rs <name> --cascade=orphan         # delete RS but keep pods

# Scale
kubectl scale rs <name> --replicas=5
kubectl scale rs <name> --replicas=0              # scale to 0 (stop all pods)

# View pods owned by a ReplicaSet
kubectl get pods -l app=aaik
kubectl get pods -l app=aaik --show-labels

# Logs from RS pods
kubectl logs rs/<name>                            # logs from one pod
kubectl logs rs/<name> --all-pods                 # logs from all pods

# Exec into RS pod
kubectl exec rs/<name> -- env

# Remove a pod from ReplicaSet control (change its label)
kubectl label pod <pod-name> app=aaik-debug --overwrite
```
