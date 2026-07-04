# CKA Exam Tips & Strategy

## Before You Start
- Set your default namespace if the question specifies one:
  `kubectl config set-context --current --namespace=<ns>`
- Use aliases to save time:
  ```bash
  alias k=kubectl
  export do="--dry-run=client -o yaml"
  # Then: k run nginx --image=nginx $do > pod.yaml
  ```

## Time-Saving Tricks

### Generate YAML instead of writing from scratch
```bash
# Pod
kubectl run mypod --image=nginx:alpine --dry-run=client -o yaml > pod.yaml

# Deployment
kubectl create deployment myapp --image=nginx --replicas=3 --dry-run=client -o yaml > deploy.yaml

# Service
kubectl expose deployment myapp --port=80 --dry-run=client -o yaml > svc.yaml

# ConfigMap
kubectl create configmap myconfig --from-literal=key=val --dry-run=client -o yaml

# ServiceAccount
kubectl create serviceaccount mysa --dry-run=client -o yaml

# Role
kubectl create role myrole --verb=get,list --resource=pods --dry-run=client -o yaml

# RoleBinding
kubectl create rolebinding myrb --role=myrole --serviceaccount=default:mysa --dry-run=client -o yaml

# Job
kubectl create job myjob --image=busybox --dry-run=client -o yaml -- /bin/sh -c "echo done"

# CronJob
kubectl create cronjob mycron --image=busybox --schedule="*/5 * * * *" --dry-run=client -o yaml
```

### kubectl explain — look up fields in the exam
```bash
kubectl explain pod.spec.containers.livenessProbe
kubectl explain deployment.spec.strategy
kubectl explain pvc.spec
kubectl explain ingress.spec.rules
```

## Common Exam Tasks

### Scale
```bash
kubectl scale deployment myapp --replicas=5
```

### Update image
```bash
kubectl set image deployment/myapp container=nginx:1.25
kubectl rollout status deployment/myapp
```

### Rollback
```bash
kubectl rollout undo deployment/myapp
```

### Create a pod in a specific namespace
```bash
kubectl run mypod --image=nginx -n mynamespace
```

### Label a node
```bash
kubectl label node node01 disk=ssd
```

### Taint & tolerate a node
```bash
kubectl taint node node01 key=val:NoSchedule
# In pod spec:
# tolerations:
# - key: "key"
#   operator: "Equal"
#   value: "val"
#   effect: "NoSchedule"
```

### Drain a node
```bash
kubectl drain node01 --ignore-daemonsets --delete-emptydir-data
kubectl uncordon node01
```

### Static pods
```bash
# Static pod manifests live on the node at:
/etc/kubernetes/manifests/
# SSH to the node and create a YAML there
ssh node01
cat > /etc/kubernetes/manifests/mypod.yaml << 'EOF'
apiVersion: v1
kind: Pod
...
EOF
```

### Check component status
```bash
kubectl get componentstatuses
kubectl get pods -n kube-system
```

## Key Weightings (CKA 2025)
| Domain | Weight |
|--------|--------|
| Cluster Architecture, Installation & Configuration | 25% |
| Workloads & Scheduling | 15% |
| Services & Networking | 20% |
| Storage | 10% |
| Troubleshooting | 30% |
