# sre-runbooks

Practical, generic SRE runbooks and incident-management templates.
All content is example material, free of employer or customer data.

## Contents

**Runbooks** (`runbooks/`)
- [High error rate](runbooks/high-error-rate.md)
- [High latency](runbooks/high-latency.md)
- [Kubernetes pod CrashLoopBackOff](runbooks/pod-crashloop.md)
- [Disk space full](runbooks/disk-full.md)

**Incident management** (`incident-management/`)
- [Severity levels](incident-management/severity-levels.md)
- [Postmortem template](incident-management/postmortem-template.md)

## Runbook principles

1. **Start with impact**: confirm who is affected before diving into causes.
2. **Mitigate first, root-cause later**: rollback, scale or fail over before debugging.
3. **Every step is a command or a decision**: no vague advice.
4. **Link from alerts**: each alert's `runbook` annotation points at one of these files.

## License

MIT
