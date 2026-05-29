# Interview Talking Points

## Purpose

This document provides structured talking points to explain the `gitlab-devsecops-pipeline` project during DevOps, DevSecOps, Platform Engineering, or Site Reliability Engineering interviews.

The goal is to help explain not only what was built, but also why it was built, how it works, what trade-offs were made, how failures are handled, and how the project maps to real production responsibilities.

---

## 60-Second Project Pitch

I built this DevSecOps CI/CD pipeline project to demonstrate secure and reliable application delivery into Kubernetes.

The project simulates a production-style delivery workflow where code changes go through validation, secret scanning, static application security testing, Kubernetes configuration scanning, container image build, SBOM generation, image vulnerability scanning, deployment to Kubernetes, and rollout verification.

Beyond the pipeline itself, I added operational evidence, screenshots, deployment runbooks, rollback procedures, and troubleshooting documentation. This shows that the environment is not only built, but also operated with DevOps/SRE discipline.

The project demonstrates CI/CD automation, Kubernetes operations, security shift-left practices, monitoring, alerting, incident tracking, evidence collection, and recovery readiness.

---

## 2-Minute Project Explanation

This project is a hands-on DevSecOps pipeline and operations portfolio.

The pipeline starts when a developer pushes code. The CI/CD workflow validates Kubernetes manifests, checks for secrets using Gitleaks, performs static analysis using Semgrep, scans Kubernetes configuration using Trivy, builds a Docker image, generates an SBOM using Syft, scans the image using Trivy, deploys the workload to Kubernetes, and verifies the rollout using `kubectl rollout status`.

The operational side of the project includes screenshots showing successful pipeline execution, Kubernetes nodes and pods running, Prometheus alert firing, Alertmanager receiving alerts, and GitHub Issues being used for incident tracking.

I also created production-style runbooks for deployment, rollback, and troubleshooting. These runbooks cover real operational scenarios such as failed rollouts, `ImagePullBackOff`, `CrashLoopBackOff`, KUBECONFIG issues, monitoring failures, Wazuh/Security Onion validation, DNS issues, node pressure, disk pressure, and rollback decision-making.

This project is designed to show that I understand the full lifecycle: build, scan, deploy, verify, monitor, troubleshoot, rollback, document, and improve.

---

## Architecture Explanation

The project follows this high-level flow:

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
Rollout Verification
      ↓
Monitoring and Alerting
      ↓
Incident Tracking and Runbooks
```

The architecture demonstrates a secure software delivery model where security checks happen before deployment, and operational validation happens after deployment.

The important point is that the pipeline does not stop at deployment. It also includes verification, monitoring evidence, alerting evidence, incident tracking, and documented recovery procedures.

---

## Why I Built This Project

I built this project because DevOps and SRE roles require more than tool installation.

A strong DevOps/SRE engineer should be able to:

- Automate delivery
- Enforce security checks
- Operate Kubernetes workloads
- Validate deployment health
- Monitor service behavior
- Respond to incidents
- Roll back safely
- Capture operational evidence
- Maintain runbooks
- Improve reliability over time

This project was designed to demonstrate those capabilities in a practical homelab environment.

---

## Problem This Project Solves

Traditional CI/CD pipelines often focus on build and deployment only.

That creates several risks:

- Secrets may be committed into repositories
- Vulnerable dependencies may enter the build
- Kubernetes manifests may contain insecure configurations
- Container images may include known CVEs
- Deployment may complete without health verification
- Teams may lack rollback readiness
- Incidents may not be documented properly
- Monitoring and alerting may not be validated

This project solves those problems by integrating security and operational checks into the delivery workflow.

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
| Deploy | kubectl | I deploy the workload to Kubernetes using a controlled pipeline |
| Verify | kubectl rollout | I verify that the deployment actually becomes healthy |

---

## Security Controls Explanation

This project applies shift-left security by moving security checks earlier into the CI/CD lifecycle.

### Secret Detection

Gitleaks is used to detect exposed secrets before deployment.

Interview explanation:

> I use secret scanning because leaked credentials are one of the fastest ways to compromise an environment. If Gitleaks detects a real secret, the correct action is not only to remove it from code, but also rotate the credential and review Git history.

### Static Application Security Testing

Semgrep is used to detect insecure code patterns.

Interview explanation:

> I use SAST to catch risky code patterns before runtime. This does not replace manual review, but it improves early detection and creates a security gate in the pipeline.

### Kubernetes Configuration Scanning

Trivy Config is used to identify insecure Kubernetes configuration.

Interview explanation:

> Kubernetes misconfiguration is a common production risk. I scan manifests to catch issues like privileged containers, missing resource limits, running as root, and weak security contexts before they are applied to the cluster.

### Container Image Vulnerability Scanning

Trivy Image Scan is used to detect CVEs in container images.

Interview explanation:

> Image scanning helps identify vulnerable OS packages and application dependencies. For critical vulnerabilities, the image should be patched or risk-accepted through a formal process before deployment.

### SBOM Generation

Syft is used to generate an SBOM.

Interview explanation:

> SBOM helps provide dependency visibility. It is useful for supply chain security because it helps teams understand what components are inside the image.

---

## Kubernetes Deployment Explanation

The Kubernetes deployment is validated through:

- Namespace checks
- Pod status checks
- Service checks
- Endpoint checks
- Rollout status verification
- Log review
- Health check testing

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

## Monitoring and Alerting Explanation

The project includes evidence for:

- Prometheus alert firing
- Alertmanager receiving alerts
- Kubernetes nodes ready
- Kubernetes pods running
- Incident tracking through GitHub Issues

Interview explanation:

> Monitoring and alerting are important because deployment success is not only about pushing code. I need to know whether the system remains healthy after release. Prometheus provides metrics and alert rules, Alertmanager handles alert visibility and routing, and incident tracking ensures operational issues are documented.

---

## Incident Management Explanation

The project includes GitHub Issue evidence for incident tracking.

Interview explanation:

> I use issue-based incident tracking to document the symptom, impact, detection method, actions taken, resolution, and follow-up. This helps create an audit trail and supports post-incident learning.

A good incident record should include:

- What happened
- When it happened
- What was impacted
- How it was detected
- What action was taken
- Whether rollback was required
- What evidence was captured
- What preventive action is needed

---

## Rollback Explanation

The rollback runbook covers multiple rollback methods:

- Kubernetes rollout undo
- Rollback to a specific Kubernetes revision
- Git revert rollback
- Image tag rollback
- ConfigMap or Secret rollback
- Terraform rollback awareness
- Database rollback caution

Interview explanation:

> Rollback is a reliability control. If a deployment causes user impact or service instability, the priority is to restore service quickly using a known-good version. Root cause analysis should happen after service recovery.

Important point:

> I prefer `git revert` over force-push because it preserves audit history and is safer in a team environment.

---

## Troubleshooting Explanation

The troubleshooting runbook covers issues across:

- Git and GitHub
- CI/CD pipelines
- GitLab Runner
- KUBECONFIG
- Kubernetes pods
- Kubernetes services
- Kubernetes nodes
- DNS and networking
- Prometheus and Grafana
- Wazuh
- Security Onion
- Storage and disk pressure
- Performance issues

Interview explanation:

> My troubleshooting approach is to confirm the symptom, identify the affected layer, check recent changes, collect evidence, review logs/events, apply the minimum safe fix, validate the result, and document the outcome.

---

## Example Troubleshooting Scenario: ImagePullBackOff

Question:

> What would you do if a Kubernetes pod is stuck in ImagePullBackOff?

Answer:

I would start by describing the pod:

```bash
kubectl describe pod POD_NAME -n NAMESPACE
```

Then I would check the image reference:

```bash
kubectl get pod POD_NAME -n NAMESPACE -o jsonpath='{.spec.containers[*].image}'
```

Common causes include wrong image tag, image not pushed, registry authentication issue, missing imagePullSecret, registry DNS issue, or node connectivity problem.

I would verify the image exists, confirm credentials, check Kubernetes secrets, and review events. If the issue affects service availability and cannot be fixed quickly, I would follow the rollback runbook.

---

## Example Troubleshooting Scenario: CrashLoopBackOff

Question:

> What would you do if a pod is in CrashLoopBackOff?

Answer:

I would check current and previous logs:

```bash
kubectl logs POD_NAME -n NAMESPACE
kubectl logs POD_NAME -n NAMESPACE --previous
```

Then describe the pod:

```bash
kubectl describe pod POD_NAME -n NAMESPACE
```

Common causes include missing environment variables, invalid configuration, database connectivity issues, permission errors, wrong application port, or an application startup failure.

I would fix the root cause, redeploy, and validate rollout. If the service is impacted and the fix is not immediate, I would roll back.

---

## Example Troubleshooting Scenario: Rollout Timeout

Question:

> What would you do if `kubectl rollout status` times out?

Answer:

I would check the deployment, ReplicaSets, pods, and events:

```bash
kubectl describe deployment DEPLOYMENT_NAME -n NAMESPACE
kubectl get rs -n NAMESPACE
kubectl get pods -n NAMESPACE -o wide
kubectl get events -n NAMESPACE --sort-by=.metadata.creationTimestamp | tail -30
```

A rollout timeout usually means new pods are not becoming ready. The cause could be image pull failure, readiness probe failure, insufficient resources, bad config, or PVC mount issue.

If the deployment causes service impact and the issue is not resolved quickly, I would roll back to the previous known-good revision.

---

## Example Troubleshooting Scenario: KUBECONFIG Issue

Question:

> What would you do if the pipeline cannot connect to Kubernetes?

Answer:

I would check whether the pipeline has the correct `KUBECONFIG_B64` variable, whether it is available to the branch, and whether it decodes correctly.

I would validate the kubeconfig in a safe way without printing secrets:

```bash
echo "$KUBECONFIG_B64" | base64 -d > kubeconfig
export KUBECONFIG=$PWD/kubeconfig
kubectl config get-contexts
kubectl get nodes
```

Possible causes include missing CI/CD variable, protected variable mismatch, invalid base64 encoding, expired certificate, wrong cluster endpoint, or wrong context.

---

## Senior-Level Trade-Offs

### GitLab CI/CD vs GitHub Actions

GitLab CI/CD is powerful for self-hosted DevOps workflows and integrates well with GitLab Runner. GitHub Actions is useful for public portfolio demonstration because the repository is hosted on GitHub.

In this project, the DevSecOps workflow concepts are applicable to both.

### Manual Deployment vs CI/CD Deployment

Manual deployment is useful for emergency debugging but should not be the normal path. CI/CD deployment provides repeatability, auditability, and consistency.

### Fast Rollback vs Full Troubleshooting

During active service impact, fast rollback is usually better than long troubleshooting. Root cause analysis should happen after service is restored.

### Security Gate Strictness

Blocking every finding can slow delivery, but ignoring high or critical issues creates production risk. A mature process needs severity thresholds, risk acceptance, and documented exceptions.

### Homelab vs Production

A homelab cannot fully represent enterprise production traffic, compliance, or scale. However, it can demonstrate architecture thinking, operational discipline, failure handling, and hands-on capability.

---

## What I Would Improve Next

If I had more time, I would improve the project by adding:

- ArgoCD GitOps workflow
- Argo Rollouts for canary deployment
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

---

## Questions I Can Answer From This Project

I can confidently discuss:

- How CI/CD pipelines enforce secure delivery
- How secret scanning reduces credential exposure risk
- How SAST helps catch insecure code patterns
- How Trivy scans both Kubernetes config and container images
- Why SBOM matters in supply chain security
- How Kubernetes deployments are validated
- How monitoring and alerting fit into SRE operations
- How incident tracking supports reliability
- How rollback decisions should be made
- How to troubleshoot Kubernetes workload failures
- How to improve this project toward production maturity

---

## Strong Interview Closing Statement

This project represents how I approach DevOps and SRE work: automate what can be automated, validate before and after deployment, shift security left, monitor runtime behavior, prepare rollback procedures, document troubleshooting steps, and continuously improve based on evidence.

It is not just a tool demo. It is an operating model for delivering and supporting workloads more safely.
