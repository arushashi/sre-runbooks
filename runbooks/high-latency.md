# Runbook: High latency

**Alert:** `HighP99Latency`

## 1. Scope it
- p50 vs p99: is everyone slow, or only the tail?
- Which endpoint, region or version? Compare against the same time yesterday.

## 2. Find where the time goes
Open a slow trace and look for the longest span:
- DB query slow -> check slow-query log, missing index, lock contention
- Downstream call slow -> follow the runbook of that service
- Time in the service itself -> check CPU throttling and GC pauses

## 3. Check saturation
```bash
kubectl top pods -n <ns>
kubectl get hpa -n <ns>
```
CPU near its limit means throttling: scale out or raise limits.

## 4. Check the database (if involved)
```sql
SELECT pid, now() - query_start AS runtime, state, left(query, 80)
FROM pg_stat_activity
WHERE state <> 'idle'
ORDER BY runtime DESC
LIMIT 10;
```

## 5. Mitigate
Scale out, shed load, roll back a recent release, or disable the expensive feature behind a flag.
