# 🏗️ Datacenter Setup – PrimeOps Cloud Hybrid Lab

**Idiomas:** [🇧🇷 Português](./README.md) | [🇬🇧 English](./README_EN.md) | [🇮🇹 Italiano](./README_IT.md)

---

## 📂 Subprojetos e Laboratórios PrimeOps

Explore os módulos que compõem o Datacenter PrimeOps:

| Módulo | Descrição | Repositório |
|--------|------------|--------------|
| 🧱 **Infraestrutura Core** | Fundação do datacenter – Proxmox, rede, VLANs e armazenamento. | [→ Ver projeto](./core-infrastructure/README.md) |
| 🔐 **Identidade & Acesso (IAM)** | Active Directory, MFA, RBAC, Zero Trust. | [→ Ver projeto](./iam/README.md) |
| ⚙️ **Pipeline DevSecOps** | CI/CD seguro, scanners e deploy automatizado. | [→ Ver projeto](./devsecops/README.md) |
| 🛡️ **Segurança & SOC** | Wazuh, TheHive, SOAR e monitoramento contínuo. | [→ Ver projeto](./soc/README.md) |
| ☁️ **CloudBridge** | Integração híbrida com AWS, Azure e GCP. | [→ Ver projeto](./cloudbridge/README.md) |
| 🌐 **Serviços Web & DMZ** | Servidores web, APIs e segurança de perímetro. | [→ Ver projeto](./dmz/README.md) |
| 🐧 **Laboratório Linux Engineering** | Hardening, automação e administração Linux. | [→ Ver projeto](./linuxlab/README.md) |
| 🎯 **Laboratório Ofensivo (Red Team)** | Pentest e simulações ofensivas. | [→ Ver projeto](./offensive-lab/README.md) |
| 🧩 **Laboratório Defensivo (Blue Team)** | Forense digital e resposta a incidentes. | [→ Ver projeto](./defense-lab/README.md) |
| 🎥 **Docs & Media Studio** | Documentação técnica e material audiovisual. | [→ Ver projeto](./docs-studio/README.md) |

---

## 📘 Visão Geral

O **Datacenter PrimeOps** foi projetado como um **ambiente híbrido para aprendizado, automação e segurança**, utilizando o **Proxmox VE** como hipervisor principal.  
Seu objetivo é oferecer uma infraestrutura completa para **laboratórios de redes, cibersegurança, computação em nuvem e automação**, integrando **ambientes on-premises e de nuvem pública (AWS, Azure e Google Cloud)**.

Este documento descreve a **estrutura física e lógica**, bem como as **etapas de instalação, configuração e boas práticas**.

---

## 🧩 Estrutura do Projeto

| Componente | Função Principal | Sistema | Status |
|-------------|------------------|----------|---------|
| **Proxmox VE** | Hipervisor principal | Debian 12 | ✅ |
| **VM - Windows Server** | Active Directory, DNS, GPO | WinSrv 2022 | ✅ |
| **VM - Windows 11** | Estação administrativa | Win11 | ✅ |
| **VM - Linux GitLab** | Servidor de repositório e CI/CD | Ubuntu 24.04 | ✅ |
| **VM - PFSense** | Firewall, VLANs, VPNs e regras de segurança | FreeBSD | ✅ |
| **VM - SIEM/SOAR** | Monitoramento, correlação de logs e automação | Wazuh + TheHive | 🔄 |
| **VM - Web Server** | Nginx + PHP + Banco de Dados | Debian | ✅ |
| **VM - Cloud Gateway** | Ponte para serviços externos (AWS, GCP, Azure) | Debian | 🔄 |

---

## ⚙️ Configuração de Hardware

| Recurso | Especificação |
|----------|----------------|
| Host Físico | Intel Xeon 8C/16T, 32 GB RAM |
| Armazenamento | SSD 250 GB (sistema) + 2x HDD 1 TB (dados e VMs) |
| Rede | 2x interfaces Gigabit + switch gerenciável |
| Virtualização | Proxmox VE 8.2 |

---

## 🌐 Tabela de VLANs e Sub-redes

| VLAN | Setor / Função | Faixa IP | Gateway | Observações |
|------|----------------|-----------|-----------|--------------|
| 10 | Administração | 10.10.10.0/24 | 10.10.10.1 | Servidores de gerenciamento |
| 20 | Usuários / Acesso | 10.10.20.0/24 | 10.10.20.1 | Estações Windows e Linux |
| 30 | Impressoras | 10.10.30.0/24 | 10.10.30.1 | VLAN isolada |
| 40 | IoT / Dispositivos | 10.10.40.0/24 | 10.10.40.1 | Sensores e dispositivos IoT |
| 50 | Segurança / SOC | 10.10.50.0/24 | 10.10.50.1 | SIEM, SOAR, Wazuh |
| 60 | DMZ / Web / VPN | 10.10.60.0/24 | 10.10.60.1 | Servidores públicos |

---

## 🗺️ Diagrama Lógico (Conceitual)

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

💻 Tabela de Interfaces PFSense
Interface	VLAN	IP	Função
LAN0	10	10.10.10.1	Administração
LAN1	20	10.10.20.1	Usuários
LAN2	30	10.10.30.1	Impressoras
LAN3	40	10.10.40.1	IoT
LAN4	50	10.10.50.1	SOC / SIEM
WAN	60	DHCP / Público	DMZ / VPN externa
🔐 VPN e Segurança

    VPN site-to-site entre PFSense e gateways externos (Fortinet / AWS / Azure).

    VPN de cliente (OpenVPN) para acesso remoto seguro de administradores.

    Política Zero Trust aplicada em todas as camadas:

        Controle de identidade (AD + MFA)

        Segmentação de rede via VLAN

        Monitoramento contínuo (Wazuh)

        SIEM/SOAR com alertas e automação

    Regras de firewall baseadas em privilégio mínimo e políticas em camadas.

🧠 Laboratórios de Segurança e Auditoria
Tipo de Lab	Ferramentas	Objetivo
Ofensivo (Red Team)	Kali Linux, Metasploit, Nmap	Testes de penetração e avaliação de vulnerabilidades
Defensivo (Blue Team)	Wazuh, Suricata, OSSEC	Monitoramento e resposta a incidentes
Auditoria & Logs	Elastic Stack, Graylog	Correlação e análise forense
SOAR	TheHive, Cortex, MISP	Automação de resposta a incidentes
⚡ Procedimentos de Instalação

    Instalar Proxmox VE
    ISO oficial: https://www.proxmox.com/en/downloads

sudo apt update && sudo apt upgrade -y
sudo apt install proxmox-ve postfix open-iscsi -y

Criação das VMs

    Nome padrão: PXD-SRV-<função>

    Discos do sistema em SSD, dados em HDD

    Rede configurada via vmbr0 com VLAN tagging

Instalar PFSense

    Interface WAN = VLAN60

    Interfaces LAN conforme tabela VLAN

    Ativar NAT e DHCP por sub-rede

VPN Site-to-Site

    Túnel IPSec entre PFSense e gateway Fortinet/AWS:

        Fase 1: AES256 / SHA256 / DH14

        Fase 2: AES256GCM / PFS Group 14

Implantação do Wazuh (SIEM)

    curl -sO https://packages.wazuh.com/4.x/wazuh-install.sh
    bash wazuh-install.sh -a

👨‍💻 Desenvolvido por

PrimeOps – Infraestrutura, Automação e Cibersegurança
Desenvolvimento e Documentação: André Gonçalvez

    “Automação não é sobre substituir pessoas — é sobre liberar o potencial humano para resolver problemas mais complexos.”
