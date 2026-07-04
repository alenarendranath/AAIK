# Pods — kubectl Commands Reference

## Creating Pods
```bash
# From YAML file
kubectl apply -f pod-simple.yaml

# Run a quick pod (imperative)
kubectl run aaik --image=nginx:alpine

# Run and expose port
kubectl run aaik --image=nginx:alpine --port=80

# Interactive pod (auto-deleted on exit)
kubectl run -it --rm debug --image=busybox --restart=Never -- sh

# Run with environment variable
kubectl run aaik --image=nginx:alpine --env="ENV=prod"
```

## Viewing Pods
```bash
kubectl get pods                                  # list pods
kubectl get pods -o wide                          # with node & IP
kubectl get pods -o yaml                          # full YAML
kubectl get pods -w                               # watch changes
kubectl get pods --show-labels                    # show labels
kubectl get pods -A                               # all namespaces
kubectl describe pod <name>                       # detailed info
```

## Interacting with Pods
```bash
# Port forwarding
kubectl port-forward pod/<name> 8080:80           # local:container

# Execute commands
kubectl exec <pod> -- ls /                        # single command
kubectl exec <pod> -- env                         # show env vars
kubectl exec -it <pod> -- bash                    # interactive shell
kubectl exec -it <pod> -c <container> -- sh       # specific container

# Copy files
kubectl cp <pod>:/etc/nginx/nginx.conf ./nginx.conf
kubectl cp ./config.txt <pod>:/tmp/config.txt

# Logs
kubectl logs <pod>                                # current logs
kubectl logs <pod> -f                             # follow/stream
kubectl logs <pod> --previous                     # previous container logs
kubectl logs <pod> -c <container>                 # specific container
kubectl logs <pod> --all-containers               # all containers
kubectl logs <pod> --since=1h                     # last 1 hour
kubectl logs <pod> --tail=50                      # last 50 lines
kubectl logs <pod> --timestamps                   # with timestamps
```

## Attaching to Pods
```bash
kubectl attach <pod>                              # attach to stdout
kubectl attach -i <pod>                           # with stdin
```

## Debugging
```bash
# Add ephemeral debug container (K8s 1.23+)
kubectl debug <pod> -it --image=nicolaka/netshoot

# Debug with copy of pod
kubectl debug <pod> -it --image=busybox --copy-to=debug-pod

# Check events for a pod
kubectl get events --field-selector involvedObject.name=<pod>
```

## Deleting Pods
```bash
kubectl delete pod <name>                         # graceful delete
kubectl delete pod <name> --grace-period=0        # immediate
kubectl delete pod -l app=aaik                    # by label
kubectl delete pods --all                         # all pods
kubectl delete -f pod.yaml                        # delete from file
```
