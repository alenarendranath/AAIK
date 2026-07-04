# Pod Lifecycle — kubectl Commands Reference

## Checking Pod Status
```bash
kubectl get pod <name>
kubectl get pod <name> -o yaml | grep -A5 "phase:"
kubectl get pod <name> -o jsonpath='{.status.phase}'
kubectl get pod <name> -o jsonpath='{.status.conditions}'
kubectl get pod <name> -o jsonpath='{.status.containerStatuses}'
kubectl describe pod <name>                        # full details incl. Events
```

## Watching Pod Changes
```bash
kubectl get pods -w                               # watch pod state changes
kubectl get events -w                             # watch events live
kubectl get events --field-selector involvedObject.name=<pod-name>
```

## Restart & Health
```bash
kubectl get pod <name> -o jsonpath='{.status.containerStatuses[0].restartCount}'
kubectl logs <pod> --previous                     # logs of previous (crashed) container
kubectl describe pod <name> | grep -A5 "Liveness:"
kubectl describe pod <name> | grep -A5 "Readiness:"
```

## Grace Period
```bash
kubectl delete pod <name> --grace-period=10       # 10 second grace period
kubectl delete pod <name> --grace-period=0 --force  # immediate (use cautiously)
```

## Simulating Probe Failures
```bash
# Make liveness probe fail (exec type)
kubectl exec <pod> -- rm /tmp/healthy

# Watch the restart
kubectl get pod <pod> -w
```
