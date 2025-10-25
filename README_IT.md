# 🏗️ Configurazione Datacenter – PrimeOps Cloud Hybrid Lab

**Lingue:** [🇧🇷 Portoghese](./README.md) | [🇬🇧 Inglese](./README_EN.md) | [🇮🇹 Italiano](./README_IT.md)

---

## 📂 Sottoprogetti e Laboratori PrimeOps

Esplora i moduli che compongono il Datacenter PrimeOps:

| Modulo | Descrizione | Repository |
|--------|--------------|-------------|
| 🧱 **Core Infrastructure** | Fondamento del datacenter – Proxmox, rete, VLAN e storage. | [→ Vedi progetto](./core-infrastructure/README.md) |
| 🔐 **Identity & Access (IAM)** | Active Directory, MFA, RBAC, Zero Trust. | [→ Vedi progetto](./iam/README.md) |
| ⚙️ **DevSecOps Pipeline** | CI/CD sicuro, scanner e distribuzione automatizzata. | [→ Vedi progetto](./devsecops/README.md) |
| 🛡️ **Security & SOC** | Wazuh, TheHive, SOAR e monitoraggio continuo. | [→ Vedi progetto](./soc/README.md) |
| ☁️ **CloudBridge** | Integrazione ibrida con AWS, Azure e GCP. | [→ Vedi progetto](./cloudbridge/README.md) |
| 🌐 **Web & DMZ Services** | Server web, API e sicurezza perimetrale. | [→ Vedi progetto](./dmz/README.md) |
| 🐧 **Linux Engineering Lab** | Hardening, automazione e amministrazione Linux. | [→ Vedi progetto](./linuxlab/README.md) |
| 🎯 **Red Team Offensive Lab** | Penetration test e simulazioni offensive. | [→ Vedi progetto](./offensive-lab/README.md) |
| 🧩 **Blue Team Defense Lab** | Analisi forense digitale e risposta agli incidenti. | [→ Vedi progetto](./defense-lab/README.md) |
| 🎥 **Docs & Media Studio** | Documentazione tecnica e materiale audiovisivo. | [→ Vedi progetto](./docs-studio/README.md) |

---

## 📘 Panoramica

Il **Datacenter PrimeOps** è stato progettato come un **ambiente ibrido di apprendimento, automazione e sicurezza**, utilizzando **Proxmox VE** come hypervisor principale.  
L'obiettivo è fornire un'infrastruttura completa per **laboratori di rete, cybersecurity, cloud computing e automazione**, integrando ambienti **on-premises e cloud pubblici (AWS, Azure e Google Cloud)**.

Questo documento descrive la **struttura fisica e logica**, oltre ai **passaggi di installazione, configurazione e buone pratiche**.

---

## 🧩 Struttura del Progetto

| Componente | Funzione Principale | Sistema | Stato |
|-------------|--------------------|----------|--------|
| **Proxmox VE** | Hypervisor principale | Debian 12 | ✅ |
| **VM - Windows Server** | Active Directory, DNS, GPO | WinSrv 2022 | ✅ |
| **VM - Windows 11** | Workstation amministrativa | Win11 | ✅ |
| **VM - Linux GitLab** | Server di repository e CI/CD | Ubuntu 24.04 | ✅ |
| **VM - PFSense** | Firewall, VLAN, VPN e regole di sicurezza | FreeBSD | ✅ |
| **VM - SIEM/SOAR** | Monitoraggio, correlazione log e automazione | Wazuh + TheHive | 🔄 |
| **VM - Web Server** | Nginx + PHP + Database | Debian | ✅ |
| **VM - Cloud Gateway** | Bridge con servizi esterni (AWS, GCP, Azure) | Debian | 🔄 |

---

## ⚙️ Configurazione Hardware

| Risorsa | Specifica |
|----------|------------|
| Host Fisico | Intel Xeon 8C/16T, 32 GB RAM |
| Storage | SSD 250 GB (sistema) + 2x HDD 1 TB (dati e VM) |
| Rete | 2 interfacce Gigabit + switch gestito |
| Virtualizzazione | Proxmox VE 8.2 |

---

## 🌐 Tabella VLAN e Subnet

| VLAN | Settore / Funzione | Intervallo IP | Gateway | Note |
|------|--------------------|----------------|----------|------|
| 10 | Amministrazione | 10.10.10.0/24 | 10.10.10.1 | Server di gestione |
| 20 | Utenti / Accesso | 10.10.20.0/24 | 10.10.20.1 | Workstation Windows e Linux |
| 30 | Stampanti | 10.10.30.0/24 | 10.10.30.1 | VLAN isolata |
| 40 | IoT / Dispositivi | 10.10.40.0/24 | 10.10.40.1 | Sensori e dispositivi IoT |
| 50 | Sicurezza / SOC | 10.10.50.0/24 | 10.10.50.1 | SIEM, SOAR, Wazuh |
| 60 | DMZ / Web / VPN | 10.10.60.0/24 | 10.10.60.1 | Server pubblici |

---

## 🗺️ Diagramma Logico (Concettuale)

```text
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

💻 Tabella Interfacce PFSense
Interfaccia	VLAN	IP	Funzione
LAN0	10	10.10.10.1	Amministrazione
LAN1	20	10.10.20.1	Utenti
LAN2	30	10.10.30.1	Stampanti
LAN3	40	10.10.40.1	IoT
LAN4	50	10.10.50.1	SOC / SIEM
WAN	60	DHCP / Pubblico	DMZ / VPN Esterna
🔐 VPN e Sicurezza

    VPN Site-to-Site tra PFSense e gateway esterni (Fortinet / AWS / Azure).

    VPN Client (OpenVPN) per accesso remoto sicuro degli amministratori.

    Politica Zero Trust implementata su più livelli:

        Controllo identità (AD + MFA)

        Segmentazione tramite VLAN

        Monitoraggio continuo (Wazuh)

        SIEM/SOAR con avvisi e automazione

    Regole firewall basate su privilegi minimi e politiche stratificate.

🧠 Laboratori di Sicurezza e Audit
Tipo di Lab	Strumenti	Obiettivo
Offensivo (Red Team)	Kali Linux, Metasploit, Nmap	Test di penetrazione e valutazione vulnerabilità
Difensivo (Blue Team)	Wazuh, Suricata, OSSEC	Monitoraggio e risposta agli incidenti
Audit & Log	Elastic Stack, Graylog	Correlazione e analisi forense
SOAR	TheHive, Cortex, MISP	Automazione della risposta agli incidenti
⚡ Procedure di Installazione
1. Installazione di Proxmox VE

ISO ufficiale: https://www.proxmox.com/en/downloads

sudo apt update && sudo apt upgrade -y  
sudo apt install proxmox-ve postfix open-iscsi -y

2. Creazione delle VM

    Nome standard: PXD-SRV-<funzione>

    Disco di sistema su SSD, dati su HDD

    Rete configurata tramite vmbr0 con VLAN tag

3. Installazione di PFSense

    Interfaccia WAN = VLAN60

    Interfacce LAN create secondo la tabella VLAN

    Abilitare NAT e DHCP per ogni subnet

4. VPN Site-to-Site

Configurazione tunnel IPSec tra PFSense e gateway Fortinet/AWS:

Phase 1: AES256 / SHA256 / DH14  
Phase 2: AES256GCM / PFS Group 14

5. Distribuzione di Wazuh (SIEM)

curl -sO https://packages.wazuh.com/4.x/wazuh-install.sh  
bash wazuh-install.sh -a

👨‍💻 Sviluppato da

PrimeOps – Infrastruttura, Automazione e Cybersecurity  
Sviluppo e Documentazione: André Gonçalvez  

> “L’automazione non riguarda la sostituzione delle persone — ma la liberazione del potenziale umano per risolvere problemi più complessi.”
