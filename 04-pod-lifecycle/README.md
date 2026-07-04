# 04 — Pod Lifecycle & Container Health

## Pod Phases

| Phase | Meaning |
|-------|---------|
| `Pending` | Pod accepted, containers not yet started (scheduling or pulling image) |
| `Running` | Pod bound to node, at least one container running |
| `Succeeded` | All containers terminated successfully (exit 0) |
| `Failed` | All containers terminated, at least one failed |
| `Unknown` | Pod state cannot be determined (node unreachable) |

## Pod Conditions

| Condition | Meaning |
|-----------|---------|
| `PodScheduled` | Pod has been scheduled to a node |
| `Initialized` | All init containers completed successfully |
| `ContainersReady` | All containers are ready |
| `Ready` | Pod is ready to serve traffic (used by Services) |

## Container States

| State | Meaning |
|-------|---------|
| `Waiting` | Not yet running (pulling image, waiting for resources) |
| `Running` | Executing |
| `Terminated` | Finished (success or failure) |

## Restart Policies

| Policy | Behavior |
|--------|----------|
| `Always` | Always restart (default; for long-running apps) |
| `OnFailure` | Restart only on non-zero exit (for batch jobs) |
| `Never` | Never restart |

Restarts use **exponential backoff**: 10s → 20s → 40s → ... → 5m

## Health Probes

### Liveness Probe
> "Is the container still alive? Should we restart it?"

Failure → container is **killed and restarted**.

### Readiness Probe
> "Is the container ready to receive traffic?"

Failure → container is **removed from Service endpoints** (no restart).

### Startup Probe
> "Has the app finished starting up?"

Useful for slow-starting apps. Liveness/readiness probes are **paused** until startup probe succeeds.

## Probe Types

```yaml
# HTTP GET — success if 2xx/3xx response
livenessProbe:
  httpGet:
    path: /healthz
    port: 80
  initialDelaySeconds: 10    # wait before first probe
  periodSeconds: 5           # probe every 5s
  timeoutSeconds: 3          # fail if no response in 3s
  failureThreshold: 3        # fail after 3 consecutive failures
  successThreshold: 1        # pass after 1 success

# TCP Socket — success if port accepts connection
livenessProbe:
  tcpSocket:
    port: 80

# Exec — success if command exits 0
livenessProbe:
  exec:
    command: ["cat", "/tmp/healthy"]
```

## Lifecycle Hooks

### postStart Hook
- Runs **immediately after** the container starts
- Runs **in parallel** with the main process
- Container stays in `Waiting` state until hook completes
- Hook failure → container is restarted

### preStop Hook
- Runs **before** the container receives SIGTERM
- Container waits for hook to finish before proceeding
- Use for graceful shutdown (e.g., drain connections, deregister)

## Termination Sequence

```
1. kubectl delete pod (or pod evicted)
2. preStop hook executes
3. SIGTERM sent to main process
4. Wait for terminationGracePeriodSeconds (default: 30s)
5. SIGKILL sent (force kill)
```

## Next Steps
→ [05 — Namespaces, Labels & Annotations](../05-namespaces-labels/)
