# Tengku Rizal — DevSecOps Engineer

Kuala Lumpur, Malaysia · Building security into every stage of the pipeline

![DevSecOps](https://img.shields.io/badge/Focus-DevSecOps-blue)
![Kubernetes](https://img.shields.io/badge/Kubernetes-kubeadm-326CE5?logo=kubernetes)
![GitLab](https://img.shields.io/badge/GitLab-CI%2FCD-FC6D26?logo=gitlab)
![Terraform](https://img.shields.io/badge/Terraform-AWS-7B42BC?logo=terraform)
![Wazuh](https://img.shields.io/badge/SIEM-Wazuh-005571)
![Checkov](https://img.shields.io/badge/Checkov-61%20passed%2C%200%20failed-brightgreen)

---

## About

Transitioning from network security operations into DevSecOps engineering. I built a full-stack homelab that demonstrates end-to-end secure software delivery — from code commit to Kubernetes deployment with automated threat detection and incident response.

The homelab runs on 4 Mini PC nodes with Proxmox, segmented into SOC, Target, Attacker, and VPN zones via pfSense and MikroTik.

---

## Skills

**CI/CD & Pipeline Security**
GitLab CE · GitLab Runner · Gitleaks · Semgrep · Trivy · Syft · Docker · Kubernetes

**Infrastructure & IaC**
Terraform · Checkov · AWS (VPC, IAM, S3) · Proxmox · pfSense · MikroTik

**Security Operations**
Wazuh SIEM · Security Onion · Shuffle SOAR · MITRE ATT&CK · Zeek · Suricata

**Orchestration**
Kubernetes (kubeadm) · Helm · Calico CNI · Prometheus · Grafana

---

## Featured Projects

### 🔐 [gitlab-devsecops-pipeline](https://github.com/TengkuRizal/gitlab-devsecops-pipeline)
9-stage security pipeline on self-hosted GitLab — **Pipeline #76: 9/9 stages passed in 48s**

<pre>
validate → secret-scan → sast → config-scan → build → sbom → image-scan → deploy → verify
kubectl    Gitleaks      Semgrep  Trivy K8s    Docker   Syft    Trivy CVE    kubectl   kubectl
dry-run    secrets       SAST     manifests    build    SBOM    HIGH/CRIT    apply     status
</pre>

---

### ☁️ [terraform-aws-devsecops](https://github.com/TengkuRizal/terraform-aws-devsecops)
AWS infrastructure with Terraform — **Checkov: 61 passed, 0 failed**

- Multi-AZ VPC with public/private subnets
- Least privilege IAM (ARN-scoped policies)
- S3 with encryption, versioning, lifecycle
- VPC Flow Logs + default SG restricted
- S3 remote state with native lockfile

---

### 🏗️ [devsecops-homelab](https://github.com/TengkuRizal/devsecops-homelab)
Enterprise-style homelab — 4 zones, 7 physical/virtual devices

<pre>
Internet → Mikrotik (192.168.0.1) → pfSense → Cisco 2960 → VLANs
                                                    │
                                              SPAN ─┼─► Security Onion
                                                    │
                              VLAN 10 (SOC)  VLAN 20 (Target)  VLAN 30 (Attacker)
                              GitLab         Windows DC01       Kali Linux
                              Wazuh          DVWA               
                              K8s Cluster    
</pre>

**Security data flow:** Kali attack → Wazuh detects → Shuffle SOAR → Active response

---

## What I've Built

| ✅ | Project |
|---|---|
| ✅ | 9-stage GitLab CI/CD security pipeline (Gitleaks, Semgrep, Trivy, Syft, kubectl) |
| ✅ | 3-node Kubernetes cluster with kubeadm + Prometheus/Grafana monitoring |
| ✅ | AWS Terraform infrastructure — Checkov 61 passed, 0 failed |
| ✅ | Wazuh SIEM with MITRE ATT&CK detection + active response |
| ✅ | Security Onion network monitoring via Cisco 2960 SPAN port |
| ✅ | Shuffle SOAR + wazuh_triage.py automated alert triage |
| ✅ | Windows AD attack simulation detected end-to-end |
| ✅ | 4-zone network segmentation (SOC, Target, Attacker, VPN) |
| ✅ | Kubernetes cluster dengan Kyverno supply chain enforcement — 3 ClusterPolicies |

---

## Open To

Mid-level DevSecOps, DevOps, Platform Engineering, or Security Automation roles in Malaysia.

[![Email](https://img.shields.io/badge/Email-EA4335?style=flat&logo=gmail&logoColor=white)](mailto:myserv@gmail.com)
