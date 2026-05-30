# Evidence

## Overview

This directory contains sanitized evidence captured from the homelab DevSecOps environment.

The purpose of this evidence is to demonstrate that the environment is not only documented, but also operated, validated, monitored, and reviewed using production-style DevOps/SRE practices.

Evidence in this directory supports:

- CI/CD execution
- Kubernetes workload validation
- Monitoring and alerting
- Incident tracking
- Operational troubleshooting
- Runbook-based validation
- Portfolio and interview discussion

> Note: All screenshots and outputs in this repository should be sanitized before being committed. Do not include passwords, tokens, private keys, kubeconfig files, customer data, or sensitive internal information.

---

## Evidence Structure

```text
evidence/
├── README.md
└── screenshots/
    ├── alertmanager-alert.png
    ├── github-actions-passing.png
    ├── github-incident-issue.png
    ├── kubernetes-nodes-ready.png
    ├── kubernetes-pods-running.png
    └── prometheus-alert-firing.png
```

---

## Phase 1 Baseline Evidence

Phase 1 evidence captures the current healthy baseline of the homelab DevSecOps/SRE platform.

| Evidence Folder | Description |
|---|---|
| [Phase 1 Baseline Validation](phase-1/README.md) | Kubernetes, monitoring, Prometheus, Alertmanager, and Git baseline validation evidence |

---

## Screenshot Index

| Evidence | Description | What It Demonstrates |
|---|---|---|
| `github-actions-passing.png` | GitHub Actions workflow completed successfully | CI/CD automation, pipeline execution, and delivery validation |
| `kubernetes-nodes-ready.png` | Kubernetes nodes are in Ready state | Cluster health and node-level operational readiness |
| `kubernetes-pods-running.png` | Kubernetes workloads are running successfully | Workload deployment, pod readiness, and runtime validation |
| `prometheus-alert-firing.png` | Prometheus alert rule is firing | Monitoring rule validation and alert condition detection |
| `alertmanager-alert.png` | Alertmanager is receiving and displaying active alerts | Alert routing and observability workflow |
| `github-incident-issue.png` | Incident tracking documented using GitHub Issues | Incident management, documentation, and operational follow-up |

---

## Evidence Categories

### 1. CI/CD Evidence

CI/CD evidence shows that changes are validated and delivered through an automated pipeline.

Relevant screenshot:

```text
screenshots/github-actions-passing.png
```

This demonstrates:

- Pipeline execution
- Automated validation
- Repeatable delivery process
- Git-based workflow
- Deployment confidence

---

### 2. Kubernetes Evidence

Kubernetes evidence shows that the cluster and workloads are operational.

Relevant screenshots:

```text
screenshots/kubernetes-nodes-ready.png
screenshots/kubernetes-pods-running.png
```

This demonstrates:

- Kubernetes node readiness
- Pod deployment status
- Workload validation
- Runtime health checks
- Operational visibility

---

### 3. Monitoring and Alerting Evidence

Monitoring evidence shows that the environment is observable and alert-capable.

Relevant screenshots:

```text
screenshots/prometheus-alert-firing.png
screenshots/alertmanager-alert.png
```

This demonstrates:

- Prometheus alert rule validation
- Alert firing behavior
- Alertmanager routing and visibility
- Monitoring-to-alerting workflow
- SRE-style operational readiness

---

### 4. Incident Management Evidence

Incident evidence shows that operational issues are tracked and documented.

Relevant screenshot:

```text
screenshots/github-incident-issue.png
```

This demonstrates:

- Incident tracking
- Issue-based documentation
- Follow-up workflow
- Operational accountability
- Post-incident learning culture

---

## Evidence Collection Guidelines

When collecting evidence, capture outputs that prove operational state.

Recommended evidence examples:

```bash
kubectl get nodes -o wide
kubectl get pods -A -o wide
kubectl get deployment -A
kubectl get svc -A
kubectl get events -A --sort-by=.metadata.creationTimestamp | tail -50
kubectl rollout status deployment/DEPLOYMENT_NAME -n NAMESPACE
curl -i http://APPLICATION_ENDPOINT/health
git log --oneline --max-count=10
```

For monitoring:

```bash
kubectl get pods -n monitoring
kubectl get svc -n monitoring
kubectl top nodes
kubectl top pods -A
```

For pipeline validation:

```bash
git status
git log --oneline --max-count=5
```

---

## Sanitization Rules

Before committing evidence, remove or mask:

- Passwords
- Tokens
- API keys
- Private keys
- Secret values
- Full kubeconfig files
- Real customer data
- Sensitive hostnames
- Public IP addresses if exposure is not intentional
- Internal documents not meant for public sharing

Private homelab RFC1918 addresses may be shown if they do not expose public infrastructure.

---

## Recommended Evidence Naming Convention

Use clear and descriptive filenames.

Examples:

```text
github-actions-passing.png
kubernetes-nodes-ready.png
kubernetes-pods-running.png
prometheus-alert-firing.png
alertmanager-alert.png
github-incident-issue.png
deployment-rollout-success.txt
health-check-success.txt
rollback-validation.txt
```

Avoid vague names such as:

```text
image1.png
screenshot-final.png
test.png
new.png
```

---

## How This Evidence Supports the Runbooks

The evidence in this directory supports the operational runbooks under the `runbooks/` directory.

| Runbook | Evidence Support |
|---|---|
| `deployment-runbook.md` | CI/CD success, Kubernetes readiness, pod health, and deployment verification |
| `rollback-runbook.md` | Can be supported by rollback validation screenshots or command outputs |
| `troubleshooting.md` | Supports troubleshooting evidence such as events, logs, alerts, and incident records |

---

## Portfolio Value

This evidence is included to show that the homelab is actively operated and validated.

It helps demonstrate:

- Practical DevOps/SRE implementation
- CI/CD workflow validation
- Kubernetes operational knowledge
- Observability and alerting maturity
- Incident management discipline
- Evidence-based troubleshooting
- Production-style thinking

This is useful for DevOps, DevSecOps, Platform Engineering, and Site Reliability Engineering interview discussions.

---

## Future Evidence to Add

Recommended future evidence:

```text
evidence/deployment-rollout-status.txt
evidence/health-check-result.txt
evidence/rollback-success.txt
evidence/trivy-scan-result.txt
evidence/gitleaks-scan-result.txt
evidence/semgrep-scan-result.txt
evidence/wazuh-alert-review.png
evidence/security-onion-network-visibility.png
evidence/grafana-dashboard.png
```

---

## Summary

This directory provides visual and command-based proof that the homelab DevSecOps environment is functioning and being operated with production-style discipline.

The goal is not only to show tools installed, but to show that the environment can be deployed, monitored, validated, troubleshot, and improved using repeatable operational practices.
