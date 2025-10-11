# 🧠 Datacenter PrimeOps  
**Infraestrutura Híbrida Automatizada | Segurança | Cloud | DevOps | Monitoramento**

🌍 [English Version](./README_EN.md) | 🇮🇹 [Versione Italiana](./README_IT.md)

---

## 🔷 Visão Geral

O **Datacenter PrimeOps** é um projeto corporativo que representa um **ambiente de datacenter híbrido totalmente automatizado**, integrando práticas de **Infraestrutura como Código (IaC)**, **Segurança Zero Trust**, **Cloud Computing** e **Monitoramento em Tempo Real**.  

Desenvolvido como laboratório técnico e ambiente de referência, o projeto demonstra a aplicação prática de soluções de **DevOps, automação e operações de rede** em ambientes **on-premise, híbridos e multi-cloud**.

---

## 🧩 Arquitetura e Topologia

- **Simulação completa via EVE-NG**, com roteadores, switches e firewalls Fortinet.  
- **Integração híbrida** entre Azure, AWS e Google Cloud.  
- **VLANs, VPNs e roteamento dinâmico** entre segmentos de rede.  
- **Servidores Linux e Windows** interligados, com autenticação centralizada.  
- Camadas de segurança baseadas em **Zero Trust e segmentação lógica**.  

📁 *O diagrama de rede e documentação da arquitetura serão adicionados na pasta `/docs/topologia`.*

---

## 🖥️ Componentes e Serviços Principais

| Componente | Descrição |
|-------------|------------|
| **Active Directory (Windows Server)** | Autenticação centralizada, políticas de grupo e controle de acesso. |
| **Servidores Linux (Debian, Ubuntu, CentOS)** | Hospedagem de serviços, automações e aplicações corporativas. |
| **Servidor Web (Apache/Nginx)** | Deploy de aplicações e sites internos. |
| **Servidor de Arquivos (Nextcloud / tipo Google Drive)** | Armazenamento interno, sincronização e controle de versões. |
| **Banco de Dados / Storage** | Replicação, snapshots e rotinas automáticas de backup. |
| **Firewall Fortinet** | Segurança perimetral e controle de tráfego. |
| **SIEM / SOC** | Coleta, correlação e resposta a eventos de segurança. |

---

## 🔐 Segurança e SOC

- Implementação de **políticas de segurança Zero Trust**.  
- **Firewall Fortinet** com inspeção profunda e regras dinâmicas.  
- **Integração com SIEM/SOC** para detecção e resposta a incidentes.  
- **Automação de auditorias e alertas** via scripts personalizados.  
- **Monitoramento centralizado** com Prometheus e Grafana.  

---

## ⚙️ Automação e Scripts

A automação é a espinha dorsal do Datacenter PrimeOps, integrando **IaC, CI/CD e gestão de configurações**.

### 🧰 Tecnologias Principais
- **Terraform** – Provisionamento automatizado de infraestrutura híbrida.  
- **Ansible** – Gerenciamento de configuração e deploy de serviços.  
- **PowerShell / Bash / Python** – Scripts para administração, backup e monitoramento.  
- **GitHub Actions / Azure Pipelines** – Integração e entrega contínua (CI/CD).  

### 📜 Categorias de Scripts

| Categoria | Stack | Descrição |
|------------|--------|-----------|
| **Provisionamento e Deploy** | Terraform / Ansible | Criação e atualização de ambientes automatizados. |
| **Monitoramento e Alertas** | Python / Bash | Coleta de métricas, logs e disponibilidade. |
| **Segurança e IAM** | Terraform / PowerShell | Automação de permissões e auditorias. |
| **Backup e Restauração** | Bash / Python | Snapshots, cópias e versionamento automático. |
| **Testes e Diagnóstico** | PowerShell / Bash | Análise de rede, conectividade e desempenho. |

📂 *Os scripts estão organizados na pasta `/scripts`, com documentação individual e exemplos de uso.*

---

## 🧠 Laboratórios e Ambientes de Teste

O projeto inclui diversos laboratórios integrados, usados para simulação, aprendizado e validação de soluções.

| Laboratório | Descrição |
|--------------|------------|
| **Lab de Redes (CCNA)** | Simulações de roteamento, VLANs e ACLs. |
| **Lab de Firewall (Fortinet)** | Políticas de segurança, inspeção de pacotes e NAT. |
| **Lab de Linux (LPIC-1 / LPIC-2)** | Administração, automação e integração em rede. |
| **Lab de Cloud (GCP / AWS)** | Deploys híbridos e interconectados. |
| **Lab de SOC / SIEM** | Centralização e correlação de logs com alertas automáticos. |

---

## 🎓 Certificações e Plano de Estudos (2025–2027)

### 2025 – Fundamentos e Redes
- Cisco **CCNA** – redes corporativas e roteamento seguro.  
- **Fortinet NSE 1–4** – segurança perimetral e firewall.  
- **Active Directory e Windows Server** – autenticação e GPOs.  
- **EVE-NG Labs** – topologias e simulações completas.

### 2026 – Sistemas e Automação
- **LPIC-1 e LPIC-2** – administração Linux e integração com AD.  
- **Automação IaC com Ansible e Terraform.**  
- **CI/CD** – pipelines com GitHub Actions e Azure DevOps.  
- **Monitoramento e Observabilidade** – Prometheus, Grafana e SIEM.

### 2027 – Cloud e Segurança Avançada
- **Google Cloud:** Associate Engineer, Security e DevOps.  
- **AWS:** Practitioner, Architect e Security Specialist.  
- **Zero Trust Architecture e SOC/SIEM Operations.**  
- **Projetos multi-cloud** com automação completa e integração segura.

📘 *Os laboratórios de certificação estão integrados ao Datacenter PrimeOps, permitindo prática contínua e validação de conhecimentos.*

---

## 🧭 Roadmap e Expansão

- [ ] Publicar diagramas detalhados da topologia e rede.  
- [ ] Finalizar integração multi-cloud (AWS + GCP + Azure).  
- [ ] Adicionar automação de logs centralizados para SOC.  
- [ ] Criar dashboards dinâmicos no Grafana.  
- [ ] Expandir os laboratórios para certificações avançadas (GCP Security / AWS Architect).  

---

## 🧾 Créditos e Autoria

Desenvolvido por **Andre Gonçallez**  
**PrimeOps — Engenharia e Automação de Infraestrutura**

📧 andregoncallez@protonmail.com  
🌐 [LinkedIn](https://www.linkedin.com/in/andregoncallez)  
💻 [GitHub](https://github.com/primeops-x2s)

---

> _Datacenter PrimeOps é um ambiente de referência em automação, segurança e operação de infraestrutura híbrida — projetado para refletir práticas modernas de engenharia e arquitetura corporativa._

---
