# 18 — Extending Kubernetes: CRDs & Operators

## Custom Resource Definitions (CRDs)

A **CRD** lets you define your own Kubernetes resource types. Once created, you can `kubectl apply` your custom objects just like Pods or Deployments.

```
Standard K8s:    kubectl get pods
Custom resource: kubectl get aaikdatabases      ← your own type!
```

### Flow
```
1. Define CRD (CustomResourceDefinition)
2. Create Custom Resources (CR) — instances of your type
3. Controller watches CRs and reconciles desired state
```

---

## Operators

An **Operator** = CRD + Controller. It encodes **operational knowledge** about a specific application.

```
Human operator knows:
  "When database replica count changes → run a specific rebalance procedure"

Kubernetes Operator automates:
  Watch AaikDatabase CR → detect replica change → run rebalance → update status
```

### Why Operators?

| Regular K8s resources | Operators |
|----------------------|-----------|
| Stateless apps, standard patterns | Stateful, complex apps |
| Deployment, Service, etc. | Databases, message queues, ML platforms |
| Generic reconciliation | App-specific operational logic |

### Popular Operators
- **Prometheus Operator** — manages Prometheus + Alertmanager
- **Cert-Manager** — automates TLS certificate provisioning
- **Strimzi** — manages Apache Kafka clusters
- **CloudNativePG** — manages PostgreSQL clusters

---

## Admission Webhooks

Extend Kubernetes API validation and mutation:

| Type | When | Use Case |
|------|------|----------|
| **MutatingWebhook** | Before object stored | Inject sidecars, set defaults |
| **ValidatingWebhook** | Before object stored | Enforce policies, reject invalid configs |

---

## API Aggregation

Register your own API server alongside the core K8s API. Users interact with it via standard `kubectl` commands. Used by: Metrics Server, custom APIs.
