# kubectl Quick Reference

## Context & Cluster
```bash
kubectl config get-contexts            # list all contexts
kubectl config current-context         # show active context
kubectl config use-context <name>      # switch context
kubectl config set-context --current --namespace=<ns>  # set default namespace
kubectl cluster-info
kubectl get nodes -o wide
```

## Pods
```bash
kubectl get pods                       # default namespace
kubectl get pods -A                    # all namespaces
kubectl get pods -o wide               # with node + IP
kubectl get pods --show-labels
kubectl get pods -l app=nginx
kubectl get pods -w                    # watch live

kubectl run nginx --image=nginx:alpine                            # quick pod
kubectl run nginx --image=nginx:alpine --port=80 --dry-run=client -o yaml  # generate YAML

kubectl describe pod <name>
kubectl logs <name>
kubectl logs <name> -f                 # follow
kubectl logs <name> --previous         # crashed container
kubectl logs <name> -c <container>     # specific container

kubectl exec -it <name> -- /bin/sh
kubectl exec <name> -- env
kubectl exec <name> -- cat /etc/hosts

kubectl delete pod <name>
kubectl delete pod <name> --grace-period=0 --force  # immediate
```

## Deployments
```bash
kubectl create deployment nginx --image=nginx:alpine --replicas=3
kubectl scale deployment nginx --replicas=5
kubectl set image deployment/nginx nginx=nginx:1.25
kubectl rollout status deployment/nginx
kubectl rollout history deployment/nginx
kubectl rollout undo deployment/nginx
kubectl rollout undo deployment/nginx --to-revision=2
```

## Services
```bash
kubectl expose deployment nginx --port=80 --type=ClusterIP
kubectl expose deployment nginx --port=80 --type=NodePort
kubectl get svc
kubectl get endpoints
kubectl port-forward svc/nginx 8080:80    # local → cluster port-forward
```

## Namespaces
```bash
kubectl get namespaces
kubectl create namespace dev
kubectl delete namespace dev
kubectl get pods -n kube-system
kubectl get all -n <namespace>
```

## ConfigMaps & Secrets
```bash
kubectl create configmap myconfig --from-literal=key=value
kubectl create configmap myconfig --from-file=config.txt
kubectl get configmap myconfig -o yaml

kubectl create secret generic mysecret --from-literal=password=s3cr3t
kubectl get secret mysecret -o jsonpath='{.data.password}' | base64 -d
```

## Storage
```bash
kubectl get pv
kubectl get pvc
kubectl get storageclass
kubectl describe pvc <name>
```

## RBAC
```bash
kubectl auth can-i get pods
kubectl auth can-i create deployments --namespace=prod
kubectl auth can-i --list
kubectl auth can-i get pods --as=system:serviceaccount:default:mysa
```

## Debugging
```bash
kubectl get events --sort-by=.metadata.creationTimestamp
kubectl get events -n <namespace> --field-selector type=Warning
kubectl top pods
kubectl top nodes
kubectl top pods --containers

# Debug with a temporary pod
kubectl run debug --image=busybox --rm -it -- /bin/sh
kubectl run debug --image=nicolaka/netshoot --rm -it -- bash

# Copy files to/from pod
kubectl cp <pod>:/path/to/file ./local-file
kubectl cp ./local-file <pod>:/path/to/file
```

## Output Formats
```bash
kubectl get pods -o yaml
kubectl get pods -o json
kubectl get pods -o wide
kubectl get pods -o name
kubectl get pod <name> -o jsonpath='{.status.podIP}'
kubectl get pods -o custom-columns=NAME:.metadata.name,STATUS:.status.phase
kubectl get pods --sort-by=.metadata.creationTimestamp
kubectl get pods --sort-by=.status.startTime
```

## Labels & Annotations
```bash
kubectl label pod <name> env=prod
kubectl label pod <name> env-         # remove label
kubectl annotate pod <name> note="test pod"
kubectl get pods -l env=prod
kubectl get pods -l 'env in (prod,staging)'
kubectl get pods -l env!=prod
```
