# Deployment Runbook

## Overview

This runbook documents the end-to-end deployment process for workloads in the homelab DevSecOps environment, covering GitLab CI/CD pipeline execution, security gate validation, and Kubernetes deployment verification.

---

## Environment Reference

| Component | Hostname / IP | Role |
|---|---|---|
| GitLab CE | `10.10.1.101` | Source control & CI/CD orchestrator |
| GitLab Runner | `10.10.1.21` | Pipeline executor (shell executor) |
| K8s Master | `k8master` / `10.10.1.70` | Kubernetes control plane |
| Wazuh SIEM | `10.10.1.30` | Security event monitoring |
| Security Onion | `10.10.1.151` | Network traffic analysis |

---

## Pre-Deployment Checklist

Before triggering a pipeline, verify the following:

- [ ] All code changes are committed and pushed to the target branch
- [ ] GitLab Runner is online and healthy
- [ ] Kubernetes cluster nodes are in `Ready` state
- [ ] `KUBECONFIG_B64` CI/CD variable is present in GitLab project settings
- [ ] Wazuh agents on K8s nodes are active

```bash
# Verify K8s node status
kubectl get nodes

# Verify GitLab Runner status (on runner host 10.10.1.21)
sudo gitlab-runner status

# Verify Wazuh agents
# Login to Wazuh dashboard at https://10.10.1.30
```

---

## Pipeline Stages

The CI/CD pipeline executes four sequential security stages before deployment. All stages must pass for deployment to proceed.

### Stage 1 — Secret Scanning (Gitleaks)

Scans the repository for hardcoded secrets, API keys, and credentials.

- **Tool:** Gitleaks
- **Fail condition:** Any detected secret causes pipeline to abort
- **Action if failed:** Remove the secret, rotate the credential, force-push with `git filter-repo`

### Stage 2 — Static Analysis (Semgrep)

Performs SAST scanning against the application source code.

- **Tool:** Semgrep
- **Ruleset:** `p/default` or project-defined rules
- **Fail condition:** High-severity findings block deployment
- **Action if failed:** Review finding in pipeline logs, remediate in code, re-push

### Stage 3 — Container Image Scanning (Trivy)

Scans the built container image for OS and library vulnerabilities.

- **Tool:** Trivy
- **Fail condition:** CRITICAL severity CVEs block deployment
- **Action if failed:** Update base image or patch affected package, rebuild

### Stage 4 — Deploy to Kubernetes

Applies Kubernetes manifests to the cluster using `kubectl`.

- **Auth method:** `KUBECONFIG_B64` decoded per-job in shell executor
- **Namespace:** As defined in manifest (e.g., `default` or `app-namespace`)

---

## Deployment Steps

### 1. Trigger Pipeline

Push to the target branch or trigger manually via GitLab UI.

```bash
git add .
git commit -m "feat: describe your change"
git push origin main
```

Or via GitLab UI: **CI/CD → Pipelines → Run Pipeline**

### 2. Monitor Pipeline Execution

Navigate to: **GitLab → Project → CI/CD → Pipelines**

Monitor each stage in real time. Expected duration per stage:

| Stage | Expected Duration |
|---|---|
| Gitleaks | ~30s |
| Semgrep | ~60–90s |
| Trivy | ~60–120s |
| kubectl deploy | ~30s |

### 3. Verify Deployment on Kubernetes

After pipeline completes successfully, verify from `k8master`:

```bash
# Check pods are running
kubectl get pods -n <namespace> -o wide

# Check deployment rollout status
kubectl rollout status deployment/<deployment-name> -n <namespace>

# Check recent events for errors
kubectl get events -n <namespace> --sort-by='.lastTimestamp' | tail -20
```

### 4. Validate Application

Confirm the deployed workload is serving correctly:

```bash
# Port-forward for local validation (if no ingress)
kubectl port-forward svc/<service-name> 8080:80 -n <namespace>

# Or check service endpoint
kubectl get svc -n <namespace>
```

### 5. Confirm in Wazuh

Check Wazuh dashboard (`https://10.10.1.30`) for any anomalous alerts triggered during deployment. Expected alerts during normal deployment: none critical.

---

## Post-Deployment Verification

| Check | Command | Expected Result |
|---|---|---|
| Pods running | `kubectl get pods -n <namespace>` | All pods in `Running` state |
| No crash loops | `kubectl get pods -n <namespace>` | RESTARTS count = 0 |
| Service reachable | `curl http://<service-ip>:<port>` | HTTP 200 |
| No Wazuh critical alerts | Wazuh Dashboard | No rule level 12+ alerts |

---

## Deployment Sign-Off

Record the following after each successful deployment:

| Field | Value |
|---|---|
| Deployment date | |
| Commit SHA | |
| Pipeline ID | |
| Deployed by | |
| Verification completed | Yes / No |
| Issues encountered | None / Describe |
