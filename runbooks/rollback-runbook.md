# Rollback Runbook

## Overview

This runbook documents the procedure to safely roll back a failed or problematic deployment in the homelab Kubernetes environment. Rollback should be initiated immediately upon detection of post-deployment degradation.

---

## When to Rollback

Initiate rollback if any of the following conditions are observed after deployment:

- Pods in `CrashLoopBackOff` or `Error` state
- Service returning non-200 responses or timing out
- Wazuh raising critical alerts (rule level 12+) correlated to the new deployment
- Kubernetes events showing `OOMKilled`, `ImagePullBackOff`, or `Readiness probe failed`
- Application logs showing repeated exceptions or panics

---

## Environment Reference

| Component | IP | Notes |
|---|---|---|
| K8s Master | `10.10.1.70` | All kubectl commands run here |
| GitLab CE | `10.10.1.101` | Re-trigger pipeline if needed |
| Wazuh SIEM | `10.10.1.30` | Monitor for security events during rollback |

---

## Rollback Procedure

### Step 1 — Assess the Situation

Before rolling back, collect information to understand the failure scope.

```bash
# Check pod status
kubectl get pods -n <namespace> -o wide

# Check recent events
kubectl get events -n <namespace> --sort-by='.lastTimestamp' | tail -30

# Check deployment rollout status
kubectl rollout status deployment/<deployment-name> -n <namespace>

# Check pod logs for the failing pod
kubectl logs <pod-name> -n <namespace> --previous
```

### Step 2 — Immediate Rollback via kubectl

Kubernetes natively tracks deployment revision history. Roll back to the previous known-good revision:

```bash
# View rollout history
kubectl rollout history deployment/<deployment-name> -n <namespace>

# Rollback to previous revision
kubectl rollout undo deployment/<deployment-name> -n <namespace>

# Rollback to a specific revision (if previous is also bad)
kubectl rollout undo deployment/<deployment-name> -n <namespace> --to-revision=<revision-number>
```

### Step 3 — Verify Rollback Success

```bash
# Confirm rollout completed
kubectl rollout status deployment/<deployment-name> -n <namespace>

# Confirm pods are healthy
kubectl get pods -n <namespace>

# Confirm correct image is running (check revision)
kubectl describe deployment/<deployment-name> -n <namespace> | grep Image
```

Expected output: all pods in `Running` state, RESTARTS = 0.

### Step 4 — Validate Service Recovery

```bash
# Test service endpoint
curl http://<service-ip>:<port>/health

# Or port-forward and test
kubectl port-forward svc/<service-name> 8080:80 -n <namespace>
curl http://localhost:8080
```

### Step 5 — Check Wazuh for Security Events

After rollback, verify in Wazuh dashboard (`https://10.10.1.30`) that no residual security alerts are active. Pay attention to:

- Lateral movement indicators from K8s nodes
- Anomalous network connections from pods
- File integrity alerts on K8s master

---

## Rollback via GitLab (Pipeline-Based)

If the issue is in the pipeline config or manifests rather than the running image:

1. Revert the commit in GitLab:
   ```bash
   git revert <bad-commit-sha>
   git push origin main
   ```
2. Monitor the new pipeline — it will re-deploy the reverted manifest
3. Verify deployment as per Step 3 above

---

## Escalation Path

If `kubectl rollout undo` does not resolve the issue:

| Scenario | Action |
|---|---|
| Image corrupted or missing | Re-build and re-push image via pipeline |
| ConfigMap/Secret misconfigured | `kubectl edit configmap` or re-apply correct manifest |
| Node not schedulable | `kubectl describe node <node>`, check taints and resources |
| Persistent volume issue | Check PVC status: `kubectl get pvc -n <namespace>` |
| Cluster-wide issue | Check control plane: `kubectl get componentstatuses` |

---

## Post-Rollback Checklist

- [ ] Service is responding correctly
- [ ] All pods in `Running` state with 0 restarts
- [ ] Wazuh shows no active critical alerts
- [ ] Root cause of failed deployment identified
- [ ] GitLab issue or note created to track the fix
- [ ] Rollback event documented below

---

## Rollback Log

| Date | Deployment Rolled Back | Revision Target | Root Cause | Resolved By |
|---|---|---|---|---|
| | | | | |
