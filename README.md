Hi, I'm Tengku Rizal 👋

I am an IT, Network Security, and Infrastructure Operations professional with hands-on experience in network security operations, firewall change management, vulnerability review, compliance support, incident management, infrastructure operations, service delivery, and stakeholder coordination.

I am currently positioning myself for mid-level DevOps / DevSecOps / Infrastructure Security / Platform Engineering roles by building a full-stack DevSecOps homelab that covers infrastructure, network segmentation, CI/CD, Kubernetes deployment, security scanning, observability, SIEM monitoring, SOAR automation, and incident ticketing.

My goal is to bridge traditional infrastructure and security operations with modern DevSecOps automation.

---

## 🎯 Career Target

I am targeting mid-level roles in:

- DevOps Engineering
- DevSecOps Engineering
- Platform Engineering
- Infrastructure Security Engineering
- Security Automation Engineering
- Cloud and Security Operations
- SOC Automation / SIEM Engineering
- Kubernetes / Container Platform Operations

---

🔐 Current Focus

- DevSecOps automation
- GitLab CI/CD
- GitLab Runner operations
- GitLab Kubernetes Agent
- Docker container build and deployment
- Kubernetes deployment and operations
- Kubernetes monitoring and observability
- Container image vulnerability scanning
- SBOM generation
- Kubernetes configuration scanning
- Wazuh SIEM monitoring
- Shuffle SOAR automation
- GitHub incident ticketing
- Prometheus and Grafana monitoring
- Infrastructure and network security
- Firewall rule management
- VLAN segmentation
- Incident response workflow automation



🧪 Featured Homelab Project

DevSecOps Homelab: CI/CD + Kubernetes + Monitoring + SIEM + SOAR + Incident Automation

I built an end-to-end DevSecOps homelab that demonstrates secure software delivery, Kubernetes deployment, infrastructure monitoring, SIEM alerting, SOAR automation, and automated incident ticket creation.

The homelab simulates a real-world DevSecOps workflow where code is built, scanned, deployed, monitored, alerted, and converted into an incident ticket automatically.

Developer Push

  ↓

GitLab CI/CD

  ↓

Docker Image Build

  ↓

Container Registry Push

  ↓

Trivy Image Vulnerability Scan

  ↓

SBOM Generation with Syft

  ↓

Kubernetes Configuration Scan

  ↓

Kubernetes Deployment via GitLab Agent

  ↓

Kubernetes Rollout Verification

  ↓

Prometheus + Grafana Monitoring

  ↓

Wazuh SIEM Alert Detection

  ↓

High-Severity Alert Forwarding

  ↓

Shuffle SOAR Webhook

  ↓

HTTP Response Action

  ↓

GitHub Incident Issue Created Automatically

```

🧱 Homelab Environment

My DevSecOps homelab is built on a segmented, multi-node infrastructure designed to simulate enterprise-style platform, network, and security operations.

Infrastructure Layer

* Proxmox virtualization
* Multiple mini PCs as physical lab nodes
* pfSense physical firewall
* MikroTik home router
* WireGuard remote access
* VLAN segmentation
* Inter-VLAN routing
* Firewall rules
* SPAN / traffic mirroring design
* Segmented SOC, target, and attacker networks

Network Segments

SOC Network
Target Network
Attacker Network
Management / Home Network
WireGuard Remote Access Network

Platform Layer

* kubeadm-based Kubernetes cluster
* Kubernetes control plane node
* Kubernetes worker nodes
* Calico CNI
* Local Path Provisioner
* metrics-server
* Helm
* NodePort service exposure
* GitLab Kubernetes Agent
* Kubernetes deployment verification

DevSecOps Layer

* GitLab CE
* GitLab Runner
* GitLab CI/CD pipelines
* Docker image build
* GitLab Container Registry
* Trivy vulnerability scanning
* Syft SBOM generation
* Kubernetes config scanning
* Automated deployment to Kubernetes
* Pipeline artifact generation
* Rollout verification

Monitoring and Observability Layer

* Prometheus
* Grafana
* kube-prometheus-stack
* metrics-server
* Kubernetes pod and node visibility
* Application availability verification

Security Monitoring and Automation Layer

* Wazuh SIEM
* Wazuh agents
* Windows endpoint monitoring
* Linux endpoint monitoring
* Active Directory event monitoring
* Shuffle SOAR
* Shuffle webhook integration
* HTTP automation action
* GitHub Issues as incident ticketing
* Severity-based alert forwarding
* Automated incident ticket creation

Security Testing Layer

* Kali Linux attacker machine
* Ubuntu DVWA target
* Windows Server Active Directory
* Windows client endpoint
* Security Onion
* Docker host / container workloads

🛠️ Tech Stack

Area                Tools / Setup

Virtualization                Proxmox

Network Security              pfSense, MikroTik, VLANs, firewall rules, WireGuard

Network Monitoring            SPAN / traffic mirroring design

Kubernetes                    kubeadm, kubectl, Helm, Calico CNI, Local Path Provisioner

Kubernetes Access             GitLab Kubernetes Agent

CI/CD                         GitLab CE, GitLab CI/CD, GitLab Runner

Containers                    Docker

Container Registry            GitLab Container Registry

Security Scanning             Trivy

SBOM                          Syft

Kubernetes Config Security    Trivy config scan

Monitoring & Observability    Prometheus, Grafana, kube-prometheus-stack, metrics-server

SIEM                          Wazuh

SOAR                          Shuffle

Incident Tracking             GitHub Issues

Security Lab                  Kali Linux, DVWA, Windows Server AD, Windows client, Security Onion

Operating Systems             Ubuntu Linux, Windows Server, Windows Client, Kali Linux

Security Operations           Incident response, alert triage, vulnerability review, firewall change management

📌 Portfolio Projects

1. DevSecOps CI/CD Pipeline

Built a GitLab CI/CD pipeline that performs container build, image push, vulnerability scanning, SBOM generation, Kubernetes config scanning, deployment, and verification.

Key capabilities:

* Docker image build
* Push to GitLab Container Registry
* Trivy image vulnerability scan
* Syft SBOM generation
* Kubernetes config scanning
* Kubernetes deployment
* Rollout verification
* Pipeline artifact generation

⸻

2. Kubernetes Homelab Cluster

Built and operated a kubeadm-based Kubernetes cluster in a homelab environment.

Key capabilities:

* Multi-node Kubernetes cluster
* Calico CNI
* Local Path Provisioner
* metrics-server
* Helm chart deployment
* NodePort service exposure
* GitLab Kubernetes Agent integration
* Application deployment and verification

⸻

3. Monitoring and Observability Stack

Deployed monitoring components to provide visibility into Kubernetes workloads and cluster health.

Key capabilities:

* Prometheus monitoring
* Grafana dashboard visibility
* kube-prometheus-stack deployment
* metrics-server setup
* Pod and node monitoring
* Application availability testing

⸻

4. Wazuh SIEM Monitoring

Implemented Wazuh SIEM to monitor Linux, Windows, and Active Directory lab systems.

Key capabilities:

* Wazuh manager setup
* Wazuh agent monitoring
* Linux log monitoring
* Windows event monitoring
* Active Directory security event monitoring
* Alert generation
* Severity-based forwarding

⸻

5. Shuffle SOAR Automation

Integrated Wazuh with Shuffle SOAR using webhooks.

Key capabilities:

* Wazuh to Shuffle webhook integration
* Shuffle workflow creation
* HTTP response action
* Alert payload processing
* Automated security workflow execution

⸻

6. GitHub Incident Ticket Automation

Automated incident ticket creation using GitHub Issues when high-severity Wazuh alerts are received.

Key capabilities:

* GitHub API integration
* Fine-grained GitHub token usage
* HTTP POST from Shuffle
* Automated issue creation
* Incident labels
* Alert severity, rule ID, timestamp, and details included
* Triage checklist support

⸻

7. Network Security Lab

Built a segmented network security lab using pfSense and MikroTik.

Key capabilities:

* pfSense firewall
* MikroTik routing
* VLAN segmentation
* Inter-VLAN routing
* Firewall policy testing
* WireGuard remote access
* SOC, target, and attacker network separation
* SPAN / traffic mirroring planning

⸻

8. Security Testing Lab

Built a security testing environment to simulate attacker, target, and monitoring workflows.

Key components:

* Kali Linux attacker VM
* Ubuntu DVWA target VM
* Windows Server Active Directory
* Windows client endpoint
* Security Onion
* Wazuh monitored endpoints

9. Incident Response Workflow

Built a workflow that turns security alerts into actionable incident tickets.

Workflow:

Wazuh Alert Level 10+
  ↓
Shuffle Webhook
  ↓
HTTP Action
  ↓
GitHub Issue
  ↓
Triage Checklist

Example incident data included:

* Alert title
* Severity
* Rule ID
* Timestamp
* Alert details
* Labels
* Triage checklist

✅ Completed Milestones

* Built segmented Proxmox homelab infrastructure
* Configured pfSense firewall
* Configured MikroTik routing and WireGuard access
* Designed SOC, target, and attacker lab segmentation
* Built kubeadm Kubernetes cluster
* Installed Calico CNI
* Installed Local Path Provisioner
* Installed metrics-server
* Installed kube-prometheus-stack
* Verified Grafana access
* Integrated GitLab CI/CD with Kubernetes
* Configured GitLab Runner
* Configured GitLab Kubernetes Agent
* Built Docker image pipeline
* Pushed image to GitLab Container Registry
* Generated SBOM using Syft
* Scanned container image using Trivy
* Scanned Kubernetes config
* Deployed demo application to Kubernetes
* Verified Kubernetes rollout
* Verified NodePort application access
* Installed and configured Wazuh SIEM
* Monitored Linux and Windows endpoints
* Integrated Wazuh alerts with Shuffle SOAR
* Configured Wazuh alert level filtering
* Built Shuffle webhook workflow
* Configured Shuffle HTTP action
* Integrated Shuffle with GitHub Issues API
* Created automated GitHub incident tickets from Wazuh alerts
* Validated end-to-end DevSecOps security automation workflow

⸻

🧠 Troubleshooting Experience

During the homelab build, I solved multiple real-world issues including:

* GitLab Runner disk space exhaustion
* Trivy Java DB download failure
* Trivy cache cleanup
* Docker cache cleanup
* Pipeline artifact upload issues
* GitLab CI YAML indentation and stage errors
* Kubernetes rollout verification
* Kubeconfig and certificate issues
* Wazuh integration configuration
* Shuffle webhook testing
* HTTP API request failures
* GitHub token permission issues
* GitHub API status code troubleshooting
* Wazuh alert severity filtering

⸻

📈 What This Homelab Demonstrates

This homelab demonstrates my ability to work across multiple DevSecOps domains:

* Infrastructure operations
* Network security
* CI/CD automation
* Kubernetes operations
* Container security
* Vulnerability scanning
* SBOM generation
* Monitoring and observability
* SIEM monitoring
* SOAR automation
* Incident response automation
* API integration
* Troubleshooting and root cause analysis

⸻

🔄 Current Improvement Roadmap

Next improvements planned:

* Add Terraform infrastructure provisioning
* Add Ansible configuration automation
* Add secrets scanning
* Add policy-as-code using OPA / Conftest
* Add Prometheus alerting rules
* Add Grafana dashboards documentation
* Add Security Onion traffic analysis documentation
* Add automated email or chat notification
* Add incident response runbooks
* Add more GitHub documentation and architecture diagrams

⸻

📫 Contact

Open to mid-level opportunities in:

* DevOps Engineering
* DevSecOps Engineering
* Infrastructure Security
* Platform Engineering
* Security Automation
* Cloud and Security Operations
* SOC Automation

