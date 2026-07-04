# Security — kubectl Commands

## ServiceAccounts
```bash
kubectl get serviceaccounts
kubectl get sa
kubectl describe sa <name>
kubectl create sa <name>
kubectl delete sa <name>

# View mounted token inside a pod
kubectl exec <pod> -- cat /var/run/secrets/kubernetes.io/serviceaccount/token
kubectl exec <pod> -- cat /var/run/secrets/kubernetes.io/serviceaccount/namespace
```

## RBAC — Roles & ClusterRoles
```bash
kubectl get roles
kubectl get clusterroles
kubectl describe role <name>
kubectl describe clusterrole <name>

# Quick create
kubectl create role pod-reader --verb=get,list,watch --resource=pods
kubectl create clusterrole pod-reader --verb=get,list,watch --resource=pods
```

## RBAC — Bindings
```bash
kubectl get rolebindings
kubectl get clusterrolebindings
kubectl describe rolebinding <name>

# Bind a role to a service account
kubectl create rolebinding aaik-binding \
  --role=pod-reader \
  --serviceaccount=default:aaik-sa

# Bind to a user
kubectl create rolebinding aaik-user-binding \
  --clusterrole=view \
  --user=narendranath \
  --namespace=default
```

## Check Permissions
```bash
# Can I do this?
kubectl auth can-i get pods
kubectl auth can-i create deployments --namespace=production
kubectl auth can-i "*" "*"                               # am I cluster-admin?

# Can a specific SA do this?
kubectl auth can-i get pods --as=system:serviceaccount:default:aaik-sa

# List what a user can do
kubectl auth can-i --list
kubectl auth can-i --list --as=system:serviceaccount:default:aaik-sa
```

## PodSecurity
```bash
# Label a namespace with security level
kubectl label namespace default pod-security.kubernetes.io/enforce=baseline
kubectl label namespace default pod-security.kubernetes.io/warn=restricted

# Check namespace labels
kubectl get namespace default -o yaml
```
