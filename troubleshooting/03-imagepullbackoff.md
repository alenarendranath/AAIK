# Pod Stuck in ImagePullBackOff / ErrImagePull

## Symptoms
```bash
NAME        READY   STATUS             RESTARTS   AGE
aaik-pod    0/1     ImagePullBackOff   0          2m
```

## Diagnose
```bash
kubectl describe pod aaik-pod
# Look at Events:
# Failed to pull image "my-image:latest": rpc error...
```

## Common Causes & Fixes

### 1. Image name / tag typo
```bash
# Event: "repository does not exist" or "manifest unknown"
# Fix: check the exact image name and tag
docker pull nginx:alpine    # verify locally first
# In manifest: image: nginx:alpine  (not nginx:alpnie)
```

### 2. Private registry — missing imagePullSecret
```bash
# Event: "unauthorized: authentication required"
# Fix: create a docker-registry secret and reference it
kubectl create secret docker-registry my-registry-secret \
  --docker-server=registry.example.com \
  --docker-username=<user> \
  --docker-password=<pass>

# In pod spec:
# imagePullSecrets:
# - name: my-registry-secret
```

### 3. Network issue — registry unreachable
```bash
# Event: "dial tcp: lookup registry.example.com: no such host"
# Fix: check cluster DNS and network policies
kubectl exec -it <another-pod> -- nslookup registry.example.com
```

### 4. Rate limiting (Docker Hub)
```bash
# Event: "toomanyrequests: You have reached your pull rate limit"
# Fix: use an authenticated pull secret or mirror images
```
