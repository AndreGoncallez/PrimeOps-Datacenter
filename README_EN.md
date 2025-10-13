# 🏗️ Datacenter Setup – PrimeOps Cloud Hybrid Lab

[🇧🇷 Português](./README.md) | [🇬🇧 English](./README.en.md) | [🇮🇹 Italiano](./README.it.md)

---

## 📂 PrimeOps Subprojects and Labs (English)

Explore the modules that make up the PrimeOps Datacenter ecosystem:

| Module                         | Description                                                      | Repository                                        |
| ------------------------------ | ---------------------------------------------------------------- | ------------------------------------------------- |
| 🧱 **Core Infrastructure**     | Datacenter foundation – Proxmox, networking, VLANs, and storage. | [→ View Project](./core-infrastructure/README.md) |
| 🔐 **Identity & Access (IAM)** | Active Directory, MFA, RBAC, Zero Trust.                         | [→ View Project](./iam/README.md)                 |
| ⚙️ **DevSecOps Pipeline**      | Secure CI/CD, scanners, and automated deployments.               | [→ View Project](./devsecops/README.md)           |
| 🛡️ **Security & SOC**         | Wazuh, TheHive, SOAR, and continuous monitoring.                 | [→ View Project](./soc/README.md)                 |
| ☁️ **CloudBridge**             | Hybrid integration with AWS, Azure, and GCP.                     | [→ View Project](./cloudbridge/README.md)         |
| 🌐 **Web & DMZ Services**      | Web servers, APIs, and perimeter security.                       | [→ View Project](./dmz/README.md)                 |
| 🐧 **Linux Engineering Lab**   | Hardening, automation, and Linux administration.                 | [→ View Project](./linuxlab/README.md)            |
| 🎯 **Red Team Offensive Lab**  | Penetration testing and offensive simulations.                   | [→ View Project](./offensive-lab/README.md)       |
| 🧩 **Blue Team Defense Lab**   | Digital forensics and incident response.                         | [→ View Project](./defense-lab/README.md)         |
| 🎥 **Docs & Media Studio**     | Technical documentation and multimedia production.               | [→ View Project](./docs-studio/README.md)         |

---

# 📘 Overview

The PrimeOps Datacenter is designed as a hybrid environment for study, automation and security, using Proxmox VE as the primary hypervisor.

The objective is to provide a complete infrastructure for networking labs, cybersecurity, cloud computing and automation, integrating on-premises environments with public cloud (AWS, Azure and Google Cloud).

This document describes the physical and logical structure, as well as a step-by-step installation, configuration and recommended practices.

---

## 🧩 Project Structure

| Component           | Primary Function                                    | System          | Status |
| ------------------- | --------------------------------------------------- | --------------- | ------ |
| Proxmox VE          | Primary hypervisor                                  | Debian 12       | ✅      |
| VM - Windows Server | Active Directory, DNS, GPO                          | WinSrv 2022     | ✅      |
| VM - Windows 11     | Administrative workstation                          | Win11           | ✅      |
| VM - Linux GitLab   | Repository and CI/CD server                         | Ubuntu 24.04    | ✅      |
| VM - PFSense        | Firewall, VLANs, VPNs and security rules            | FreeBSD         | ✅      |
| VM - SIEM/SOAR      | Monitoring, log correlation and automation          | Wazuh + TheHive | 🔄     |
| VM - Web Server     | Nginx + PHP + Database                              | Debian          | ✅      |
| VM - Cloud Gateway  | Bridge to external cloud services (AWS, GCP, Azure) | Debian          | 🔄     |

---

## ⚙️ Hardware Configuration

| Resource       | Specification                                |
| -------------- | -------------------------------------------- |
| Physical Host  | Intel Xeon 8C/16T, 32 GB RAM                 |
| Storage        | SSD 250 GB (OS) + 2x HDD 1 TB (data and VMs) |
| Network        | 2 Gigabit interfaces + managed switch        |
| Virtualization | Proxmox VE 8.2                               |

---

## 🌐 VLANs and Subnet Table

| VLAN | Sector / Function | IP Range      | Gateway    | Notes                          |
| ---- | ----------------- | ------------- | ---------- | ------------------------------ |
| 10   | Administration    | 10.10.10.0/24 | 10.10.10.1 | Management servers             |
| 20   | Users / Access    | 10.10.20.0/24 | 10.10.20.1 | Windows and Linux workstations |
| 30   | Printers          | 10.10.30.0/24 | 10.10.30.1 | Isolated VLAN                  |
| 40   | IoT / Devices     | 10.10.40.0/24 | 10.10.40.1 | Sensors and IoT                |
| 50   | Security / SOC    | 10.10.50.0/24 | 10.10.50.1 | SIEM, SOAR, Wazuh              |
| 60   | DMZ / Web / VPN   | 10.10.60.0/24 | 10.10.60.1 | Public-facing servers          |

---

## 🗺️ Logical Diagram (conceptual)

```
                  +-------------------+
                  |    Cloud Hybrid   |
                  | AWS / GCP / Azure |
                  +---------+---------+
                            |
                     +------v------+
                     | Cloud GW VM |
                     +-------------+
                            |
       +-----------------------------------------------------+
       |                 PFSense Firewall                    |
       +-----------------------------------------------------+
       | VLAN10 | VLAN20 | VLAN30 | VLAN40 | VLAN50 | VLAN60 |
       |   AD    |  Users | Printers|  IoT  |  SOC   |  DMZ  |
       +-----------------------------------------------------+
```

---

## 💻 PFSense Interfaces Table

| Interface | VLAN | IP            | Function           |
| --------- | ---- | ------------- | ------------------ |
| LAN0      | 10   | 10.10.10.1    | Administration     |
| LAN1      | 20   | 10.10.20.1    | Users              |
| LAN2      | 30   | 10.10.30.1    | Printers           |
| LAN3      | 40   | 10.10.40.1    | IoT                |
| LAN4      | 50   | 10.10.50.1    | SOC / SIEM         |
| WAN       | 60   | DHCP / Public | DMZ / External VPN |

---

## 🔐 VPN and Security

* Site-to-Site VPN between PFSense and external gateways (Fortinet / AWS / Azure).
* Client VPN (OpenVPN) for secure remote administrator access.

Policy: Zero Trust implemented in layers:

* Identity control (AD + MFA)
* Segmentation via VLANs
* Continuous monitoring with Wazuh
* SIEM/SOAR with alerts and automation

Firewall rules organized by layers and least-privilege policies.

---

## 🧠 Security and Audit Labs

| Lab Type            | Tools                        | Objective                                        |
| ------------------- | ---------------------------- | ------------------------------------------------ |
| Attack (Red Team)   | Kali Linux, Metasploit, Nmap | Penetration testing and vulnerability assessment |
| Defense (Blue Team) | Wazuh, Suricata, OSSEC       | Monitoring and incident response                 |
| Audit & Logs        | Elastic Stack, Graylog       | Correlation and forensic analysis                |
| SOAR                | TheHive, Cortex, MISP        | Automated incident response                      |

---

## ⚡ Installation Procedures

### 1. Install Proxmox VE

Official ISO: [https://www.proxmox.com/en/downloads](https://www.proxmox.com/en/downloads)

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install proxmox-ve postfix open-iscsi -y
```

### 2. Create VMs

* Standard naming: `PXD-SRV-<function>`
* Use SSD for OS and HDD for VM data
* Network configured via `vmbr0` with VLAN tagging

### 3. Install PFSense

* WAN interface = VLAN60
* Create LAN interfaces according to VLAN table
* Enable NAT and DHCP per subnet

### 4. Site-to-Site VPN

IPSec tunnel configuration with Fortinet/AWS:

* Phase 1: AES256 / SHA256 / DH14
* Phase 2: AES256GCM / PFS Group 14

### 5. Deploy Wazuh (SIEM)

```bash
curl -sO https://packages.wazuh.com/4.x/wazuh-install.sh
bash wazuh-install.sh -a
```

---

## 👨‍💻 Developed by

PrimeOps – Infrastructure, Automation and Cybersecurity

Development and documentation: André Gonçalvez
