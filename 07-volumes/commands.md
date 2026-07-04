# Volumes — kubectl Commands

```bash
# Inspect volumes on a running pod
kubectl describe pod <name>                        # shows Volumes section
kubectl get pod <name> -o jsonpath='{.spec.volumes}'

# Execute into container to check mounts
kubectl exec <pod> -- df -h                        # disk usage
kubectl exec <pod> -- ls -la /mounted/path
kubectl exec <pod> -- cat /etc/config/app.conf

# Check if configmap volume updated
kubectl exec <pod> -- ls -la /etc/config/          # see symlinks
kubectl exec <pod> -- cat /etc/config/key

# Shared volume between containers
kubectl exec <pod> -c container1 -- echo "hello" > /shared/file
kubectl exec <pod> -c container2 -- cat /shared/file
```
