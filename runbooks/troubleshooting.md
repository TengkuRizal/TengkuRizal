# Troubleshooting Guide

## Overview

This guide covers common failure scenarios across the homelab DevSecOps pipeline and Kubernetes environment. Each section includes symptoms, diagnosis steps, and remediation commands.

---

## Environment Quick Reference

| Component | IP / Host | Access |
|---|---|---|
| GitLab CE | `10.10.1.101` | Web UI, SSH |
| GitLab Runner | `10.10.1.21` | SSH, `gitlab-runner` CLI |
| K8s Master | `k8master` / `10.10.1.70` | SSH, `kubectl` |
| Wazuh SIEM | `10.10.1.30` | Web UI (`https://10.10.1.30`) |
| Security Onion | `10.10.1.151` | Web UI (run `sudo so-firewall includehost analyst <your-ip>` first) |
| Kali Attacker | `10.10.3.20` | SSH |
| Windows AD | `10.10.2.30` | RDP |

> **Note:** Security Onion web UI access resets on reboot. To avoid repeating this, add the full management subnet: `sudo so-firewall includehost analyst 192.168.0.0/24`

---

## Section 1 — GitLab Pipeline Issues

### 1.1 Pipeline Stuck / Not Triggering

**Symptoms:** Push to repo but no pipeline starts, or pipeline stays in `pending` indefinitely.

**Diagnosis:**
```bash
# On GitLab Runner host (10.10.1.21)
sudo gitlab-runner status
sudo gitlab-runner verify
```

**Remediation:**
```bash
# Restart runner
sudo gitlab-runner restart

# If runner shows as unregistered in GitLab UI, re-register
sudo gitlab-runner register
```

Check GitLab UI: **Settings → CI/CD → Runners** — runner must show green.

---

### 1.2 Gitleaks Stage Fails

**Symptoms:** Pipeline fails at `secret-scan` stage with detected secrets.

**Diagnosis:** Read the pipeline log. Gitleaks will output the file, line, and rule that triggered.

**Remediation:**
```bash
# Remove the secret from code
# Then purge from git history (if already committed)
pip install git-filter-repo
git filter-repo --path <file-with-secret> --invert-paths

# Rotate the exposed credential immediately
# Re-push
git push origin main --force
```

> Do not simply delete the file in a new commit — the secret remains in git history.

---

### 1.3 Semgrep Stage Fails

**Symptoms:** Pipeline fails at `sast` stage with rule violations.

**Diagnosis:** Pipeline log shows rule ID, file, and line number.

**Remediation:**
- Fix the flagged code pattern
- If finding is a false positive, add inline suppression:
  ```python
  result = eval(user_input)  # nosemgrep: dangerous-eval
  ```
- Re-push and verify stage passes

---

### 1.4 Trivy Stage Fails

**Symptoms:** Pipeline fails at `image-scan` stage with CRITICAL CVEs.

**Diagnosis:**
```bash
# Run Trivy locally to see full report
trivy image <your-image>:<tag>
```

**Remediation:**
- Update the base image to a patched version in your `Dockerfile`
- Update the affected OS package:
  ```dockerfile
  RUN apt-get update && apt-get upgrade -y
  ```
- Rebuild and re-push image

---

### 1.5 kubectl Deploy Stage Fails — KUBECONFIG Error

**Symptoms:** Stage fails with `error: no configuration has been provided` or `unauthorized`.

**Diagnosis:** The `KUBECONFIG_B64` variable is missing, empty, or incorrectly encoded.

**Remediation:**
```bash
# On k8master, re-generate the base64 kubeconfig
cat ~/.kube/config | base64 -w 0

# Copy output and update in GitLab:
# Settings → CI/CD → Variables → KUBECONFIG_B64
# Ensure: Masked = ON, Protected = as needed
```

Verify the `.gitlab-ci.yml` decode step:
```yaml
before_script:
  - echo "$KUBECONFIG_B64" | base64 -d > /tmp/kubeconfig
  - export KUBECONFIG=/tmp/kubeconfig
```

---

## Section 2 — Kubernetes Issues

### 2.1 Pod in CrashLoopBackOff

**Symptoms:** `kubectl get pods` shows `CrashLoopBackOff`.

**Diagnosis:**
```bash
kubectl describe pod <pod-name> -n <namespace>
kubectl logs <pod-name> -n <namespace> --previous
```

**Common causes:**
| Cause | Fix |
|---|---|
| Application error on startup | Check logs for exception/panic, fix code |
| Missing environment variable | Add to deployment manifest or ConfigMap |
| Wrong image tag | Correct `image:` field in manifest |
| OOMKilled | Increase `resources.limits.memory` in manifest |

---

### 2.2 Pod in ImagePullBackOff

**Symptoms:** `kubectl get pods` shows `ImagePullBackOff` or `ErrImagePull`.

**Diagnosis:**
```bash
kubectl describe pod <pod-name> -n <namespace>
# Look at Events section at the bottom
```

**Remediation:**
```bash
# Verify image exists in registry
# If using GitLab registry, ensure imagePullSecret is configured
kubectl create secret docker-registry gitlab-registry \
  --docker-server=10.10.1.101:5050 \
  --docker-username=<user> \
  --docker-password=<token> \
  -n <namespace>
```

---

### 2.3 Node NotReady

**Symptoms:** `kubectl get nodes` shows node in `NotReady` state.

**Diagnosis:**
```bash
kubectl describe node <node-name>
# Check Conditions section and Events

# SSH to the affected node and check kubelet
sudo systemctl status kubelet
sudo journalctl -u kubelet -n 50
```

**Remediation:**
```bash
# Restart kubelet
sudo systemctl restart kubelet

# If node is fully unresponsive, restart the VM from Proxmox UI
# After VM restart, kubelet should auto-reconnect within 2–3 minutes
```

---

### 2.4 kubectl Commands Hanging / Timeout

**Symptoms:** `kubectl get pods` hangs or returns `connection refused`.

**Diagnosis:**
```bash
# Check API server on k8master
sudo systemctl status kube-apiserver
# Or if using kubeadm:
kubectl get pods -n kube-system

# Check if etcd is healthy
kubectl get pods -n kube-system | grep etcd
```

**Remediation:**
```bash
# Restart control plane components (kubeadm clusters)
sudo systemctl restart kubelet

# If all control plane pods are down, check disk space first
df -h
# etcd crashes if disk is full
```

---

## Section 3 — Wazuh SIEM Issues

### 3.1 Agent Showing Disconnected

**Symptoms:** Wazuh dashboard shows agent status as `Disconnected`.

**Diagnosis:**
```bash
# On the affected agent host
sudo systemctl status wazuh-agent
sudo tail -50 /var/ossec/logs/ossec.log
```

**Remediation:**
```bash
# Restart agent
sudo systemctl restart wazuh-agent

# Verify manager connectivity
ping 10.10.1.30
telnet 10.10.1.30 1514
```

---

### 3.2 No Alerts Appearing in Dashboard

**Symptoms:** Wazuh dashboard shows agents as active but no alerts visible.

**Diagnosis:**
- Check the time range selector in the dashboard (default may be last 15 min)
- Verify OpenSearch is running on the Wazuh host:
  ```bash
  sudo systemctl status wazuh-indexer
  ```

**Remediation:**
```bash
# Restart indexer if stopped
sudo systemctl restart wazuh-indexer
sudo systemctl restart wazuh-dashboard
```

---

### 3.3 wazuh_triage.py Script Not Running

**Symptoms:** No triage output files generated, cron job appears silent.

**Diagnosis:**
```bash
# Check cron logs
grep CRON /var/log/syslog | tail -20

# Run script manually to see errors
python3 /path/to/wazuh_triage.py
```

**Remediation:**
```bash
# Verify OpenSearch is reachable from script host
curl -k -u admin:<password> https://10.10.1.30:9200/_cluster/health

# Check crontab entry
crontab -l

# Verify Python dependencies
pip3 list | grep -E "requests|opensearch"
```

---

## Section 4 — Security Onion Issues

### 4.1 Cannot Access Security Onion Web UI

**Symptoms:** Browser times out or refuses connection to Security Onion dashboard.

**Cause:** Security Onion firewall resets analyst IP allowlist on reboot.

**Remediation:**
```bash
# SSH to Security Onion host (10.10.1.151)
sudo so-firewall includehost analyst 192.168.0.0/24

# Verify
sudo so-firewall list | grep analyst
```

> Add the full `/24` subnet rather than a single IP to avoid repeating this after every reboot.

---

### 4.2 Zeek Not Capturing Traffic

**Symptoms:** Security Onion shows no network events or Zeek logs are empty.

**Diagnosis:**
```bash
# Verify SPAN port traffic is reaching Security Onion
sudo tcpdump -i <capture-interface> -c 20
```

**Remediation:**
- Verify Cisco 2960 SPAN port configuration:
  ```
  # On Cisco 2960 (via console or SSH)
  show monitor session 1
  # Should show source: Fa0/6 (trunk to pfSense), destination: Fa0/8 (to Security Onion)
  ```
- Ensure Security Onion capture interface matches the NIC connected to Fa0/8

---

## Quick Diagnostic Cheatsheet

```bash
# Full cluster health check
kubectl get nodes && kubectl get pods -A

# Check all failing pods
kubectl get pods -A | grep -v Running | grep -v Completed

# Wazuh agent status (all agents)
# Login to https://10.10.1.30 → Agents

# GitLab runner health
ssh user@10.10.1.21 "sudo gitlab-runner status && sudo gitlab-runner verify"

# Security Onion access fix
ssh user@10.10.1.151 "sudo so-firewall includehost analyst 192.168.0.0/24"
```
