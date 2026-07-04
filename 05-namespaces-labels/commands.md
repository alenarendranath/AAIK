# Namespaces, Labels & Annotations — kubectl Commands

## Namespaces
```bash
kubectl get namespaces                            # list namespaces
kubectl get ns                                    # shorthand
kubectl create namespace aaik-dev                 # create namespace
kubectl apply -f namespace.yaml                   # from file
kubectl delete namespace aaik-dev                 # delete (and ALL resources in it!)

# Work in a specific namespace
kubectl get pods -n aaik-dev                      # -n or --namespace
kubectl get pods --all-namespaces                 # across all namespaces
kubectl get pods -A                               # shorthand

# Set default namespace for current context
kubectl config set-context --current --namespace=aaik-dev
```

## Labels
```bash
# View labels
kubectl get pods --show-labels                    # show all labels
kubectl get pods -L app,env,version               # show specific labels as columns

# Filter by label
kubectl get pods -l app=aaik                      # equality
kubectl get pods -l app=aaik,env=prod             # multiple (AND)
kubectl get pods -l 'env in (prod,staging)'       # set-based
kubectl get pods -l 'app,!canary'                 # has app, no canary key

# Add/update labels
kubectl label pod <name> env=prod                 # add label
kubectl label pod <name> env=staging --overwrite  # update label
kubectl label pod <name> env-                     # remove label (note the dash)
kubectl label pods --all suite=aaik               # label all pods

# Label nodes
kubectl label node <node-name> gpu=cuda
kubectl label node <node-name> gpu-              # remove
```

## Annotations
```bash
kubectl annotate pod <name> description="AAIK main pod"
kubectl annotate pod <name> description="updated" --overwrite
kubectl annotate pod <name> description-          # remove annotation
kubectl describe pod <name> | grep -A5 Annotations:
```

## Field Selectors
```bash
kubectl get pods --field-selector status.phase=Running
kubectl get pods --field-selector spec.nodeName=kind-worker
kubectl get pods --field-selector status.phase!=Running
```
