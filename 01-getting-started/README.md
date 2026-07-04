# 01 — Getting Started with Kubernetes

## What is Kubernetes?

Kubernetes (K8s) is an open-source platform for **automating deployment, scaling, and management of containerized applications**. Think of it as an operating system for your cluster.

## Cluster Architecture

```
┌─────────────────────────────────────────────────────┐
│                   CONTROL PLANE                      │
│  ┌──────────┐  ┌──────┐  ┌───────────┐  ┌───────┐  │
│  │API Server│  │ etcd │  │ Scheduler │  │Ctrller│  │
│  └──────────┘  └──────┘  └───────────┘  └───────┘  │
└─────────────────────────────────────────────────────┘
         │               │               │
┌────────▼───┐   ┌───────▼──┐   ┌───────▼──┐
│ WORKER NODE│   │WORKER NODE│   │WORKER NODE│
│  kubelet   │   │  kubelet  │   │  kubelet  │
│ kube-proxy │   │ kube-proxy│   │ kube-proxy│
│ containers │   │ containers│   │ containers│
└────────────┘   └──────────┘   └──────────┘
```

### Control Plane Components
| Component | Role |
|-----------|------|
| **API Server** | Front door to Kubernetes; all communication goes through it |
| **etcd** | Distributed key-value store; stores all cluster state |
| **Scheduler** | Decides which node a pod should run on |
| **Controller Manager** | Runs controllers (ReplicaSet, Deployment, etc.) |

### Worker Node Components
| Component | Role |
|-----------|------|
| **kubelet** | Agent on each node; ensures containers are running |
| **kube-proxy** | Handles network routing for Services |
| **Container Runtime** | Runs containers (containerd, CRI-O) |

## Setting Up a Cluster

### Option 1: Docker Desktop (Simplest)
1. Install Docker Desktop
2. Go to Settings → Kubernetes → Enable Kubernetes
3. Click Apply & Restart

### Option 2: Minikube
```bash
# Install
brew install minikube        # macOS
# choco install minikube     # Windows

# Start
minikube start
minikube start --nodes=3     # multi-node

# Dashboard
minikube dashboard
```

### Option 3: Kind (Kubernetes in Docker)
```bash
# Install
brew install kind

# Create single-node cluster
kind create cluster --name aaik

# Create multi-node cluster
cat <<EOF | kind create cluster --name aaik --config=-
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
- role: worker
EOF

# Delete cluster
kind delete cluster --name aaik
```

### Option 4: GKE (Google Cloud)
```bash
gcloud container clusters create aaik-cluster \
  --num-nodes=3 --zone=us-central1-a
gcloud container clusters get-credentials aaik-cluster \
  --zone=us-central1-a
```

### Option 5: EKS (AWS)
```bash
eksctl create cluster --name aaik-cluster --nodes=3 --region=us-east-1
```

## kubectl Setup

```bash
# Install kubectl (macOS)
brew install kubectl

# Verify
kubectl version --client

# Enable tab completion (bash)
source <(kubectl completion bash)
echo "source <(kubectl completion bash)" >> ~/.bashrc

# Enable tab completion (zsh)
source <(kubectl completion zsh)

# Handy alias
alias k=kubectl
complete -o default -F __start_kubectl k
```

## kubeconfig

kubectl uses `~/.kube/config` to store cluster credentials.

```bash
# View current config
kubectl config view

# List contexts (clusters)
kubectl config get-contexts

# Switch context
kubectl config use-context kind-aaik

# Set default namespace
kubectl config set-context --current --namespace=default
```

## Next Steps
→ [02 — Kubernetes API & Object Model](../02-kubernetes-api/)
