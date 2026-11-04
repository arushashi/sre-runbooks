# Runbook: Pod CrashLoopBackOff

**Alert:** `PodCrashLooping`

## 1. Identify why it exits
```bash
kubectl get pod <pod> -n <ns>
kubectl describe pod <pod> -n <ns>          # see "Last State" and Events
kubectl logs <pod> -n <ns> --previous       # logs from the crashed container
```

## 2. Match the symptom

| Last State / Reason | Likely cause | Fix |
|---|---|---|
| `OOMKilled` (exit 137) | Memory limit too low or leak | Raise limit, investigate leak |
| `Error`, exit 1 | App crash on start | Read `--previous` logs; check config and secrets |
| Liveness probe failed | Probe too strict or app slow to start | Add `startupProbe`, tune thresholds |
| `ImagePullBackOff` | Bad tag or registry credentials | Check image name and `imagePullSecrets` |
| `CreateContainerConfigError` | Missing ConfigMap or Secret | Create the missing object |

## 3. Verify config references
```bash
kubectl get configmap,secret -n <ns>
kubectl get deployment <name> -n <ns> -o yaml | grep -A3 envFrom
```

## 4. Recover
Fix the cause, then `kubectl rollout restart deployment/<name> -n <ns>`, or roll back to the last working revision.
