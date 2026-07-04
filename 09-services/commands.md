# Services & Networking — kubectl Commands

## Services
```bash
kubectl get services                              # list services
kubectl get svc                                   # shorthand
kubectl get svc -o wide                           # with selector and endpoints
kubectl get svc -A                                # all namespaces
kubectl describe svc <name>                       # full details
kubectl delete svc <name>

# Create imperatively
kubectl expose pod aaik --port=80 --target-port=80 --name=aaik-svc
kubectl expose deployment aaik --type=LoadBalancer --port=80
kubectl expose deployment aaik --type=NodePort --port=80

# Change selector
kubectl set selector service aaik-svc app=aaik-v2
```

## Endpoints
```bash
kubectl get endpoints                             # list all
kubectl get ep <service-name> -o yaml            # see pod IPs
kubectl get endpointslices -l kubernetes.io/service-name=<svc>
```

## DNS Testing
```bash
# Run a debug pod to test DNS
kubectl run dns-test -it --rm --image=busybox --restart=Never -- sh
# Inside the pod:
nslookup aaik-service
nslookup aaik-service.default.svc.cluster.local
wget -qO- http://aaik-service/
```

## NodePort Access
```bash
# Get NodePort number
kubectl get svc aaik-nodeport -o jsonpath='{.spec.ports[0].nodePort}'
# Get node IP
kubectl get nodes -o wide
# Access: curl http://<node-ip>:<nodePort>

# Minikube
minikube service aaik-nodeport --url
```
