# Getting Started — kubectl Commands Reference

## Cluster Info
```bash
kubectl cluster-info
kubectl cluster-info dump                         # full cluster dump
kubectl version                                   # client + server version
kubectl api-versions                              # list all API versions
kubectl api-resources                             # list all resource types
```

## Nodes
```bash
kubectl get nodes                                 # list nodes
kubectl get nodes -o wide                         # with IP, OS, runtime
kubectl get nodes -o yaml                         # full YAML
kubectl describe node <node-name>                 # detailed info
kubectl top nodes                                 # CPU/memory usage (metrics-server required)
```

## kubectl proxy
```bash
kubectl proxy                                     # starts proxy on localhost:8001
# Then access: curl http://localhost:8001/api/v1/nodes
```

## Minikube
```bash
minikube start
minikube stop
minikube delete
minikube status
minikube dashboard
minikube ssh                                      # SSH into the node
minikube service <service-name> --url            # get service URL
```

## Kind
```bash
kind create cluster --name aaik
kind get clusters
kind delete cluster --name aaik
kind get nodes --name aaik
```
