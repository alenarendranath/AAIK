# Deployments — kubectl Commands

```bash
# Create
kubectl create deployment aaik --image=nginx:alpine --replicas=3
kubectl apply -f deployment.yaml

# View
kubectl get deployments
kubectl get deploy
kubectl get deploy -o wide
kubectl describe deploy <name>

# Scale
kubectl scale deployment aaik --replicas=5
kubectl autoscale deployment aaik --min=2 --max=10 --cpu-percent=80

# Update image
kubectl set image deployment/aaik aaik=nginx:1.25
kubectl set image deployment/aaik aaik=nginx:alpine --record  # deprecated but still used

# Rollout management
kubectl rollout status deployment/aaik          # watch progress
kubectl rollout history deployment/aaik         # view revisions
kubectl rollout history deployment/aaik --revision=2
kubectl rollout pause deployment/aaik           # pause (canary)
kubectl rollout resume deployment/aaik          # resume
kubectl rollout undo deployment/aaik            # roll back to previous
kubectl rollout undo deployment/aaik --to-revision=1

# Patch
kubectl patch deployment aaik -p '{"spec":{"template":{"metadata":{"annotations":{"redeploy":"true"}}}}}'

# Delete
kubectl delete deployment aaik
kubectl delete deploy aaik --cascade=orphan     # keep pods
```
