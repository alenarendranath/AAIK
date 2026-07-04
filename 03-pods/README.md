# 03 — Pods

## What is a Pod?

A **Pod** is the smallest deployable unit in Kubernetes. It wraps one or more containers that:
- Share the same **network namespace** (same IP, same ports)
- Share the same **IPC namespace**
- Can share **volumes**

Think of a Pod as a logical host for your containers.

```
Pod: aaik-pod (IP: 10.1.2.3)
├── Container: aaik (port 80)
└── Container: log-shipper (no port)
     Both share the same IP and can talk via localhost
```

## When to Use Multiple Containers in a Pod?

Put containers in the **same pod** when they:
- Must run on the same node
- Share storage
- Communicate via localhost (tight coupling)

Keep containers in **separate pods** when they:
- Scale independently
- Are developed by different teams
- Have different lifecycles

## Pod Patterns

### 1. Single Container Pod (Most Common)
One app per pod — the standard approach.

### 2. Sidecar Pattern
Main app container + helper container (e.g., log shipper, proxy, config watcher).

### 3. Init Container Pattern
One or more containers that run to completion **before** the main containers start.
Use for: DB migrations, downloading config, waiting for dependencies.

### 4. Native Sidecar (K8s 1.29+)
Init container with `restartPolicy: Always` — starts before main containers but stays running.

## Pod Spec Key Fields

```yaml
spec:
  restartPolicy: Always       # Always | OnFailure | Never
  terminationGracePeriodSeconds: 30
  imagePullSecrets:
  - name: my-registry-secret
  initContainers: []          # run before main containers
  containers:
  - name: app
    image: nginx:alpine
    imagePullPolicy: IfNotPresent  # Always | Never | IfNotPresent
    ports:
    - containerPort: 80
    env:
    - name: ENV_VAR
      value: "hello"
    resources:
      requests:
        cpu: "100m"
        memory: "128Mi"
      limits:
        cpu: "500m"
        memory: "256Mi"
    volumeMounts:
    - name: data
      mountPath: /data
  volumes:
  - name: data
    emptyDir: {}
```

## Next Steps
→ [04 — Pod Lifecycle & Health](../04-pod-lifecycle/)
