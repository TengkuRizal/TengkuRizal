# Project Summary

## Overview

This project documents a production-style DevSecOps/SRE homelab environment that demonstrates CI/CD, Kubernetes operations, GitOps, monitoring, alerting, incident tracking, runbooks, and evidence collection.

The goal is not only to show that tools were installed. The goal is to show that the environment can be deployed, validated, monitored, reconciled, troubleshot, recovered, and improved using repeatable DevOps/SRE practices.

---

## What This Project Demonstrates

| Area | Demonstrated Capability |
|---|---|
| CI/CD | Automated pipeline execution using Git-based workflows |
| Kubernetes | Cluster validation, pod health checks, deployment verification, and operational troubleshooting |
| GitOps | ArgoCD-based desired-state reconciliation, sync validation, and self-healing |
| Monitoring | Prometheus-based alerting and metric visibility |
| Alerting | Alertmanager workflow validation |
| Incident Management | GitHub issue-based incident documentation |
| Runbooks | Deployment, rollback, GitOps deployment, and troubleshooting procedures |
| Evidence Collection | Screenshots and command output structure for operational proof |
| Security Awareness | Sanitized evidence, secret handling awareness, and security monitoring mindset |
| SRE Practices | Reliability validation, rollback readiness, drift detection, and post-fix verification |

---

## Tools and Technologies

| Tool / Platform | Purpose |
|---|---|
| GitHub | Repository hosting, documentation, and incident issue tracking |
| GitHub Actions | CI/CD workflow execution and public portfolio evidence |
| GitLab CI/CD | Secure pipeline design pattern for validation, scanning, build, deploy, and verify stages |
| Kubernetes | Container orchestration and workload management |
| kubectl | Kubernetes administration and validation |
| ArgoCD | GitOps continuous delivery, desired-state reconciliation, and self-healing |
| Prometheus | Metrics collection and alert rule validation |
| Alertmanager | Alert routing and alert visibility |
| Grafana | Monitoring dashboard visualization |
| Wazuh | Security monitoring and alert review |
| Security Onion | Network visibility and traffic analysis |
| Docker | Container image build and runtime testing |
| Gitleaks | Secret scanning |
| Semgrep | Static application security testing |
| Trivy | Kubernetes configuration and container image vulnerability scanning |
| Syft | Software Bill of Materials generation |

---

## Architecture Summary

```text
Code Change
    ↓
Git Repository
    ↓
CI/CD Pipeline
    ↓
Validation and Security Checks
    ↓
Container Build / Deployment
    ↓
Kubernetes Runtime
    ↓
GitOps Reconciliation with ArgoCD
    ↓
Monitoring and Alerting
    ↓
Incident Tracking
    ↓
Runbook-Based Troubleshooting and Recovery
```

This flow demonstrates how a change moves from source control to runtime operation, while still being supported by monitoring, alerting, evidence, GitOps reconciliation, and recovery procedures.

---

## Key Repository Areas

| Path | Purpose |
|---|---|
| `README.md` | Main portfolio landing page |
| `docs/project-summary.md` | Project summary and portfolio explanation |
| `docs/interview-talking-points.md` | Interview-ready explanation of the project |
| `runbooks/` | Operational runbooks for deployment, rollback, GitOps, and troubleshooting |
| `evidence/` | Sanitized screenshots and operational evidence |
| `evidence/screenshots/` | Visual proof of CI/CD, Kubernetes, monitoring, alerting, GitOps, and incident tracking |
| `evidence/phase-1/` | Baseline Kubernetes and monitoring validation evidence |
| `evidence/phase-2/` | ArgoCD GitOps sync, scale test, and self-heal evidence |
| `kubernetes/` | Kubernetes desired-state manifests for `demo-nginx` |
| `argocd/applications/` | ArgoCD Application manifests |

---

## Evidence Included

| Evidence | What It Shows |
|---|---|
| `github-actions-passing.png` | CI/CD workflow completed successfully |
| `kubernetes-nodes-ready.png` | Kubernetes nodes are healthy and ready |
| `kubernetes-pods-running.png` | Kubernetes workloads are running |
| `prometheus-alert-firing.png` | Prometheus alert rule firing |
| `alertmanager-alert.png` | Alertmanager receiving active alerts |
| `github-incident-issue.png` | Incident tracking using GitHub Issues |
| `argocd-demo-nginx-synced.png` | ArgoCD showing `demo-nginx` as Synced and Healthy |
| `gitops-scale-test-deployment.txt` | `demo-nginx` scaled to 3 replicas through GitOps |
| `gitops-self-heal-deployment.txt` | ArgoCD self-healed manual drift back to 3 replicas |

---

## Phase 1 Baseline Validation

Phase 1 focused on proving that the existing homelab platform had a stable operational baseline.

| Capability | Result |
|---|---|
| Kubernetes nodes | All nodes were validated as Ready |
| Kubernetes workloads | Pods and deployments were captured across namespaces |
| Services | Kubernetes service exposure was documented |
| Monitoring | Prometheus, Grafana, Alertmanager, node-exporter, and kube-state-metrics were validated |
| Security tools | Falco, Kyverno, Wazuh, and Security Onion were included in operational documentation |
| Evidence | Baseline evidence was captured under `evidence/phase-1/` |

Phase 1 established a known-good baseline before implementing more advanced DevOps/SRE capabilities.

---

## Phase 2 GitOps Implementation

Phase 2 introduced ArgoCD-based GitOps deployment control.

Kubernetes manifests were added under the `kubernetes/` directory, and an ArgoCD Application was created under `argocd/applications/demo-nginx-app.yaml`.

The `demo-nginx` workload is now managed from Git as the source of truth.

### Phase 2 Outcomes

| Capability | Result |
|---|---|
| ArgoCD installation | ArgoCD was installed and exposed through NodePort for homelab access |
| GitOps application | `demo-nginx` was onboarded as an ArgoCD Application |
| Desired state source | GitHub repository `kubernetes/` directory is used as the desired state |
| Sync validation | ArgoCD reported the application as `Synced` and `Healthy` |
| Scale test | Replica count was changed in Git and reconciled by ArgoCD |
| Self-heal test | Manual drift was created with `kubectl scale`, and ArgoCD restored the workload back to Git desired state |
| Evidence | Phase 2 evidence was captured under `evidence/phase-2/` |

### Phase 2 Evidence

Relevant evidence:

- `evidence/phase-2/README.md`
- `evidence/phase-2/gitops-scale-test-deployment.txt`
- `evidence/phase-2/gitops-self-heal-deployment.txt`
- `evidence/screenshots/argocd-demo-nginx-synced.png`

### Phase 2 Interview Value

Phase 2 proves that the platform supports GitOps delivery, drift detection, desired-state reconciliation, and self-healing.

A strong interview summary:

> I implemented ArgoCD GitOps, synced a Kubernetes workload from Git, tested desired-state reconciliation by scaling replicas through Git, and tested self-healing by manually creating drift with `kubectl scale`. ArgoCD detected the drift and restored the workload back to the Git-defined state.

---

## Runbooks Included

| Runbook | Purpose |
|---|---|
| `runbooks/deployment-runbook.md` | Defines the deployment process, validation checks, monitoring steps, and evidence collection |
| `runbooks/rollback-runbook.md` | Defines recovery procedures for failed deployments, unstable workloads, and rollback scenarios |
| `runbooks/gitops-deployment-runbook.md` | Defines ArgoCD GitOps deployment, sync, validation, troubleshooting, and rollback process |
| `runbooks/troubleshooting.md` | Defines structured troubleshooting steps for Git, CI/CD, Kubernetes, monitoring, Wazuh, Security Onion, DNS, storage, and node issues |
| `runbooks/README.md` | Provides an index and explanation of all runbooks |

---

## DevOps/SRE Practices Demonstrated

### Deployment Discipline

The deployment process is documented with pre-deployment checks, pipeline validation, Kubernetes rollout verification, health checks, log validation, monitoring review, and evidence capture.

### GitOps Deployment Control

The GitOps process demonstrates Git as the source of truth, ArgoCD Application management, sync and health validation, drift detection, self-healing reconciliation, and rollback through Git history.

### Rollback Readiness

The rollback process covers Kubernetes rollout undo, Git revert rollback, image tag rollback, ConfigMap and Secret rollback awareness, Terraform rollback awareness, database rollback caution, GitOps rollback through Git desired state, and post-rollback validation.

### Troubleshooting Discipline

The troubleshooting guide covers pipeline failures, GitLab/GitHub workflow issues, Kubernetes pod failures, node readiness issues, DNS/network problems, monitoring/alerting issues, Wazuh/Security Onion issues, disk pressure, resource pressure, and ArgoCD sync issues.

### Monitoring and Alerting

The project includes evidence of Prometheus alert rule firing, Alertmanager alert visibility, Kubernetes node and pod checks, ArgoCD sync/health state, and incident tracking workflow.

---

## Mapping to Senior DevOps/SRE Responsibilities

| Senior DevOps/SRE Responsibility | Project Evidence |
|---|---|
| Build and maintain CI/CD pipelines | GitHub Actions pipeline evidence and GitLab CI/CD pipeline design |
| Operate Kubernetes workloads | Kubernetes node and pod validation screenshots |
| Implement GitOps delivery | ArgoCD Application, sync evidence, scale test, and self-heal test |
| Monitor service health | Prometheus and Alertmanager evidence |
| Respond to incidents | GitHub incident issue and runbooks |
| Create operational documentation | Deployment, rollback, GitOps, and troubleshooting runbooks |
| Improve reliability | Rollback readiness, self-healing, and post-fix validation |
| Support security operations | Wazuh and Security Onion references in runbooks |
| Troubleshoot production-style issues | Troubleshooting runbook with real operational scenarios |
| Capture evidence and audit trail | Evidence directory and screenshot index |

---

## Interview Talking Points

This project can be used to explain:

- How a CI/CD pipeline validates and deploys workloads
- How Kubernetes deployment health is verified
- How ArgoCD implements GitOps desired-state reconciliation
- How ArgoCD self-heal corrects manual drift
- How monitoring and alerting support reliability
- How incident tracking is documented
- How rollback decisions are made
- How troubleshooting is structured under pressure
- How evidence is collected for operational proof
- How a homelab can simulate production-style responsibilities

---

## Current Limitations

This project is based on a homelab environment, so it does not represent a full enterprise production environment.

Current limitations include limited production traffic simulation, limited multi-region architecture, limited advanced SLO implementation, limited chaos engineering coverage, limited centralized log analytics evidence, limited ArgoCD RBAC/project isolation, and limited progressive delivery implementation.

---

## Future Improvements

Recommended future improvements:

- Add Argo Rollouts for progressive delivery
- Add Kustomize overlays for dev/staging/prod
- Add Loki for centralized logging
- Add OpenTelemetry tracing
- Add SLO and error budget documentation
- Add automated smoke tests
- Add automated rollback trigger
- Add Wazuh alert evidence
- Add Security Onion network visibility evidence
- Add chaos engineering test case
- Add Terraform infrastructure documentation
- Add disaster recovery simulation
- Add ArgoCD RBAC and notifications
- Add External Secrets or Vault integration with GitOps

---

## Summary

This project demonstrates a practical DevSecOps/SRE operating model using a homelab environment.

It shows the ability to build and validate CI/CD workflows, operate Kubernetes workloads, implement GitOps with ArgoCD, prove desired-state reconciliation and self-healing, monitor and alert on system behavior, track incidents, document deployment and recovery procedures, troubleshoot operational failures, capture evidence, and think in a production-style reliability mindset.
