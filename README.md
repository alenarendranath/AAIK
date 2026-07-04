# AleK8s — Ale's Kubernetes Learning Repository

> **AleK8s** is a comprehensive, hands-on Kubernetes learning repository covering all core topics with original YAML manifests, kubectl command references, end-to-end examples, troubleshooting guides, and CKA exam cheatsheets.

![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)
![Original Content](https://img.shields.io/badge/Content-Original-green.svg)
![Kubernetes](https://img.shields.io/badge/Kubernetes-1.29%2B-326CE5?logo=kubernetes)

---

## Topics

| # | Topic | Concepts |
|---|-------|----------|
| 01 | [Getting Started](./01-getting-started/) | minikube, kind, kubectl, kubeconfig |
| 02 | [Kubernetes API](./02-kubernetes-api/) | API groups, object model, kubectl explain |
| 03 | [Pods](./03-pods/) | Single & multi-container, init containers, native sidecars |
| 04 | [Pod Lifecycle](./04-pod-lifecycle/) | Liveness, readiness, startup probes, lifecycle hooks |
| 05 | [Namespaces & Labels](./05-namespaces-labels/) | Namespaces, labels, selectors, annotations |
| 06 | [ConfigMaps & Secrets](./06-configmaps-secrets/) | Config injection, env vars, volume mounts |
| 07 | [Volumes](./07-volumes/) | emptyDir, hostPath, projected volumes |
| 08 | [Persistent Storage](./08-persistent-storage/) | PV, PVC, StorageClass, dynamic provisioning |
| 09 | [Services](./09-services/) | ClusterIP, NodePort, LoadBalancer, headless |
| 10 | [Ingress](./10-ingress/) | Routing rules, TLS, multi-path |
| 11 | [Gateway API](./11-gateway-api/) | GatewayClass, Gateway, HTTPRoute |
| 12 | [ReplicaSets](./12-replicasets/) | Reconciliation, selectors, HPA |
| 13 | [Deployments](./13-deployments/) | Rolling updates, canary, blue-green |
| 14 | [StatefulSets](./14-statefulsets/) | Stable identity, ordered scaling, volumeClaimTemplates |
| 15 | [DaemonSets](./15-daemonsets/) | Per-node pods, log agents, monitoring |
| 16 | [Jobs & CronJobs](./16-jobs-cronjobs/) | One-off tasks, parallel jobs, scheduled jobs |
| 17 | [Security](./17-security/) | RBAC, ServiceAccounts, PodSecurity |
| 18 | [Extending Kubernetes](./18-extending-kubernetes/) | CRDs, Operators, Admission Webhooks |

---

## Extras

| Folder | Contents |
|--------|----------|
| [examples/](./examples/) | End-to-end multi-manifest applications |
| [troubleshooting/](./troubleshooting/) | Common failures with diagnosis and fixes |
| [cheatsheets/](./cheatsheets/) | CKA exam tips, kubectl quick reference, YAML patterns |

---

## Usage

```bash
git clone https://github.com/alenarendranath/AleK8s.git
cd AleK8s

# Apply a topic
kubectl apply -f 03-pods/manifests/01-pod-simple.yaml

# Apply a full end-to-end example
kubectl apply -f examples/full-web-app/
```

---

## License

MIT License — see [LICENSE](./LICENSE). Original educational content by Narendranath.  
See [DISCLAIMER.md](./DISCLAIMER.md) for third-party acknowledgements.
