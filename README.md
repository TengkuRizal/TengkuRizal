# Hi, I'm Tengku Rizal 👋

**DevSecOps Engineer** — I build secure CI/CD pipelines, automate threat response, and run enterprise-style security operations from a bare-metal homelab.

15+ years in network security, firewall operations, and vulnerability management — now applied to modern DevSecOps engineering.

---

## 🔐 CI/CD Security Pipeline

```mermaid
flowchart LR
    A([Code Push]) --> B[🔍 Gitleaks\nSecrets Scan]
    B --> C[🔬 Semgrep\nSAST]
    C --> D[📦 Trivy\nImage Scan + SBOM]
    D --> E[🚀 K8s Deploy\n+ Rollout Verify]

    B -->|secret found| F([❌ Blocked])
    C -->|critical finding| F
    D -->|Critical/High CVE| F
    E --> G([✅ Live in Cluster])
```

Pipeline enforces policy — builds are **blocked**, not just reported. Trivy configured with `exit-code: 1` on Critical/High severity.

---

## 🧪 Homelab Architecture

```mermaid
graph TB
    subgraph SOC["VLAN 10 — SOC (10.10.1.x)"]
        GL["GitLab CE\n10.10.1.101"]
        GR["GitLab Runner\n10.10.1.21"]
        WZ["Wazuh SIEM\n10.10.1.30"]
        K8["K8s Cluster\n10.10.1.70"]
        SO["Security Onion\n10.10.1.151"]
    end

    subgraph Targets["VLAN 20 — Targets (10.10.2.x)"]
        DC["Windows AD / DC01\n10.10.2.30"]
        DVWA["DVWA"]
    end

    subgraph Attack["VLAN 30 — Attacker (10.10.3.x)"]
        KL["Kali Linux\n10.10.3.20"]
    end

    PF["pfSense\nGateway + Firewall"] --> SOC & Targets & Attack
    SW["Cisco 2960\nSPAN Port"] -->|mirrored traffic| SO
    WZ -->|"webhook level 10+"| SH["Shuffle SOAR"]
    SH -->|"HTTP POST"| GH["GitHub Issues\nAuto-created"]
    GL --> GR --> K8
```

4 Mini PC i7 nodes · Proxmox · Cisco 2960 SPAN · WireGuard remote access

---

## 🛠 Tech Stack

**CI/CD & Pipeline Security**

![GitLab CI](https://img.shields.io/badge/GitLab_CI-FC6D26?style=flat&logo=gitlab&logoColor=white)
![GitLab Runner](https://img.shields.io/badge/GitLab_Runner-FC6D26?style=flat&logo=gitlab&logoColor=white)
![Gitleaks](https://img.shields.io/badge/Gitleaks-F05033?style=flat&logo=git&logoColor=white)
![Semgrep](https://img.shields.io/badge/Semgrep-31AF91?style=flat&logo=semgrep&logoColor=white)
![Trivy](https://img.shields.io/badge/Trivy-1904DA?style=flat&logo=aqua&logoColor=white)
![Syft](https://img.shields.io/badge/Syft_SBOM-6C3483?style=flat&logo=anchore&logoColor=white)

**Kubernetes & Containers**

![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=flat&logo=kubernetes&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat&logo=docker&logoColor=white)
![Helm](https://img.shields.io/badge/Helm-0F1689?style=flat&logo=helm&logoColor=white)
![containerd](https://img.shields.io/badge/containerd-575757?style=flat&logo=containerd&logoColor=white)
![Calico](https://img.shields.io/badge/Calico_CNI-FB8C00?style=flat&logo=linux&logoColor=white)

**Monitoring & Observability**

![Prometheus](https://img.shields.io/badge/Prometheus-E6522C?style=flat&logo=prometheus&logoColor=white)
![Grafana](https://img.shields.io/badge/Grafana-F46800?style=flat&logo=grafana&logoColor=white)
![kube-prometheus-stack](https://img.shields.io/badge/kube--prometheus--stack-326CE5?style=flat&logo=kubernetes&logoColor=white)

**SIEM, SOAR & Security Automation**

![Wazuh](https://img.shields.io/badge/Wazuh-005571?style=flat&logo=elastic&logoColor=white)
![Security Onion](https://img.shields.io/badge/Security_Onion-4CAF50?style=flat&logo=linux&logoColor=white)
![Zeek](https://img.shields.io/badge/Zeek-777BB4?style=flat&logo=zeek&logoColor=white)
![Shuffle](https://img.shields.io/badge/Shuffle_SOAR-FF6F00?style=flat&logo=zapier&logoColor=white)

**Automation & Scripting**

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![Bash](https://img.shields.io/badge/Bash-4EAA25?style=flat&logo=gnubash&logoColor=white)
![YAML](https://img.shields.io/badge/YAML-CB171E?style=flat&logo=yaml&logoColor=white)

**Network & Infrastructure**

![pfSense](https://img.shields.io/badge/pfSense-212121?style=flat&logo=pfsense&logoColor=white)
![Cisco](https://img.shields.io/badge/Cisco-1BA0D7?style=flat&logo=cisco&logoColor=white)
![MikroTik](https://img.shields.io/badge/MikroTik-293239?style=flat&logo=mikrotik&logoColor=white)
![WireGuard](https://img.shields.io/badge/WireGuard-88171A?style=flat&logo=wireguard&logoColor=white)
![Proxmox](https://img.shields.io/badge/Proxmox-E57000?style=flat&logo=proxmox&logoColor=white)
![Palo Alto](https://img.shields.io/badge/Palo_Alto-FA582D?style=flat&logo=paloaltonetworks&logoColor=white)
![Fortinet](https://img.shields.io/badge/Fortinet-EE3124?style=flat&logo=fortinet&logoColor=white)

---

## 📁 Featured Projects

### 🔒 [devsecops-homelab](https://github.com/TengkuRizal/devsecops-homelab)

End-to-end DevSecOps lab covering secure pipelines, Kubernetes, SIEM-driven SOAR automation, and network security monitoring.

| Component | What was built |
|---|---|
| **CI/CD Pipeline** | 4-stage security pipeline — Gitleaks → Semgrep → Trivy (enforced gate) → kubectl deploy |
| **SBOM** | Syft generates CycloneDX SBOM per build, stored as pipeline artifact |
| **Kubernetes** | 3-node kubeadm cluster — Calico CNI, containerd, local-path-provisioner, metrics-server |
| **Observability** | kube-prometheus-stack — Prometheus scraping cluster metrics, Grafana dashboards |
| **Wazuh SIEM** | Agents on Windows AD, Linux servers, Kali — full attack simulation with verified detections |
| **SOAR Automation** | Wazuh level 10+ → Shuffle webhook → GitHub Issue auto-created with triage checklist |
| **Python Triage** | `wazuh_triage.py` — queries Wazuh REST API, classifies alerts by severity + rule group, hourly cron |
| **Network Security** | Security Onion + Zeek via Cisco SPAN — wire-level traffic capture and analysis |
| **Network Segmentation** | pfSense VLAN 10/20/30 — SOC, Targets, Attacker isolated with enforced firewall policy |
| **Attack Simulation** | Kali vs Windows AD + DVWA — SSH brute force, AD recon, web attacks — all detected and correlated |

---

### 🐍 wazuh-triage *(coming soon)*

Standalone Python automation for Wazuh alert triage.

- Queries Wazuh REST API — no external dependencies, standard library only
- Classifies alerts by severity level and rule group
- Outputs structured triage report — top offenders, per-agent breakdown, top 5 critical alerts
- Runs hourly via cron

---

## 📈 Currently Adding

- [ ] Terraform + Checkov — IaC security scanning in pipeline
- [ ] Falco — Kubernetes runtime threat detection
- [ ] HashiCorp Vault — secrets lifecycle management
- [ ] GCP Security Command Center — cloud security posture

---

## 📫 Contact

Open to **DevSecOps** and **Security Engineering** roles in Malaysia.

[![Email](https://img.shields.io/badge/Email-EA4335?style=flat&logo=gmail&logoColor=white)](mailto:myserv@gmail.com)
