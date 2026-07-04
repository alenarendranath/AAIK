# Kubernetes API — kubectl Commands Reference

## Exploring the API
```bash
kubectl api-resources                             # list all resource types
kubectl api-resources --namespaced=true           # only namespaced resources
kubectl api-resources --namespaced=false          # only cluster-scoped resources
kubectl api-resources --api-group=apps            # resources in apps group
kubectl api-versions                              # all API versions available
```

## kubectl explain (built-in docs)
```bash
kubectl explain pod                               # explain Pod resource
kubectl explain pod.spec                          # explain pod spec fields
kubectl explain pod.spec.containers               # explain containers array
kubectl explain pod.spec.containers.image         # explain specific field
kubectl explain deployment --recursive            # full field tree
```

## Getting Objects
```bash
kubectl get pods                                  # list pods (default namespace)
kubectl get pods -o wide                          # with extra columns
kubectl get pods -o yaml                          # full YAML output
kubectl get pods -o json                          # JSON output
kubectl get pods -o jsonpath='{.items[*].metadata.name}'
kubectl get all                                   # get all common resources
kubectl get all -A                                # all namespaces
```

## Describing Objects
```bash
kubectl describe pod <name>                       # detailed pod info
kubectl describe node <name>                      # detailed node info
kubectl describe all                              # describe all resources
```

## Events
```bash
kubectl get events                                # all events
kubectl get events -w                             # watch events live
kubectl get events --field-selector type=Warning  # only warnings
kubectl get events --field-selector involvedObject.name=<pod-name>
kubectl get events --sort-by='.lastTimestamp'     # sort by time
```

## Raw API Access
```bash
kubectl proxy &                                   # start proxy
curl http://localhost:8001/api/v1/nodes           # list nodes via API
curl http://localhost:8001/apis/apps/v1           # apps API group
```
