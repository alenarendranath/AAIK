# Jobs & CronJobs — kubectl Commands

## Jobs
```bash
kubectl get jobs                                  # list jobs
kubectl get job <name> -o yaml
kubectl describe job <name>
kubectl delete job <name>
kubectl delete job <name> --cascade=orphan        # keep pods

# Watch job pods
kubectl get pods -l job-name=<name> -w
kubectl logs job/<name>                           # logs from a job pod
kubectl logs job/<name> --all-containers --prefix # all containers

# Suspend/resume
kubectl patch job <name> -p '{"spec":{"suspend":true}}'
kubectl patch job <name> -p '{"spec":{"suspend":false}}'
```

## CronJobs
```bash
kubectl get cronjobs                              # list CronJobs
kubectl get cj                                    # shorthand
kubectl get cj <name> -o yaml
kubectl describe cj <name>
kubectl delete cj <name>

# Suspend CronJob
kubectl patch cj <name> -p '{"spec":{"suspend":true}}'
kubectl patch cj <name> -p '{"spec":{"suspend":false}}'

# Manually trigger a CronJob
kubectl create job --from=cronjob/<name> <manual-job-name>

# View jobs created by a CronJob
kubectl get jobs -l app=<cronjob-app-label>
```
