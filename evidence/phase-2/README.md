# Phase 2 GitOps Baseline Evidence

## Overview

This folder contains Phase 2 evidence for the GitOps implementation using ArgoCD.

The purpose of Phase 2 is to introduce GitOps-based deployment control into the homelab DevSecOps/SRE platform. In this phase, Kubernetes manifests are stored in Git and ArgoCD continuously compares the desired state in Git with the live state in the Kubernetes cluster.

This provides a stronger production-style deployment model because Git becomes the source of truth for Kubernetes desired state.

---

## Phase 2 Objective

Implement and validate a basic GitOps workflow:

- Install ArgoCD
- Expose ArgoCD for homelab access
- Create an ArgoCD Application for `demo-nginx`
- Use GitHub repository as the source of truth
- Sync Kubernetes manifests from Git to the cluster
- Validate ArgoCD application health and sync status
- Capture evidence for portfolio and operational review

---

## GitOps Flow

```text
GitHub Repository
      ↓
kubernetes/ manifests
      ↓
ArgoCD Application
      ↓
Kubernetes API
      ↓
demo namespace
      ↓
demo-nginx Deployment and Service
```

---

## Evidence Files

| File | Purpose |
|---|---|
| [argocd-applications.txt](argocd-applications.txt) | Shows ArgoCD applications and their sync/health status |
| [demo-nginx-argocd-application.yaml](demo-nginx-argocd-application.yaml) | Full ArgoCD Application state for `demo-nginx` |
| [argocd-pods.txt](argocd-pods.txt) | Shows ArgoCD control plane pod status |
| [argocd-services.txt](argocd-services.txt) | Shows ArgoCD services and NodePort exposure |
| [demo-nginx-pods.txt](demo-nginx-pods.txt) | Shows `demo-nginx` workload pods after GitOps sync |
| [demo-nginx-service.txt](demo-nginx-service.txt) | Shows `demo-nginx` Kubernetes service |
| [demo-nginx-deployment.txt](demo-nginx-deployment.txt) | Shows deployment readiness and availability |
| [gitops-scale-test-argocd-app.txt](gitops-scale-test-argocd-app.txt) | Shows ArgoCD application status after GitOps scale test |
| [gitops-scale-test-deployment.txt](gitops-scale-test-deployment.txt) | Shows `demo-nginx` deployment scaled to 3 replicas through GitOps |
| [gitops-scale-test-pods.txt](gitops-scale-test-pods.txt) | Shows 3 running `demo-nginx` pods after GitOps reconciliation |

---

## Repository Files Added

| Path | Purpose |
|---|---|
| [../../kubernetes/namespace.yaml](../../kubernetes/namespace.yaml) | Namespace manifest for `demo` |
| [../../kubernetes/deployment.yaml](../../kubernetes/deployment.yaml) | Deployment manifest for `demo-nginx` |
| [../../kubernetes/service.yaml](../../kubernetes/service.yaml) | NodePort service manifest for `demo-nginx` |
| [../../argocd/applications/demo-nginx-app.yaml](../../argocd/applications/demo-nginx-app.yaml) | ArgoCD Application manifest |

---

## Validated Components

Phase 2 validates the following components:

- ArgoCD namespace
- ArgoCD application controller
- ArgoCD repo server
- ArgoCD API/UI server
- ArgoCD Redis
- ArgoCD ApplicationSet controller
- ArgoCD NodePort access
- GitHub repository as source of truth
- Kubernetes manifest sync from Git
- `demo-nginx` deployment health
- `demo-nginx` service availability

---

## ArgoCD Application State

The `demo-nginx` ArgoCD Application validates that ArgoCD can read the GitHub repository, generate manifests from the `kubernetes/` path, compare desired state against live cluster state, and sync resources into the `demo` namespace.

Expected application status:

```text
SYNC STATUS:   Synced
HEALTH STATUS: Healthy
```

The synced resources include:

- Namespace: `demo`
- Deployment: `demo-nginx`
- Service: `demo-nginx`

---

## Why This Matters

GitOps improves deployment maturity by making Git the source of truth for Kubernetes desired state.

This provides:

- Better audit trail
- Clear desired-state management
- Reduced manual deployment drift
- Easier rollback through Git history
- Better separation between CI and CD
- Stronger operational control
- Continuous reconciliation between Git and the cluster

---

## Operational Value

This phase demonstrates that the platform can move from manual or pipeline-driven `kubectl apply` deployments toward a GitOps model.

In a production-style environment, this matters because:

- Changes are reviewed through Git
- Desired state is version-controlled
- Rollbacks can be performed using Git history
- Cluster drift can be detected
- ArgoCD provides visibility into sync and health status
- Deployment status is easier to audit

---

## Security and Reliability Notes

Important operational considerations:

- ArgoCD admin credentials must not be committed to Git.
- The initial ArgoCD admin secret should be removed after password rotation.
- Public repositories should not contain sensitive cluster credentials.
- Kubernetes manifests should avoid hardcoded secrets.
- GitOps does not replace security scanning; it complements CI/CD validation.
- ArgoCD should eventually be protected with proper authentication and network access controls.

---

## Validation Commands

Useful validation commands:

```bash
kubectl get pods -n argocd -o wide
kubectl get svc -n argocd
kubectl get application -n argocd
kubectl get application demo-nginx -n argocd -o yaml
kubectl get pods -n demo -o wide
kubectl get svc -n demo
kubectl get deployment demo-nginx -n demo -o wide
```

Expected result:

- ArgoCD pods are running
- ArgoCD server service is exposed through NodePort
- `demo-nginx` Application is `Synced` and `Healthy`
- `demo-nginx` pods are running
- `demo-nginx` service is available through NodePort

---

## Troubleshooting Notes

Common issues during Phase 2:

### ArgoCD Application Shows Unknown

Possible causes:

- Repository path does not exist
- ArgoCD repo-server cache is stale
- GitHub repository is unreachable
- Manifest generation failed
- Wrong `repoURL`
- Wrong `targetRevision`
- Wrong `path`

Useful commands:

```bash
kubectl describe application demo-nginx -n argocd
kubectl logs -n argocd deployment/argocd-repo-server --tail=100
kubectl logs -n argocd statefulset/argocd-application-controller --tail=100
```

### ArgoCD UI Not Accessible

Possible causes:

- Wrong NodePort
- ArgoCD server pod not ready
- Browser certificate warning
- Firewall or routing issue
- Wrong node IP

Useful commands:

```bash
kubectl get svc argocd-server -n argocd
kubectl get pods -n argocd -o wide
curl -k -I https://NODE_IP:HTTPS_NODEPORT
```

### Application OutOfSync

Possible causes:

- Live cluster state differs from Git
- Manual changes were made using `kubectl`
- Git manifests changed but not yet synced

Useful commands:

```bash
kubectl get application demo-nginx -n argocd
kubectl describe application demo-nginx -n argocd
```

---

## Interview Explanation

Phase 2 demonstrates that I can move from direct Kubernetes deployment to a GitOps-based delivery model.

Instead of manually applying manifests with `kubectl`, ArgoCD watches the Git repository and reconciles the cluster to the desired state stored in Git.

This is closer to a production-style delivery model because changes are reviewed, committed, tracked, synchronized, and recoverable through Git.

A strong interview explanation:

> In Phase 2, I implemented ArgoCD to introduce GitOps deployment control. The Kubernetes manifests are stored in GitHub under the `kubernetes/` directory, and ArgoCD manages the `demo-nginx` application from that desired state. This gives me a clear audit trail, sync status, health status, and a cleaner rollback path through Git history.

---

## Future Improvements

Recommended next improvements:

- Add ArgoCD project-level RBAC
- Add repository credentials management if private repos are used
- Add ArgoCD health checks and alerts
- Add ArgoCD dashboard screenshot evidence
- Add automated sync policy documentation
- Add manual approval workflow for production-style environments
- Add Argo Rollouts for canary deployment
- Add GitOps rollback runbook
- Add ApplicationSet examples
- Add separate dev/staging/prod overlays using Kustomize or Helm

---

## Summary

Phase 2 introduces ArgoCD-based GitOps deployment control into the homelab platform.

It proves that the platform can manage Kubernetes workloads from Git and validate sync/health status through ArgoCD.

This is an important step toward production-style DevOps/SRE maturity because it improves deployment control, auditability, rollback readiness, and desired-state management.
