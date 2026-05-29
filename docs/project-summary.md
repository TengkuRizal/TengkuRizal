# Project Summary

## Overview

This project documents a production-style DevSecOps/SRE homelab environment designed to demonstrate practical skills in CI/CD, Kubernetes operations, monitoring, alerting, incident tracking, runbooks, and operational evidence collection.

The goal of this project is not only to show that tools were installed, but to show that the environment can be deployed, validated, monitored, troubleshot, and recovered using repeatable DevOps/SRE practices.

---

## What This Project Demonstrates

This project demonstrates hands-on capability across the following areas:

| Area | Demonstrated Capability |
|---|---|
| CI/CD | Automated pipeline execution using Git-based workflows |
| Kubernetes | Cluster validation, pod health checks, deployment verification, and operational troubleshooting |
| Monitoring | Prometheus-based alerting and metric visibility |
| Alerting | Alertmanager workflow validation |
| Incident Management | GitHub issue-based incident documentation |
| Runbooks | Deployment, rollback, and troubleshooting procedures |
| Evidence Collection | Screenshots and command output structure for operational proof |
| Security Awareness | Sanitized evidence, secret handling awareness, and security monitoring mindset |
| SRE Practices | Reliability validation, rollback readiness, and post-fix verification |

---

## Tools and Technologies

| Tool / Platform | Purpose |
|---|---|
| GitHub | Repository hosting, documentation, and incident issue tracking |
| GitHub Actions | CI/CD workflow execution |
| Kubernetes | Container orchestration and workload management |
| kubectl | Kubernetes administration and validation |
| Prometheus | Metrics collection and alert rule validation |
| Alertmanager | Alert routing and alert visibility |
| Grafana | Monitoring dashboard visualization |
| Wazuh | Security monitoring and alert review |
| Security Onion | Network visibility and traffic analysis |
| Docker | Container image build and runtime testing |
| Markdown | Operational documentation and runbook structure |

---

## Architecture Summary

The project follows a production-style operational flow:

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
Monitoring and Alerting
    ↓
Incident Tracking
    ↓
Runbook-Based Troubleshooting and Recovery
```

This flow demonstrates how a change moves from source control to runtime operation, while still being supported by monitoring, alerting, evidence, and recovery procedures.

---

## Key Repository Areas

| Path | Purpose |
|---|---|
| `README.md` | Main portfolio landing page |
| `runbooks/` | Operational runbooks for deployment, rollback, and troubleshooting |
| `evidence/` | Sanitized screenshots and supporting operational evidence |
| `evidence/screenshots/` | Visual proof of CI/CD, Kubernetes, monitoring, alerting, and incident tracking |
| `docs/` | Supporting documentation and project explanations |

---

## Evidence Included

The repository includes screenshots that provide proof of operational validation.

| Evidence | What It Shows |
|---|---|
| `github-actions-passing.png` | CI/CD workflow completed successfully |
| `kubernetes-nodes-ready.png` | Kubernetes nodes are healthy and ready |
| `kubernetes-pods-running.png` | Kubernetes workloads are running |
| `prometheus-alert-firing.png` | Prometheus alert rule firing |
| `alertmanager-alert.png` | Alertmanager receiving active alerts |
| `github-incident-issue.png` | Incident tracking using GitHub Issues |

This evidence helps demonstrate that the environment is not only documented, but also actively validated.

---

## Runbooks Included

| Runbook | Purpose |
|---|---|
| `runbooks/deployment-runbook.md` | Defines the deployment process, validation checks, monitoring steps, and evidence collection |
| `runbooks/rollback-runbook.md` | Defines recovery procedures for failed deployments, unstable workloads, and rollback scenarios |
| `runbooks/troubleshooting.md` | Defines structured troubleshooting steps for Git, CI/CD, Kubernetes, monitoring, Wazuh, Security Onion, DNS, storage, and node issues |
| `runbooks/README.md` | Provides an index and explanation of all runbooks |

These runbooks demonstrate production-style operational discipline.

---

## DevOps/SRE Practices Demonstrated

### 1. Deployment Discipline

The deployment process is documented with:

- Pre-deployment checks
- Pipeline validation
- Kubernetes rollout verification
- Health checks
- Log validation
- Monitoring review
- Evidence capture

### 2. Rollback Readiness

The rollback process covers:

- Kubernetes rollout undo
- Git revert rollback
- Image tag rollback
- ConfigMap and Secret rollback awareness
- Terraform rollback awareness
- Database rollback caution
- Post-rollback validation

### 3. Troubleshooting Discipline

The troubleshooting guide covers:

- Pipeline failures
- GitLab/GitHub workflow issues
- Kubernetes pod failures
- Node readiness issues
- DNS and network problems
- Monitoring and alerting issues
- Wazuh and Security Onion issues
- Disk pressure and resource pressure

### 4. Monitoring and Alerting

The project includes evidence of:

- Prometheus alert rule firing
- Alertmanager alert visibility
- Kubernetes node and pod checks
- Incident tracking workflow

### 5. Incident Management

Incident handling is demonstrated through:

- GitHub issue-based incident documentation
- Evidence collection
- Runbook references
- Post-fix validation mindset
- Continuous improvement recommendations

---

## Mapping to Senior DevOps/SRE Responsibilities

| Senior DevOps/SRE Responsibility | Project Evidence |
|---|---|
| Build and maintain CI/CD pipelines | GitHub Actions pipeline evidence |
| Operate Kubernetes workloads | Kubernetes node and pod validation screenshots |
| Monitor service health | Prometheus and Alertmanager evidence |
| Respond to incidents | GitHub incident issue and runbooks |
| Create operational documentation | Deployment, rollback, and troubleshooting runbooks |
| Improve reliability | Rollback readiness and post-fix validation |
| Support security operations | Wazuh and Security Onion references in runbooks |
| Troubleshoot production-style issues | Troubleshooting runbook with real operational scenarios |
| Capture evidence and audit trail | Evidence directory and screenshot index |

---

## Interview Talking Points

This project can be used to explain:

- How a CI/CD pipeline validates and deploys workloads
- How Kubernetes deployment health is verified
- How monitoring and alerting support reliability
- How incident tracking is documented
- How rollback decisions are made
- How troubleshooting is structured under pressure
- How evidence is collected for operational proof
- How security monitoring tools fit into DevSecOps operations
- How a homelab can simulate production-style responsibilities

---

## What Makes This Project Valuable

This project is valuable because it connects tools with operational practice.

Many labs only show installation steps. This project shows:

- How the environment is operated
- How deployments are validated
- How alerts are tested
- How incidents are tracked
- How failures are handled
- How evidence is collected
- How runbooks support repeatable operations

This makes the project more relevant for DevOps, DevSecOps, Platform Engineering, and Site Reliability Engineering roles.

---

## Current Limitations

This project is based on a homelab environment, so it does not represent a full enterprise production environment.

Current limitations may include:

- Limited production traffic simulation
- Limited multi-region architecture
- Limited automated rollback
- Limited advanced SLO implementation
- Limited chaos engineering coverage
- Limited centralized log analytics evidence

These are acceptable limitations for a portfolio project and can be used as future improvement areas.

---

## Future Improvements

Recommended future improvements:

- Add ArgoCD GitOps workflow
- Add Argo Rollouts for progressive delivery
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

---

## Summary

This project demonstrates a practical DevSecOps/SRE operating model using a homelab environment.

It shows the ability to:

- Build and validate CI/CD workflows
- Operate Kubernetes workloads
- Monitor and alert on system behavior
- Track incidents
- Document deployment and recovery procedures
- Troubleshoot operational failures
- Capture evidence
- Think in a production-style reliability mindset

The overall objective is to show readiness for DevOps, DevSecOps, Platform Engineering, or Site Reliability Engineering responsibilities.
