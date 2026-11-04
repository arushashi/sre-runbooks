# Runbook: High error rate (5xx)

**Alerts:** `CheckoutErrorBudgetFastBurn`, `CheckoutErrorBudgetSlowBurn`

## 1. Assess impact
- Which service, endpoints and status codes? Is it all users or a segment (region, tenant, version)?
- Check the SLO dashboard: how fast is the budget burning?

## 2. Check for recent changes (most common cause)
```bash
kubectl rollout history deployment/<name> -n <ns>
kubectl get events -n <ns> --sort-by=.lastTimestamp | tail -20
```
Deploy, config or feature-flag change in the last hour? **Roll back first:**
```bash
kubectl rollout undo deployment/<name> -n <ns>
```

## 3. Check dependencies
- Database connections / saturation
- Downstream APIs returning errors or timing out
- Cache availability

## 4. Check resources
```bash
kubectl top pods -n <ns>
kubectl describe pod <pod> -n <ns>     # OOMKilled? Probe failures?
```

## 5. Mitigate
| Situation | Action |
|---|---|
| Bad release | Roll back |
| Overload | Scale out, enable rate limiting |
| Dependency down | Enable fallback / circuit breaker |

## 6. Close out
Confirm the error ratio is back under budget for 30 minutes, then write a
[postmortem](../incident-management/postmortem-template.md) for SEV1/SEV2.
