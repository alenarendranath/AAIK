# 16 — Jobs & CronJobs

## Jobs

A **Job** creates one or more pods and ensures they **run to completion** (exit 0). Unlike Deployments, pods are not restarted indefinitely — they run until done.

```
Job: process-data
├── Pod 1: runs → completes ✅
├── Pod 2: runs → fails → retries → completes ✅
└── Pod 3: runs → completes ✅
→ Job is Done!
```

## Job Patterns

| Pattern | Config | Use Case |
|---------|--------|----------|
| **Single** | `completions: 1`, `parallelism: 1` | One-off task |
| **Sequential** | `completions: N`, `parallelism: 1` | Process N items one by one |
| **Parallel** | `completions: N`, `parallelism: M` | Process N items, M at a time |
| **Indexed** | `completionMode: Indexed` | Sharded work (each pod gets unique index) |

## Job Key Fields

| Field | Default | Meaning |
|-------|---------|---------|
| `completions` | 1 | Total successful runs required |
| `parallelism` | 1 | Max concurrent pods |
| `backoffLimit` | 6 | Max retries before Job fails |
| `activeDeadlineSeconds` | — | Hard timeout for the entire Job |
| `ttlSecondsAfterFinished` | — | Auto-delete Job N seconds after completion |

## CronJobs

A **CronJob** creates a Job on a **schedule** (standard cron format):

```
┌───── minute (0-59)
│ ┌─── hour (0-23)
│ │ ┌─ day of month (1-31)
│ │ │ ┌ month (1-12)
│ │ │ │ ┌ day of week (0-6, 0=Sunday)
│ │ │ │ │
* * * * *

Examples:
0 * * * *      → every hour at :00
0 9 * * 1-5   → 9am on weekdays
*/5 * * * *   → every 5 minutes
0 0 1 * *     → midnight on 1st of every month
```

## ConcurrencyPolicy

| Policy | Behavior |
|--------|----------|
| `Allow` (default) | Multiple Jobs can run simultaneously |
| `Forbid` | Skip new Job if previous is still running |
| `Replace` | Cancel previous Job, start new one |
