# DaemonSets — kubectl Commands

```bash
# View
kubectl get daemonsets
kubectl get ds
kubectl get ds -o wide
kubectl describe ds <name>

# Pods (one per node)
kubectl get pods -l app=<ds-label> -o wide    # shows which node each pod is on

# Rollout
kubectl rollout status ds/<name>
kubectl rollout history ds/<name>
kubectl rollout undo ds/<name>

# Update image
kubectl set image ds/<name> <container>=<new-image>

# Update strategy patch
kubectl patch ds <name> -p '{"spec":{"updateStrategy":{"type":"RollingUpdate","rollingUpdate":{"maxUnavailable":2}}}}'

# Delete (removes all ds pods from all nodes)
kubectl delete ds <name>
kubectl delete ds <name> --cascade=orphan    # keep pods

# Node selectors — label a node
kubectl label node <node-name> gpu=cuda
kubectl label node <node-name> gpu-        # remove label
```
