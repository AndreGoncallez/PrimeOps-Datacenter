# 🏗️ Datacenter Setup – PrimeOps Cloud Hybrid Lab

**Idiomas:** [🇧🇷 Português](./Datacenter_Setup.md) | [🇬🇧 English](./Datacenter_Setup.en.md) | [🇮🇹 Italiano](./Datacenter_Setup.it.md)

---

## 📘 Visão Geral

O **Datacenter PrimeOps** foi projetado como um **ambiente híbrido de estudo, automação e segurança**, utilizando o **Proxmox VE** como hypervisor principal.  
O objetivo é fornecer uma infraestrutura completa para **laboratórios de redes, cibersegurança, cloud computing e automação**, com integração entre ambientes **on-premises e cloud pública (AWS, Azure e Google Cloud)**.

Este documento descreve a **estrutura física e lógica**, bem como o **passo a passo de instalação, configuração e práticas recomendadas**.

---

## 🧩 Estrutura do Projeto

| Componente              | Função Principal                                   | Sistema | Status |
|--------------------------|----------------------------------------------------|----------|---------|
| **Proxmox VE**           | Hypervisor principal                              | Debian 12 | ✅ |
| **VM - Windows Server**  | Active Directory, DNS, GPO                         | WinSrv 2022 | ✅ |
| **VM - Windows 11**      | Estação administrativa                             | Win11 | ✅ |
| **VM - Linux GitLab**    | Servidor de repositórios e CI/CD                  | Ubuntu 24.04 | ✅ |
| **VM - PFSense**         | Firewall, VLANs, VPNs e regras de segurança        | FreeBSD | ✅ |
| **VM - SIEM/SOAR**       | Monitoramento, correlação de logs e automação      | Wazuh + TheHive | 🔄 |
| **VM - Servidor Web**    | Nginx + PHP + Banco de dados                      | Debian | ✅ |
| **VM - Cloud Gateway**   | Bridge com serviços externos (AWS, GCP, Azure)     | Debian | 🔄 |

---

## ⚙️ Configuração de Hardware

| Recurso | Especificação |
|----------|----------------|
| Host Físico | Intel Xeon 8C/16T, 32 GB RAM |
| Armazenamento | SSD 250 GB (sistema) + 2x HDD 1 TB (dados e VMs) |
| Rede | 2 interfaces Gigabit + switch gerenciável |
| Virtualização | Proxmox VE 8.2 |

---

## 🌐 Tabela de VLANs e Sub-redes

| VLAN | Setor / Função       | Faixa IP            | Gateway        | Observações |
|------|----------------------|---------------------|----------------|--------------|
| 10   | Administração        | 10.10.10.0/24       | 10.10.10.1     | Servidores de gestão |
| 20   | Usuários / Acesso    | 10.10.20.0/24       | 10.10.20.1     | Estações Windows e Linux |
| 30   | Impressoras          | 10.10.30.0/24       | 10.10.30.1     | VLAN isolada |
| 40   | IoT / Dispositivos   | 10.10.40.0/24       | 10.10.40.1     | Sensores e IoT |
| 50   | Segurança / SOC      | 10.10.50.0/24       | 10.10.50.1     | SIEM, SOAR, Wazuh |
| 60   | DMZ / Web / VPN      | 10.10.60.0/24       | 10.10.60.1     | Servidores públicos |

---

## 🗺️ Diagrama Lógico (conceitual)

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

    VPN Site-to-Site entre o PFSense e gateways externos (Fortinet / AWS / Azure).

    VPN Cliente (OpenVPN) para acesso remoto seguro de administradores.

    Política Zero Trust implementada em camadas:

        Controle de identidade (AD + MFA)

        Segmentação via VLAN

        Monitoramento contínuo via Wazuh

        SIEM/SOAR com alertas e automação

    Firewall Rules organizadas por camadas e políticas de menor privilégio.

🧠 Laboratórios de Segurança e Auditoria
Tipo de Lab	Ferramentas	Objetivo
Ataque (Red Team)	Kali Linux, Metasploit, Nmap	Testes de penetração e vulnerabilidades
Defesa (Blue Team)	Wazuh, Suricata, OSSEC	Monitoramento e resposta a incidentes
Auditoria & Logs	Elastic Stack, Graylog	Correlação e análise forense
SOAR	TheHive, Cortex, MISP	Automação de resposta a incidentes

⚡ Procedimentos de Instalação
1. Instalação do Proxmox VE

# ISO oficial: https://www.proxmox.com/en/downloads
sudo apt update && sudo apt upgrade -y
sudo apt install proxmox-ve postfix open-iscsi -y

2. Criação das VMs

    Nome padrão: PXD-SRV-<função>

    Armazenamento em SSD para SO e HDD para dados

    Rede configurada via vmbr0 com VLAN tag

3. Instalação do PFSense

    Interface WAN = VLAN60

    Interfaces LAN criadas conforme tabela de VLANs

    Habilitar NAT e DHCP conforme sub-rede

4. VPN Site-to-Site

Configuração de túnel IPSec entre PFSense e gateway Fortinet/AWS:

Phase 1: AES256 / SHA256 / DH14
Phase 2: AES256GCM / PFS Group 14

5. Implantação do Wazuh (SIEM)

curl -sO https://packages.wazuh.com/4.x/wazuh-install.sh
bash wazuh-install.sh -a

👨‍💻 Desenvolvido por

PrimeOps – Infraestrutura, Automação e Cibersegurança
Desenvolvimento e documentação: André Gonçalvez
