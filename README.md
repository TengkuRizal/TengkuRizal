# Hi, I'm Tengku Rizal 👋

**DevSecOps Engineer** — I build secure CI/CD pipelines, automate threat response, and run enterprise-style security operations from a bare-metal homelab.

15+ years in network security, firewall operations, and vulnerability management — now applied to modern DevSecOps engineering.

---

## 🔐 What I Build

```mermaid
flowchart LR
    A([Code Push]) --> B[🔍 Gitleaks\nSecrets Scan]
    B --> C[🔬 Semgrep\nSAST]
    C --> D[📦 Trivy\nImage Scan + SBOM]
    D --> E[🚀 K8s Deploy\n+ Verify]
    
    B -->|secret found| F([❌ Blocked])
    C -->|critical finding| F
    D -->|Critical CVE| F
    E --> G([✅ Live])
```

Pipeline enforces policy — builds are **blocked**, not just reported.

---

## 🧪 Homelab

```mermaid
graph LR
    subgraph SOC["VLAN 10 — SOC"]
        GL[GitLab CE] --> GR[Runner]
        GR --> K8[K8s Cluster]
        WZ[Wazuh SIEM] --> SH[Shuffle SOAR]
        SH --> GH[GitHub Issues]
    end
    subgraph Targets["VLAN 20 — Targets"]
        DC[Windows AD]
        DVWA[DVWA]
    end
    subgraph Attack["VLAN 30 — Attacker"]
        KL[Kali Linux]
    end
    KL -->|simulated attacks| DC
    DC -->|alerts| WZ
    PF[pfSense] --> SOC & Targets & Attack
    SO[Security Onion\nZeek] -->|SPAN port| PF
```

4 Mini PC nodes · Proxmox · Cisco 2960 SPAN · WireGuard remote access

---

## 🛠 Tech Stack

**CI/CD & Security Gates**

![GitLab CI](https://img.shields.io/badge/GitLab_CI-FC6D26?style=flat&logo=gitlab&logoColor=white)
![Gitleaks](https://img.shields.io/badge/Gitleaks-F05033?style=flat&logo=git&logoColor=white)
![Semgrep](https://img.shields.io/badge/Semgrep-31AF91?style=flat&logo=semgrep&logoColor=white)
![Trivy](https://img.shields.io/badge/Trivy-1904DA?style=flat&logo=aqua&logoColor=white)

**Kubernetes & Containers**

![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=flat&logo=kubernetes&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat&logo=docker&logoColor=white)
![Helm](https://img.shields.io/badge/Helm-0F1689?style=flat&logo=helm&logoColor=white)
![Prometheus](https://img.shields.io/badge/Prometheus-E6522C?style=flat&logo=prometheus&logoColor=white)
![Grafana](https://img.shields.io/badge/Grafana-F46800?style=flat&logo=grafana&logoColor=white)

**SIEM & Security Automation**

![Wazuh](https://img.shields.io/badge/Wazuh-005571?style=flat&logo=elastic&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![Bash](https://img.shields.io/badge/Bash-4EAA25?style=flat&logo=gnubash&logoColor=white)

**Network & Infrastructure**

![pfSense](https://img.shields.io/badge/pfSense-212121?style=flat)
![Proxmox](https://img.shields.io/badge/Proxmox-E57000?style=flat&logo=proxmox&logoColor=white)
![Cisco](https://img.shields.io/badge/Cisco-1BA0D7?style=flat&logo=cisco&logoColor=white)

---

## 📁 Projects

| Repo | What it does |
|---|---|
| 🔒 [devsecops-homelab](https://github.com/TengkuRizal/devsecops-homelab) | Full homelab — pipeline, K8s, SIEM, SOAR, network segmentation |
| 🐍 [wazuh-triage](https://github.com/TengkuRizal/wazuh-triage) *(coming soon)* | Python automation — Wazuh REST API alert triage + structured reporting |

---

## 📈 Currently Adding

- [ ] Terraform + Checkov — IaC security scanning
- [ ] Falco — Kubernetes runtime threat detection  
- [ ] HashiCorp Vault — secrets lifecycle management

---

## 📫 Contact

Open to **DevSecOps** and **Security Engineering** roles in Malaysia.

[![Email](https://img.shields.io/badge/Email-EA4335?style=flat&logo=gmail&logoColor=white)](mailto:myserv@gmail.com)
