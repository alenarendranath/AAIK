# Troubleshooting Flowchart

## Pod Not Running

```
kubectl get pods → what STATUS?

├── Pending
│   └── kubectl describe pod → Events?
│       ├── "Insufficient cpu/memory" → reduce requests or add nodes
│       ├── "didn't match node selector" → fix labels or nodeSelector
│       ├── "had untolerated taint" → add toleration or remove taint
│       └── "persistentvolumeclaim not found" → check PVC status
│
├── CrashLoopBackOff
│   └── kubectl logs <pod> --previous → read the error
│       ├── Exit 1:   app error → fix code or config
│       ├── Exit 127: command not found → fix image/entrypoint
│       ├── Exit 137: OOMKilled → increase memory limit
│       └── Exit 0:   process exits immediately → add long-running command
│
├── ImagePullBackOff / ErrImagePull
│   └── kubectl describe pod → Events?
│       ├── "not found" / "manifest unknown" → check image name/tag
│       ├── "unauthorized" → add imagePullSecret
│       └── "toomanyrequests" → authenticate or use a mirror
│
├── OOMKilled
│   └── kubectl top pods → check memory usage
│       └── Increase limits.memory in manifest
│
└── Running but not working
    └── kubectl logs <pod> → check app logs
        kubectl exec -it <pod> -- /bin/sh → debug inside
```

## Service Not Reachable

```
kubectl get endpoints <svc> → has IPs?

├── <none> → label mismatch
│   └── kubectl describe svc → Selector
│       kubectl get pods --show-labels
│       → fix: match pod labels to service selector
│
└── Has IPs → traffic not reaching?
    ├── kubectl port-forward svc/<name> 8080:80 → test locally
    ├── Wrong targetPort → match to containerPort
    ├── Pod not Ready → check readinessProbe
    └── NetworkPolicy blocking → check kubectl get networkpolicies
```

## PVC Stuck in Pending

```
kubectl describe pvc → Events?

├── "no persistent volumes available" → no PV matches; check storageClass
├── "storageclass not found" → kubectl get storageclass; create one
└── "waiting for first consumer" → PVC is created lazily; schedule a pod to use it
```

## Node Issues

```
kubectl get nodes → STATUS?

├── NotReady
│   └── kubectl describe node → Conditions
│       └── ssh node → check kubelet: systemctl status kubelet
│
└── SchedulingDisabled (SchedulingDisabled / cordoned)
    └── kubectl uncordon <node>
```
