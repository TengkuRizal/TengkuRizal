# Phase 1 Homelab Baseline Validation Evidence

## Overview

This folder contains Phase 1 baseline validation evidence for the homelab DevSecOps/SRE platform.

The purpose of Phase 1 is to prove that the current platform is stable, observable, and ready for further DevOps/SRE improvements before adding more advanced components such as GitOps, progressive delivery, centralized logging, SLOs, or chaos engineering.

---

## Phase 1 Objective

Validate the current operational baseline:

- Kubernetes cluster health
- Workload status
- Service exposure
- Deployment availability
- Monitoring stack health
- Prometheus and Alertmanager availability
- Git repository state

---

## Evidence Files

| File | Purpose |
|---|---|
| [kubernetes-nodes.txt](kubernetes-nodes.txt) | Shows Kubernetes node readiness and version details |
| [kubernetes-pods.txt](kubernetes-pods.txt) | Shows all running pods across namespaces |
| [kubernetes-services.txt](kubernetes-services.txt) | Shows Kubernetes services and exposed ports |
| [kubernetes-deployments.txt](kubernetes-deployments.txt) | Shows deployment readiness and availability |
| [kubernetes-events.txt](kubernetes-events.txt) | Captures recent Kubernetes events |
| [monitoring-pods.txt](monitoring-pods.txt) | Shows monitoring stack pod status |
| [monitoring-services.txt](monitoring-services.txt) | Shows monitoring services such as Grafana, Prometheus, and Alertmanager |
| [prometheus-rules.txt](prometheus-rules.txt) | Shows Prometheus alert rules configured in the cluster |
| [prometheus.txt](prometheus.txt) | Shows Prometheus custom resources |
| [alertmanager.txt](alertmanager.txt) | Shows Alertmanager custom resources |
| [git-status.txt](git-status.txt) | Shows Git working tree state at evidence capture time |
| [git-history.txt](git-history.txt) | Shows recent Git commit history |

---

## Validated Components

Phase 1 validates that the following platform components are running:

- Kubernetes control plane
- Kubernetes worker nodes
- Demo application workload
- Ingress NGINX
- Prometheus monitoring stack
- Grafana
- Alertmanager
- kube-state-metrics
- node-exporter
- metrics-server
- Falco runtime security
- Kyverno policy engine
- Vault

---

## Summary

This evidence proves that the homelab platform has a healthy operational baseline.

It provides a known-good state before implementing the next phase of improvements.
