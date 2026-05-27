# aksoy-net-homelab
Self-Hosted Home Lab — Proxmox, OPNsense, Docker, Wazuh SIEM, CrowdSec, Grafana
# AKSOY-NET HOME LAB

<div align="center">

![Aksoy-Net]
![Proxmox](https://img.shields.io/badge/Proxmox-9.2-E57000?style=for-the-badge&logo=proxmox&logoColor=white)
![OPNsense](https://img.shields.io/badge/OPNsense-25.7-D94F00?style=for-the-badge&logo=opnsense&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Wazuh](https://img.shields.io/badge/Wazuh-SIEM-005571?style=for-the-badge&logo=wazuh&logoColor=white)
![Status](https://img.shields.io/badge/Status-In%20Betrieb-brightgreen?style=for-the-badge)

**Professionelles Self-Hosted Home Lab — Proxmox, OPNsense, Docker, Wazuh SIEM, CrowdSec, Grafana und mehr.**

Selbst aufgebaut, konfiguriert und dokumentiert — im Rahmen der FISI-Ausbildung mit Fokus IT-Security.

[📖 Dokumentation](#-dokumentation) · [🖥️ Stack](#️-tech-stack) · [🔒 Security](#-security-stack) · [📊 Monitoring](#-monitoring) · [🗺️ Roadmap](#️-roadmap)

</div>

---

## 🏠 Was ist Aksoy-Net?

Aksoy-Net ist ein vollständig selbst betriebenes Home Lab auf einem Dell OptiPlex in der Garage — von der Netzwerkarchitektur über Virtualisierung bis zum Security-Stack alles selbst konfiguriert und dokumentiert.

Keine gemieteten Cloud-Server. Keine vorgefertigten Lösungen. Alles on-premise.

---

## 🖥️ Tech Stack

### Hardware

| Komponente | Details |
|---|---|
| Server | Dell OptiPlex 7060 SFF |
| CPU | Intel Core i3-9100 (4C/4T, 9. Gen) |
| RAM | 32 GB DDR4 |
| Storage OS | 256 GB NVMe SSD (Proxmox + VM) |
| Storage Daten | 1 TB SSHD (alle Docker-Daten) |
| KI-Accelerator | Google Coral TPU M.2 A+E (Frigate NVR) |
| Router | AVM FRITZ!Box 6690 Cable |
| Switch | TP-Link TL-SG108E (Managed, 8-Port) |

### Netzwerk-Architektur

```
Internet
    │
FRITZ!Box (192.xxx.xxx.xxx)  ← Heimnetz
    │
OPNsense WAN (192.xxx.xxx.xxx)  ← Firewall VM
    │
OPNsense LAN (10.xxx.xxx.xxx)  ← Server-Netz
    │
Ubuntu VM Docker-Host (10.x.x.x)
    │
    ├── Cloudflare Tunnel  →  aksoy-net.de (alle Domains)
    └── Tailscale VPN      →  xxx.xxx.xxx.xxx (immer erreichbar)
```

### Virtualisierung

| Komponente | Details |
|---|---|
| Hypervisor | Proxmox VE 9.2 (Kernel 7.0.2) |
| VM 100 | Ubuntu Server 24.04 — Docker-Host (10.10.10.10) |
| VM 101 | OPNsense 25.7 — Firewall & Router |
| VPN | Tailscale (WireGuard-basiert) — immer erreichbar |

---

## 🐳 Docker-Services

### Cloud & Zugang

| Service | Port | URL |
|---|---|---|
| Nextcloud | xxxx | https:// |
| Vaultwarden | xxxx | https:// |
| Cloudflare Tunnel | outbound | Alle Domains — keine offenen Ports |
| Tailscale VPN | host | xxx.xxx.xxx.xxx |

### Management

| Service | Port | URL |
|---|---|---|
| Homepage Dashboard | port | https:// |
| Portainer CE |port | intern |
| Apache Guacamole |port | https:// |
| Uptime Kuma | port | https:// |

### Monitoring

| Service | Port | Funktion |
|---|---|---|
| Grafana | xxxx | https:// |
| Loki | xxxx | Log-Aggregation |
| Promtail | — | Log-Collector |
| Prometheus | xxxx | Metriken |
| Netdata | xxxx | Echtzeit System-Monitoring |

### Kameras & KI

| Service | Port | Funktion |
|---|---|---|
| Frigate NVR | xxxx | KI-Kameraüberwachung  |
| Mosquitto MQTT | xxxx | Kommunikationsbroker |
| Double-Take | xxxx | Gesichtserkennung |
| DeepStack | xxxx | KI-Engine |

---

## 🔒 Security-Stack

```
┌─────────────────────────────────────────────────────────────┐
│                    SECURITY LAYERS                          │
│                                                             │
│  🌐 Cloudflare                                              │
│     DDoS-Schutz · echte IP versteckt · SSL · Bot-Schutz     │
│     Cloudflare Access (OTP) für sensitive Services          │
│                                                             │
│  🔥 OPNsense Firewall (VM 101)                              │
│     Netzwerk-Segmentierung · NAT · Firewall-Regeln          │
│     Heimnetz ↔ Server-Netz kontrolliert                     │
│                                                             │
│  🛡️ CrowdSec IPS (System-Service)                           │
│     Community Threat Intelligence                           │
│     > 22.000 IPs automatisch geblockt                       │
│     iptables-Bouncer · Prometheus-Metriken                  │
│                                                             │
│  🔍 Suricata IDS (System-Service)                           │
│     Netzwerk-Traffic-Analyse · IDS-Modus                    │
│     ~50.000 ET Open Signaturen · Loki-Integration           │
│                                                             │
│  🧠 Wazuh SIEM (Docker)                                     │
│     Threat Detection · File Integrity Monitoring            │
│     MITRE ATT&CK · Config Assessment · Vuln Detection       │
│     Dashboard: https://10.xx.xx.xx:xxx                      │
│                                                             │
│  📊 Grafana Security Dashboard                              │
│     CrowdSec Live-Entscheidungen · Suricata Alerts          │
│     Auth-Logs · Angreifer-Weltkarte · Telegram Alerts       │
└─────────────────────────────────────────────────────────────┘
```

### Security-Highlights

- ✅ **Keine offenen Ports** — externer Zugriff nur via Cloudflare Tunnel
- ✅ **> 22.000 IPs geblockt** — CrowdSec Community-Blockliste
- ✅ **DMARC `p=reject`** — kein E-Mail-Spoofing von aksoy-net.de möglich
- ✅ **HSTS aktiviert** — Browser erzwingen HTTPS
- ✅ **Cloudflare Access** — OTP-Login für sensitive Dienste
- ✅ **Tailscale VPN** — sicherer Admin-Zugriff von überall

---

## 📊 Monitoring

Grafana-Dashboard mit Live-Daten aus Loki + Prometheus:

| Panel | Datenquelle | Zeigt |
|---|---|---|
| CrowdSec Active Decisions | Prometheus | Geblockte IPs als Live-Chart |
| CrowdSec Erkennungen | Loki | Erkannte Angriffsmuster |
| Suricata IDS Alerts | Loki | Netzwerk-Intrusion Events |
| Auth-Logs | Loki | Login-Versuche (SSH, sudo) |
| Angreifer-Weltkarte | GeoIP + Loki | Geografische Herkunft |
| CrowdSec Security Engines | Prometheus (ID 19010) | Offizielles CrowdSec Dashboard |

**Telegram-Alerts** bei erkannten Angriffen via Grafana Contact Point.

---

## 📷 Kamerasystem

| Kamera | IP | Standort |
|---|---|---|
| 0| xxx.xxx.xxx.xxx | x |
| 1 |  | x |
| 2 | xxx.xxx.xxx.xxx | x |
| 3 | xxx.xxx.xxx.xxx | x |

**Frigate NVR** mit Google Coral TPU M.2 für KI-gestützte Objekterkennung (Person, Auto, Fahrrad, ...) — CPU-Last durch Coral von ~200% auf ~5% reduziert.

---

## 💻 Endgeräte

| Gerät | CPU | RAM | OS |
|---|---|---|---|
| ThinkPad T540 | Intel i7-4700MQ | 16 GB | Windows 10 + Parrot OS (Dual Boot) |
| ThinkPad X380 Yoga | Intel i5 (8. Gen) | 7.5 GB | Parrot OS Security |

Beide Laptops laufen unter **Parrot OS Security** — Debian-basierte Security-Distribution mit vorinstallierten Tools (nmap, Wireshark, Metasploit, Burp Suite).

---

## 📖 Dokumentation

| Dokument | Inhalt |
|---|---|
| [`docs/Aksoy-Net_Master_V6.pdf`](docs/Aksoy-Net_Master_V6.pdf) | Vollständige Infrastruktur-Dokumentation (15 Kapitel) |
| [`docs/Aksoy-Net_Installations_Referenz.pdf`](docs/Aksoy-Net_Installations_Referenz.pdf) | Alle Installations-Anleitungen, docker-compose, Befehlsreferenz |
| [`configs/`](configs/) | Beispiel-Konfigurationen (anonymisiert) |

### Dokumentations-Struktur

```
aksoy-net-homelab/
├── README.md
├── docs/
│   ├── Aksoy-Net_Master_V6.pdf
│   └── Aksoy-Net_Installations_Referenz.pdf
├── configs/
│   ├── frigate/config.yml
│   ├── grafana/promtail/config.yml
│   ├── grafana/docker-compose.yml
│   ├── crowdsec/collections.txt
│   └── homepage/services.yaml.example
└── screenshots/
    ├── dashboard.png
    ├── security-dashboard.png
    └── topologie.png
```

> ⚠️ **Sicherheitshinweis:** Alle Passwörter, API-Keys und Tokens sind durch Platzhalter ersetzt. Secrets werden ausschließlich über `.env`-Dateien verwaltet, die nicht im Repository enthalten sind.

---

## 🗺️ Roadmap

### Abgeschlossen ✅

- Proxmox VE mit Ubuntu VM und Docker-Basis
- Nextcloud, Vaultwarden, Uptime Kuma, Homepage Dashboard
- Grafana + Loki + Prometheus + Netdata
- Wazuh SIEM, CrowdSec IPS, Suricata IDS
- Cloudflare Tunnel + Access, Tailscale VPN
- Frigate NVR + Google Coral TPU M.2
- OPNsense Firewall & Migration auf Server-Netz (10.10.10.x)
- Apache Guacamole Remote-Access
- Portainer CE
- Restic-Backup (täglich, automatisch)
- Parrot OS auf beiden Laptops

### Geplant 🔧

- [ ] VLAN 20 — Pentest Lab (Kali Linux, isoliert)
- [ ] VLAN 30 — Kamera-Netz (Tapo isoliert, kein Internet)
- [ ] TP-Link Switch — 802.1Q VLAN-Konfiguration
- [ ] Global andere Orte Kameras  via go2rtc / Tailscale
- [ ] Docker + Grafana Karten-Panel
- [ ] MicroK8s (Lernumgebung auf LXC)

---

## 👤 Über das Projekt

**Ismail Aksoy**  
Angehender Fachinformatiker für Systemintegration (FISI) mit Fokus IT-Security

Aksoy-Net ist ein kontinuierlich wachsendes Home-Lab-Projekt, das parallel zur Umschulung aufgebaut wurde. Theorie aus der Ausbildung wird hier direkt in die Praxis umgesetzt — von Netzwerksegmentierung über SIEM bis zu KI-gestützter Kameraüberwachung.

Ab Oktober 2026: Praktikum im Bereich IT-Security.

- 🔗 GitHub: [github.com/dolunay38](https://github.com/dolunay38)
- 💼 LinkedIn: [Ismail Aksoy](https://linkedin.com/in/ismail-aksoy)
- 🌐 KI-Projekt: [KI-ARCHIV PRO](https://github.com/dolunay38/KI-ARCHIV-PRO)

---

<div align="center">

*Aksoy-Net — Self-Hosted. Security-First. Vollständig dokumentiert.*

</div>
