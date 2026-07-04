# YAML Quick Reference

## Pod Skeleton
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: mypod
  namespace: default
  labels:
    app: myapp
  annotations:
    description: "example pod"
spec:
  containers:
  - name: main
    image: nginx:alpine
    ports:
    - containerPort: 80
    env:
    - name: ENV_VAR
      value: "hello"
    - name: FROM_CONFIGMAP
      valueFrom:
        configMapKeyRef:
          name: myconfigmap
          key: mykey
    - name: FROM_SECRET
      valueFrom:
        secretKeyRef:
          name: mysecret
          key: password
    resources:
      requests:
        cpu: "100m"
        memory: "64Mi"
      limits:
        cpu: "500m"
        memory: "256Mi"
    readinessProbe:
      httpGet:
        path: /healthz
        port: 80
      initialDelaySeconds: 5
      periodSeconds: 5
    livenessProbe:
      httpGet:
        path: /healthz
        port: 80
      initialDelaySeconds: 10
      periodSeconds: 10
    volumeMounts:
    - name: config-vol
      mountPath: /etc/config
  initContainers:
  - name: init
    image: busybox
    command: ["/bin/sh", "-c", "echo init done"]
  volumes:
  - name: config-vol
    configMap:
      name: myconfigmap
  nodeSelector:
    disk: ssd
  tolerations:
  - key: "dedicated"
    operator: "Equal"
    value: "gpu"
    effect: "NoSchedule"
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: topology.kubernetes.io/zone
            operator: In
            values: [us-east-1a, us-east-1b]
  serviceAccountName: mysa
  restartPolicy: Always    # Always | OnFailure | Never
```

## Probe Types
```yaml
# HTTP
httpGet:
  path: /health
  port: 8080
  httpHeaders:
  - name: X-Custom-Header
    value: Awesome

# TCP
tcpSocket:
  port: 3306

# Exec
exec:
  command: ["/bin/sh", "-c", "cat /tmp/healthy"]

# gRPC
grpc:
  port: 8080
```

## Container Security Context
```yaml
securityContext:
  runAsUser: 1000
  runAsGroup: 3000
  runAsNonRoot: true
  allowPrivilegeEscalation: false
  readOnlyRootFilesystem: true
  capabilities:
    drop: ["ALL"]
    add: ["NET_BIND_SERVICE"]
```

## Resource Units
```
CPU:    1 = 1 core = 1000m (millicores)
        100m = 0.1 core
Memory: 1Ki = 1024 bytes
        1Mi = 1024 Ki
        1Gi = 1024 Mi
```
