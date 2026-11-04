# Runbook: Disk space full

## 1. Find the filesystem
```bash
df -h
df -i            # inode exhaustion looks like a full disk too
```

## 2. Find what is using it
```bash
du -xh / --max-depth=2 2>/dev/null | sort -rh | head -20
```
Common culprits: logs, container images, core dumps, database WAL, temp files.

## 3. Safe cleanups
```bash
journalctl --vacuum-size=500M
docker system prune -f                 # unused containers/images
find /var/log -name "*.gz" -mtime +14 -delete
```
Deleted files still held open by a process keep using space. Find them with
`lsof +L1`, then restart that process.

## 4. Kubernetes nodes
```bash
kubectl describe node <node> | grep -i -A5 conditions
```
`DiskPressure=True` evicts pods. Clean up as above or cordon and drain the node.

## 5. Prevent recurrence
- Alert at 80% usage, not 100%
- Log rotation and retention limits
- Size volumes with growth in mind and enable auto-expansion
