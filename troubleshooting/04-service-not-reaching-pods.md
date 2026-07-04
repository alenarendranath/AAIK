# Service Not Reaching Pods

## Symptoms
- `curl <service-ip>` times out or connection refused
- App returns errors connecting to another service

## Diagnose
```bash
# 1. Does the Service exist and have an IP?
kubectl get svc aaik-svc

# 2. Does the Service have Endpoints?
kubectl get endpoints aaik-svc
# Healthy: ENDPOINTS column shows pod IPs
# Broken:  ENDPOINTS shows <none>

# 3. Do the pod labels match the Service selector?
kubectl describe svc aaik-svc            # shows Selector
kubectl get pods --show-labels            # shows pod labels

# 4. Are the target pods actually Running and Ready?
kubectl get pods -l app=aaik

# 5. Test connectivity from inside cluster
kubectl run debug --image=busybox --rm -it -- /bin/sh
# inside:
wget -qO- http://aaik-svc.default.svc.cluster.local
nslookup aaik-svc.default.svc.cluster.local
```

## Common Causes & Fixes

### Label mismatch (most common)
```bash
# Service selector:  app: aaik
# Pod label:         app: aaik-web   ← mismatch!
# Fix: make selector and pod labels match exactly
kubectl patch svc aaik-svc -p '{"spec":{"selector":{"app":"aaik-web"}}}'
```

### Wrong targetPort
```bash
# Service targetPort: 8080  but container listens on 80
kubectl describe svc aaik-svc | grep TargetPort
kubectl describe pod <pod> | grep containerPort
# Fix: match targetPort to containerPort
```

### Pod not Ready (readinessProbe failing)
```bash
kubectl describe pod <pod> | grep -A10 Readiness
# Fix: fix the readiness probe path or delay
```

### NetworkPolicy blocking traffic
```bash
kubectl get networkpolicies
kubectl describe networkpolicy <name>
# Fix: add ingress rule allowing the source namespace/pod
```
