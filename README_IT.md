[🇧🇷 Portuguese](README.md) | [🇬🇧 English](README_EN.md) | [🇮🇹 Italiano](README_IT.md)

# 🏢 Datacenter PrimeOps  
**Ingegneria dell’infrastruttura ibrida e automazione professionale**

Il progetto **Datacenter PrimeOps** rappresenta un ambiente completo e modulare progettato per l’automazione delle infrastrutture enterprise, le operazioni di sicurezza e la gestione di datacenter ibridi.  
È concepito come **laboratorio professionale** e **modello di riferimento** per l’ingegneria cloud, di rete e di sicurezza informatica.

---

## 🚀 Panoramica
PrimeOps integra più domini — ingegneria di rete, amministrazione dei sistemi, automazione cloud e sicurezza — in un unico framework coerente.  
È strutturato per l’**apprendimento continuo**, la **preparazione alle certificazioni** e il **collaudo di ambienti infrastrutturali reali**.

**Obiettivi principali:**
- Progettare e simulare un datacenter moderno con integrazione cloud ibrida.  
- Implementare pipeline CI/CD, automazione IaC e principi di Zero Trust.  
- Testare scenari reali di sicurezza, resilienza e scalabilità.  

---

## 🧩 Architettura principale

| Livello | Descrizione |
|----------|-------------|
| **Infrastruttura** | Topologia virtualizzata costruita con EVE-NG, integrando ambienti Fortinet, Cisco e Linux. |
| **Rete** | Routing, segmentazione VLAN, tunnel VPN e firewall perimetrali. |
| **Active Directory** | Autenticazione centralizzata e criteri di gruppo per ambienti aziendali. |
| **Server Linux** | Sistemi basati su LPIC per servizi di file, web e automazione. |
| **Servizi Dati** | Storage cloud interno simile a Google Drive per distribuzioni private. |
| **Sicurezza** | Laboratori Fortinet, simulazioni SOC, integrazione SIEM e architettura Zero Trust. |
| **Automazione** | Infrastructure as Code con Ansible, Terraform e GitHub Actions. |

---

## ⚙️ Automazione e Script
Il repository include una collezione completa di script di automazione e modelli di configurazione:

- **Distribuzione di rete** → creazione automatizzata di topologie tramite EVE-NG.  
- **Politiche di sicurezza** → configurazioni Fortinet e routine di validazione.  
- **Automazione Linux** → script Bash e Ansible per hardening e distribuzione servizi.  
- **Monitoraggio** → stack Prometheus + Grafana con allarmi e forwarding dei log.  
- **CI/CD** → pipeline GitHub Actions per test e validazione dell’infrastruttura.  

---

## 🔐 Laboratori di Sicurezza e SOC
PrimeOps include un ambiente dedicato per la **simulazione di un Security Operations Center (SOC)**:

- Laboratorio firewall e VPN Fortinet  
- Implementazione SIEM con analisi e correlazione dei log  
- Flussi di rilevamento e risposta agli incidenti  
- Modello Zero Trust per ambienti multi-cloud  

---

## ☁️ Integrazione Cloud
Ambiente ibrido progettato per la pratica certificativa e le implementazioni aziendali:

- **Google Cloud Platform** — laboratori di automazione e gestione identità  
- **AWS** — scenari di rete ibrida e sicurezza  
- **Azure** — pipeline CI/CD e governance infrastrutturale  

---

## 🧠 Piano di certificazioni e formazione (2025–2027)

### 2025 — Fondamenta e Reti
- Cisco **CCNA**  
- Fortinet **NSE 1–4**  
- Laboratori EVE-NG per automazione di rete  
- Active Directory e gestione Windows Server  

### 2026 — Sistemi e Automazione
- **LPIC-1 / LPIC-2**  
- Automazione con Ansible e Terraform  
- Pipeline CI/CD con GitHub Actions e Azure DevOps  
- Monitoraggio e osservabilità — Prometheus, Grafana, SIEM  

### 2027 — Cloud e Sicurezza Avanzata
- Certificazioni Google Cloud (Engineer / Security / DevOps)  
- AWS Solutions Architect & Security Specialist  
- Architetture Zero Trust e operazioni SOC  
- Automazione multi-cloud e interconnessione sicura  

---

## 🧭 Roadmap del Progetto
1. Costruzione e documentazione della topologia EVE-NG.  
2. Integrazione dei sistemi Fortinet e Cisco.  
3. Distribuzione dei server Linux e Active Directory.  
4. Automazione dei processi di deploy e monitoraggio.  
5. Configurazione dei collegamenti ibridi con GCP e AWS.  
6. Espansione degli ambienti SOC e Zero Trust.  

---

## 📁 Struttura del Repository

/
├── /automation/ → Script Ansible, Terraform, Bash
├── /network-labs/ → Topologie e configurazioni EVE-NG
├── /security-labs/ → Laboratori Fortinet e SIEM
├── /linux/ → Configurazioni e gestione server
├── /cloud/ → Template d’integrazione GCP e AWS
└── README_IT.md → Questo documento


---

## 🧾 Licenza
Questo progetto è disponibile per scopi educativi e professionali sotto licenza **MIT**.  
L’uso commerciale o la redistribuzione richiedono un’autorizzazione esplicita.

---

## 👨‍💻 Sviluppato da  
**Andre Gonçallez** – PrimeOps  
_Ingegneria dell’infrastruttura e dell’automazione_
