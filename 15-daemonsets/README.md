# 15 — DaemonSets

## What is a DaemonSet?

A **DaemonSet** ensures that **exactly one pod runs on every node** (or a subset of nodes matching a selector).

```
Node 1: [log-agent pod]
Node 2: [log-agent pod]
Node 3: [log-agent pod]
→ When Node 4 is added: [log-agent pod] is automatically created
→ When Node 2 is removed: its pod is automatically deleted
```

## Common Use Cases

| Use Case | Examples |
|----------|---------|
| Log collection | Fluentd, Filebeat, Logstash |
| Monitoring/metrics | Prometheus node-exporter, Datadog agent |
| Network plugins | Calico, Flannel, Cilium (CNI) |
| Storage drivers | Ceph, GlusterFS agents |
| Security agents | Falco, runtime security tools |

## Node Selector

To run only on specific nodes:
```yaml
spec:
  template:
    spec:
      nodeSelector:
        gpu: cuda                  # only on GPU nodes
```

## System DaemonSets

Kubernetes itself uses DaemonSets for core components:
- `kube-proxy` — handles Service routing on every node
- `kindnet`/`calico` — CNI network plugin on every node

## Next Steps
→ [16 — Jobs & CronJobs](../16-jobs-cronjobs/)
