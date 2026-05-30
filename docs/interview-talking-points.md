# Interview Talking Points

## Purpose

This document provides structured talking points to explain the `gitlab-devsecops-pipeline` project during DevOps, DevSecOps, Platform Engineering, or Site Reliability Engineering interviews.

The goal is to help explain not only what was built, but also why it was built, how it works, what trade-offs were made, how failures are handled, and how the project maps to real production responsibilities.

---

## 60-Second Project Pitch

I built this DevSecOps CI/CD and GitOps project to demonstrate secure and reliable application delivery into Kubernetes.

The project simulates a production-style delivery workflow where code changes go through validation, secret scanning, static application security testing, Kubernetes configuration scanning, container image build, SBOM generation, image vulnerability scanning, deployment to Kubernetes, and rollout verification.

I then extended the project with ArgoCD GitOps. Kubernetes manifests are stored in Git, ArgoCD syncs the desired state to the cluster, and I validated both GitOps scale reconciliation and self-healing drift correction.

Beyond the pipeline itself, I added operational evidence, screenshots, deployment runbooks, rollback procedures, GitOps runbook, and troubleshooting documentation.

---

## 2-Minute Project Explanation

This project is a hands-on DevSecOps, GitOps, and SRE operations portfolio.

The CI/CD side validates Kubernetes manifests, checks for secrets using Gitleaks, performs static analysis using Semgrep, scans Kubernetes configuration using Trivy, builds a Docker image, generates an SBOM using Syft, scans the image using Trivy, deploys the workload to Kubernetes, and verifies the rollout using `kubectl rollout status`.

The GitOps side uses ArgoCD. I added Kubernetes manifests under `kubernetes/` and an ArgoCD Application under `argocd/applications/demo-nginx-app.yaml`. ArgoCD watches the GitHub repository and manages the `demo-nginx` workload in the `demo` namespace.

I validated GitOps with two practical tests. First, I changed the replica count in Git from 2 to 3 and ArgoCD reconciled the deployment to 3 available replicas. Second, I manually created drift with `kubectl scale` by scaling the deployment down to 1 replica. ArgoCD detected `OutOfSync` and self-healed the deployment back to the Git-defined desired state of 3 replicas.

This project demonstrates the full lifecycle: build, scan, deploy, sync, verify, monitor, troubleshoot, self-heal, rollback, document, and improve.

---

## Architecture Explanation

```text
Developer Push
      ↓
Git Repository
      ↓
CI/CD Pipeline
      ↓
Validation and Security Checks
      ↓
Container Build and SBOM Generation
      ↓
Container Image Vulnerability Scan
      ↓
Kubernetes Deployment
      ↓
ArgoCD GitOps Reconciliation
      ↓
Monitoring and Alerting
      ↓
Incident Tracking and Runbooks
```

The important point is that the project does not stop at deployment. It includes verification, monitoring evidence, alerting evidence, incident tracking, GitOps reconciliation, self-healing validation, and documented recovery procedures.

---

## Why I Built This Project

I built this project because DevOps and SRE roles require more than tool installation.

A strong DevOps/SRE engineer should be able to automate delivery, enforce security checks, operate Kubernetes workloads, implement GitOps deployment control, validate deployment health, monitor service behavior, respond to incidents, roll back safely, detect and correct drift, capture operational evidence, maintain runbooks, and improve reliability over time.

---

## Problem This Project Solves

Traditional CI/CD pipelines often focus on build and deployment only.

That creates risks such as committed secrets, vulnerable dependencies, insecure Kubernetes manifests, container images with known CVEs, deployments without health verification, lack of rollback readiness, manual cluster drift, undocumented incidents, and unvalidated monitoring.

This project solves those problems by integrating security checks, GitOps reconciliation, monitoring, evidence, and runbooks into the delivery workflow.

---

## Pipeline Stage Explanation

| Stage | Tool | What I Would Explain in Interview |
|---|---|---|
| Validate | kubectl | I validate Kubernetes manifests before deployment to catch syntax and schema issues early |
| Secret Scan | Gitleaks | I prevent secrets, tokens, passwords, and credentials from entering the repository |
| SAST | Semgrep | I detect insecure code patterns before the application is deployed |
| Config Scan | Trivy | I scan Kubernetes YAML files for misconfigurations such as privileged containers or missing security context |
| Build | Docker | I build the application container image in a repeatable way |
| SBOM | Syft | I generate a Software Bill of Materials so dependencies are visible |
| Image Scan | Trivy | I scan the final image for known vulnerabilities |
| Deploy | kubectl / ArgoCD | I deploy and reconcile the workload using Kubernetes and GitOps workflows |
| Verify | kubectl rollout / ArgoCD | I verify that the deployment becomes healthy and synced |

---

## Security Controls Explanation

### Secret Detection

> I use secret scanning because leaked credentials are one of the fastest ways to compromise an environment. If Gitleaks detects a real secret, the correct action is not only to remove it from code, but also rotate the credential and review Git history.

### Static Application Security Testing

> I use SAST to catch risky code patterns before runtime. This does not replace manual review, but it improves early detection and creates a security gate in the pipeline.

### Kubernetes Configuration Scanning

> Kubernetes misconfiguration is a common production risk. I scan manifests to catch issues like privileged containers, missing resource limits, running as root, and weak security contexts before they are applied to the cluster.

### Container Image Vulnerability Scanning

> Image scanning helps identify vulnerable OS packages and application dependencies. For critical vulnerabilities, the image should be patched or risk-accepted through a formal process before deployment.

### SBOM Generation

> SBOM helps provide dependency visibility. It is useful for supply chain security because it helps teams understand what components are inside the image.

---

## Kubernetes Deployment Explanation

The Kubernetes deployment is validated through namespace checks, pod status checks, service checks, endpoint checks, rollout status verification, log review, and health check testing.

Example validation commands:

```bash
kubectl get nodes -o wide
kubectl get pods -n demo
kubectl get svc -n demo
kubectl get endpoints -n demo
kubectl rollout status deployment/demo-nginx -n demo
```

Interview explanation:

> I do not consider a deployment successful just because `kubectl apply` completed. I verify rollout status, pod readiness, service endpoints, logs, and application response. This is important because a deployment can be applied successfully but still fail at runtime.

---

## Phase 2 GitOps Talking Points

### What I Implemented

In Phase 2, I implemented ArgoCD to introduce GitOps-based deployment control into the homelab Kubernetes environment.

I created Kubernetes manifests under the `kubernetes/` directory and created an ArgoCD Application manifest under `argocd/applications/demo-nginx-app.yaml`.

ArgoCD now watches the GitHub repository and reconciles the `demo-nginx` workload in the `demo` namespace.

### How I Validated It

I validated Phase 2 using two practical tests.

#### Test 1: GitOps Scale Test

I changed the desired replica count in Git from 2 replicas to 3 replicas.

After committing and pushing the change, ArgoCD synchronized the desired state and Kubernetes updated the deployment to 3 available replicas.

Evidence:

```text
demo-nginx   3/3   3   3
```

This proves that ArgoCD can reconcile Git desired state into the live Kubernetes cluster.

#### Test 2: Self-Heal / Drift Correction Test

I manually created drift using `kubectl scale` by scaling the live deployment down from 3 replicas to 1 replica.

ArgoCD detected the live state was different from Git desired state and marked the application as `OutOfSync`.

Because self-heal was enabled, ArgoCD reconciled the workload back to the Git-defined desired state of 3 replicas.

Final state:

```text
demo-nginx   Synced   Healthy
demo-nginx   3/3
```

This proves GitOps drift detection and self-healing reconciliation.

### Strong Interview Explanation

> I implemented ArgoCD GitOps in my homelab and onboarded `demo-nginx` as an ArgoCD Application. The Kubernetes manifests are stored in GitHub, and ArgoCD continuously compares the Git desired state with the live cluster state. I validated the workflow by changing replicas through Git and observing ArgoCD sync the deployment to 3 replicas. Then I manually created drift using `kubectl scale`, and ArgoCD self-healed the deployment back to the Git-defined state. This demonstrates GitOps deployment control, auditability, drift detection, and desired-state reconciliation.

### Why This Matters

GitOps provides Git-based audit trail, desired-state control, drift detection, safer rollback through Git history, separation of CI and CD, better deployment visibility, and self-healing reconciliation.

---

## Monitoring and Alerting Explanation

The project includes evidence for Prometheus alert firing, Alertmanager receiving alerts, Kubernetes nodes ready, Kubernetes pods running, ArgoCD application Synced and Healthy, and incident tracking through GitHub Issues.

Interview explanation:

> Monitoring and alerting are important because deployment success is not only about pushing code. I need to know whether the system remains healthy after release. Prometheus provides metrics and alert rules, Alertmanager handles alert visibility and routing, ArgoCD provides GitOps sync and health visibility, and incident tracking ensures operational issues are documented.

---

## Incident Management Explanation

> I use issue-based incident tracking to document the symptom, impact, detection method, actions taken, resolution, and follow-up. This helps create an audit trail and supports post-incident learning.

A good incident record should include what happened, when it happened, what was impacted, how it was detected, what action was taken, whether rollback was required, what evidence was captured, and what preventive action is needed.

---

## Rollback Explanation

The rollback runbook covers Kubernetes rollout undo, rollback to a specific Kubernetes revision, Git revert rollback, image tag rollback, ConfigMap or Secret rollback, Terraform rollback awareness, database rollback caution, and GitOps rollback through Git history.

Interview explanation:

> Rollback is a reliability control. If a deployment causes user impact or service instability, the priority is to restore service quickly using a known-good version. Root cause analysis should happen after service recovery.

Important point:

> I prefer `git revert` over force-push because it preserves audit history and is safer in a team environment. In GitOps, rollback should generally be done by reverting Git desired state and letting ArgoCD reconcile the cluster.

---

## Troubleshooting Explanation

The troubleshooting runbook covers Git and GitHub, CI/CD pipelines, GitLab Runner, KUBECONFIG, Kubernetes pods, Kubernetes services, Kubernetes nodes, DNS/networking, Prometheus/Grafana, Wazuh, Security Onion, ArgoCD, storage, disk pressure, and performance issues.

Interview explanation:

> My troubleshooting approach is to confirm the symptom, identify the affected layer, check recent changes, collect evidence, review logs/events, apply the minimum safe fix, validate the result, and document the outcome.

---

## Example Troubleshooting Scenario: ArgoCD Application Unknown

Question:

> What would you do if an ArgoCD Application shows Unknown?

Answer:

I would inspect the application condition first:

```bash
kubectl describe application demo-nginx -n argocd
```

Then I would check repo-server and application-controller logs:

```bash
kubectl logs -n argocd deployment/argocd-repo-server --tail=100
kubectl logs -n argocd statefulset/argocd-application-controller --tail=100
```

Common causes include wrong repo URL, wrong manifest path, missing Git folder, GitHub access issue, ArgoCD repo cache issue, or manifest generation error.

In my project, I saw an `app path does not exist` error when the `kubernetes/` path was not yet present in GitHub. I fixed it by adding the Kubernetes manifests to the repo, then refreshed ArgoCD.

---

## Senior-Level Trade-Offs

### CI/CD vs GitOps

CI/CD is useful for validation, testing, scanning, image build, and artifact creation.

GitOps is useful for continuous delivery, desired-state reconciliation, drift detection, and cluster state control.

A strong production model often separates CI and CD:

```text
CI builds and validates artifacts.
GitOps reconciles desired state into Kubernetes.
```

### Manual Deployment vs GitOps Deployment

Manual deployment is useful for emergency debugging but should not be the normal path. GitOps deployment provides repeatability, auditability, consistency, and drift detection.

### Fast Rollback vs Full Troubleshooting

During active service impact, fast rollback is usually better than long troubleshooting. Root cause analysis should happen after service is restored.

### Homelab vs Production

A homelab cannot fully represent enterprise production traffic, compliance, or scale. However, it can demonstrate architecture thinking, operational discipline, failure handling, GitOps concepts, and hands-on capability.

---

## What I Would Improve Next

If I had more time, I would improve the project by adding:

- Argo Rollouts for canary deployment
- Kustomize overlays for dev/staging/prod
- Automated smoke tests
- Automated rollback trigger
- Loki for centralized logging
- OpenTelemetry tracing
- SLO and error budget documentation
- Kyverno policy-as-code
- Cosign image signing
- Falco runtime security monitoring
- Wazuh alert evidence
- Security Onion network visibility evidence
- Terraform infrastructure documentation
- Chaos engineering test case
- ArgoCD RBAC and notifications
- External Secrets or Vault integration

---

## Questions I Can Answer From This Project

I can confidently discuss how CI/CD pipelines enforce secure delivery, how secret scanning reduces credential exposure risk, how SAST helps catch insecure code patterns, how Trivy scans Kubernetes config and container images, why SBOM matters, how Kubernetes deployments are validated, how ArgoCD supports GitOps deployment control, how desired-state reconciliation works, how ArgoCD self-healing corrects manual drift, how monitoring and alerting fit into SRE operations, how incident tracking supports reliability, how rollback decisions should be made, and how to troubleshoot Kubernetes workload failures.

---

## Strong Interview Closing Statement

This project represents how I approach DevOps and SRE work: automate what can be automated, validate before and after deployment, shift security left, manage desired state through GitOps, monitor runtime behavior, prepare rollback procedures, document troubleshooting steps, and continuously improve based on evidence.

It is not just a tool demo. It is an operating model for delivering and supporting workloads more safely.
