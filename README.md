# AAIK — Ale Action in Kubernetes 🚀

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-1.29+-blue.svg)](https://kubernetes.io)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

> **Original educational content** — All YAML manifests, explanations, and examples are original work by the author. See [DISCLAIMER.md](DISCLAIMER.md) for full legal notice.

A hands-on Kubernetes learning repository covering every core concept through practical YAML manifests, kubectl commands, and real-world examples.

## The Demo Application

Throughout this repository we use a simple web application called **AAIK App** (image: `nginx:alpine`) to demonstrate Kubernetes features. It is lightweight, stateless, and works on any cluster.

## Repository Structure

| # | Topic | Concepts |
|---|-------|----------|
| 01 | [Getting Started](./01-getting-started/) | Cluster setup, kubectl basics |
| 02 | [Kubernetes API & Object Model](./02-kubernetes-api/) | API, manifests, apiVersion/kind/metadata/spec/status |
| 03 | [Pods](./03-pods/) | Single/multi-container, sidecar, init containers |
| 04 | [Pod Lifecycle & Health](./04-pod-lifecycle/) | Liveness/readiness/startup probes, postStart/preStop hooks |
| 05 | [Namespaces, Labels & Annotations](./05-namespaces-labels/) | Organizing and selecting resources |
| 06 | [ConfigMaps & Secrets](./06-configmaps-secrets/) | App configuration, TLS certs, Downward API |
| 07 | [Volumes](./07-volumes/) | emptyDir, hostPath, configMap, secret, projected volumes |
| 08 | [Persistent Storage](./08-persistent-storage/) | PV, PVC, StorageClass, CSI, VolumeSnapshots |
| 09 | [Services & Networking](./09-services/) | ClusterIP, NodePort, LoadBalancer, headless, DNS |
| 10 | [Ingress](./10-ingress/) | HTTP routing, path rules, TLS termination |
| 11 | [Gateway API](./11-gateway-api/) | GatewayClass, HTTPRoute, traffic splitting, TLS |
| 12 | [ReplicaSets](./12-replicasets/) | Scaling, reconciliation loop, pod ownership |
| 13 | [Deployments](./13-deployments/) | Rolling updates, rollback, canary, blue-green |
| 14 | [StatefulSets](./14-statefulsets/) | Stable identity, ordered scaling, PVC templates |
| 15 | [DaemonSets](./15-daemonsets/) | Per-node workloads, node agents |
| 16 | [Jobs & CronJobs](./16-jobs-cronjobs/) | Batch processing, parallel jobs, scheduling |

## Prerequisites

- A running Kubernetes cluster (see [01-getting-started](./01-getting-started/))
- `kubectl` installed and configured
- Basic knowledge of containers and Docker

## Quick Start

```bash
git clone https://github.com/alenarendranath/AAIK.git
cd AAIK
# Start from topic 01
cd 01-getting-started && cat README.md
```

## How to Use

1. Follow topics in order (01 → 16)
2. Read `README.md` for the concept explanation
3. Study `commands.md` for all kubectl commands
4. Apply manifests from the `manifests/` folder
5. Experiment, break things, and learn!

## License

This project is licensed under the [MIT License](LICENSE) — free to use, share, and modify with attribution.

## Disclaimer

This repository is an independent educational resource. See [DISCLAIMER.md](DISCLAIMER.md) for full legal notice.

> *"The best way to learn Kubernetes is to run it."*
