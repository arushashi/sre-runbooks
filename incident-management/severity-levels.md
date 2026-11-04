# Incident severity levels

| Level | Definition | Example | Response |
|---|---|---|---|
| **SEV1** | Critical customer-facing outage or data loss | Checkout down for all users | Page on-call immediately, incident commander assigned, updates every 15 min |
| **SEV2** | Major degradation affecting many users | p99 latency 10x normal, partial outage | Page on-call, updates every 30 min |
| **SEV3** | Minor impact or workaround exists | One non-critical feature broken | Handle in business hours, ticket |
| **SEV4** | No customer impact | Alert noise, tech debt | Backlog |

## Roles (SEV1/SEV2)
- **Incident Commander:** coordinates, makes decisions, owns the timeline
- **Operations lead:** hands-on investigation and mitigation
- **Communications lead:** status page and stakeholder updates
- **Scribe:** records the timeline for the postmortem

## Rules of thumb
- If unsure between two levels, pick the higher one and downgrade later.
- Mitigate first; investigate root cause after service is restored.
