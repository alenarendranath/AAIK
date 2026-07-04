# ConfigMaps & Secrets — kubectl Commands

## ConfigMaps
```bash
# Create from literal values
kubectl create configmap aaik-config \
  --from-literal=APP_ENV=production \
  --from-literal=LOG_LEVEL=info

# Create from a file
kubectl create configmap aaik-config --from-file=nginx.conf

# Create from a directory (each file becomes a key)
kubectl create configmap aaik-config --from-file=./config/

# From YAML file
kubectl apply -f configmap.yaml

# View ConfigMaps
kubectl get configmaps                            # list
kubectl get cm                                    # shorthand
kubectl get cm aaik-config -o yaml               # full YAML
kubectl describe cm aaik-config                   # details

# Edit
kubectl edit configmap aaik-config

# Delete
kubectl delete configmap aaik-config
```

## Secrets
```bash
# Generic secret from literals
kubectl create secret generic aaik-secret \
  --from-literal=DB_PASSWORD=supersecret \
  --from-literal=API_KEY=myapikey123

# TLS secret from cert/key files
kubectl create secret tls aaik-tls \
  --cert=tls.crt --key=tls.key

# Docker registry secret
kubectl create secret docker-registry regcred \
  --docker-server=registry.example.com \
  --docker-username=myuser \
  --docker-password=mypassword

# View secrets (values are base64 encoded)
kubectl get secrets
kubectl get secret aaik-secret -o yaml
kubectl describe secret aaik-secret

# Decode a secret value
kubectl get secret aaik-secret -o jsonpath='{.data.DB_PASSWORD}' | base64 -d

# Delete
kubectl delete secret aaik-secret
```

## Verify injection in pod
```bash
kubectl exec <pod> -- env | grep APP_ENV
kubectl exec <pod> -- cat /etc/config/nginx.conf
```
