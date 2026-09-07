# Technologies utilisées dans le labo réseau/sécurité

Inventaire des technologies déployées, avec leur rôle dans l'architecture globale.

---

## 1. Virtualisation

| Technologie | Rôle dans le labo |
|---|---|
| **Microsoft Hyper-V** | Hyperviseur principal, exécuté sur deux hôtes physiques distincts (`DESKTOP-J7H10AF` et `PC-DADDY`) qui hébergent l'ensemble des VMs du labo. |
| **Hyper-V Manager** | Console de gestion pour créer, configurer et administrer les machines virtuelles et les switches virtuels. |
| **Hyper-V Virtual Switch** (Internal / External) | Réseaux virtuels reliant les VMs entre elles (Internal) ou au monde extérieur via une carte physique (External). Plusieurs switches dédiés : `LAN-OPNsense`, `External-WiFi`, `vSwitch-LAB`, `SOC-Internal`, etc. |

---

## 2. Pare-feu / Routage réseau

| Technologie | Rôle dans le labo |
|---|---|
| **OPNsense** (v24.7.12_4) | Pare-feu/routeur central du labo, virtualisé sous Hyper-V. Gère le WAN, le LAN, le NAT, les VPN et le filtrage de trafic. |
| **FreeBSD 14.1** | Système d'exploitation sous-jacent à OPNsense. |
| **pf (Packet Filter)** | Moteur de filtrage de paquets natif de FreeBSD, utilisé par OPNsense pour appliquer les règles firewall et le NAT. |
| **Unbound DNS** | Résolveur DNS récursif intégré à OPNsense, sert de serveur DNS pour le LAN. |
| **DHCP (ISC/Kea)** | Serveur DHCP intégré à OPNsense pour l'attribution automatique d'adresses IP sur le LAN. |
| **NAT (Outbound)** | Traduction d'adresses (mode automatique/hybride) permettant aux machines du LAN de sortir vers Internet via l'IP WAN. |

---

## 3. VPN

| Technologie | Rôle dans le labo |
|---|---|
| **OpenVPN** | VPN Site-to-Site et accès distant (road warrior), sécurisé par certificats TLS/PKI, utilisé pour relier des sites distants et pour l'accès mobile au LAN. |
| **WireGuard** | VPN moderne léger, configuré en serveur avec plusieurs pairs mobiles pour l'accès distant. |
| **OpenVPN Connect** | Client VPN (mobile/desktop) utilisé pour se connecter aux instances OpenVPN d'OPNsense. |

---

## 4. Systèmes d'exploitation des VMs

| Technologie | Rôle dans le labo |
|---|---|
| **Windows Server 2022** | OS des serveurs `DC-SVR`, `SRV-WEB01`, `SVR-MON01` (contrôleur de domaine, serveur web, serveur de monitoring). |
| **Windows 11** | OS des postes clients de test. |
| **Ubuntu 22.04 LTS** | OS hébergeant `Wazuh-Manager` (serveur SIEM). |

---

## 5. Sécurité — EDR / SIEM / NSM

| Technologie | Rôle dans le labo |
|---|---|
| **Huntress** | Agent EDR (Endpoint Detection & Response) cloud-managé, déployé sur les serveurs Windows pour la détection de menaces et de ransomware. |
| **Wazuh** (v4.7.5) | Plateforme SIEM/XDR open-source pour la collecte de logs, la détection basée sur des règles, et le monitoring d'intégrité de fichiers. |
| **Wazuh Indexer** (basé sur **OpenSearch** 2.8.0) | Moteur de stockage et d'indexation des événements collectés par Wazuh. |
| **Wazuh Dashboard** | Interface web (Node.js / OpenSearch Dashboards) de visualisation des alertes et agents Wazuh. |
| **Security Onion** | Plateforme NSM (Network Security Monitoring) tout-en-un, incluant Suricata (IDS), Zeek et Elasticsearch pour l'inspection de trafic réseau. |
| **Darktrace** (switch de mirroring) | Point de capture réseau (`Darktrace-Mirror`) prévu pour l'inspection passive du trafic. |

---

## 6. Monitoring / Observabilité

| Technologie | Rôle dans le labo |
|---|---|
| **Prometheus** (v2.51.0) | Système de collecte et de stockage de métriques temporelles. |
| **node_exporter** | Exportateur de métriques système pour les hôtes Linux/OPNsense, scrapé par Prometheus. |
| **windows_exporter** | Exportateur de métriques système pour les hôtes Windows (ex. `SRV-WEB01`). |
| **Grafana** | Outil de visualisation des métriques collectées par Prometheus (tableaux de bord). |

---

## 7. PKI / Certificats

| Technologie | Rôle dans le labo |
|---|---|
| **OpenSSL** | Génération et gestion des certificats TLS utilisés par OPNsense (interface web, VPN). |
| **Certificats X.509 / CA interne** | Autorité de certification interne (`VPN-CA`) émettant les certificats serveur/client pour OpenVPN. |

---

## 8. DNS dynamique

| Technologie | Rôle dans le labo |
|---|---|
| **Cloudflare** | Fournisseur DNS géré via API, hébergeant l'enregistrement `vpn.daddyconnectit.com`. |
| **ddclient** | Client DDNS sur OPNsense qui met à jour automatiquement l'enregistrement DNS avec l'IP WAN publique. |

---

## 9. Outils réseau / diagnostic (CLI)

| Technologie | Rôle dans le labo |
|---|---|
| **SSH (OpenSSH)** | Accès distant en ligne de commande à OPNsense et aux VMs Linux. |
| **PowerShell / PowerShell Remoting** (`Invoke-Command`) | Administration à distance des VMs Windows depuis l'hôte Hyper-V. |
| **tcpdump** | Capture et analyse de paquets réseau pour le diagnostic. |
| **pfctl** | Utilitaire en ligne de commande pour inspecter et manipuler les règles pf d'OPNsense. |
| **GRUB 2** | Bootloader Linux, utilisé ici pour la récupération d'accès (reset de mot de passe root). |
| **LVM (Logical Volume Manager)** | Gestion des volumes disque sous Ubuntu. |

---

## 10. Documentation / production de contenu

| Technologie | Rôle dans le labo |
|---|---|
| **OBS Studio** | Logiciel d'enregistrement d'écran utilisé pour produire la vidéo de démonstration du labo. |

---

## Schéma d'architecture simplifié

```
Internet
   │
   ▼
[OPNsense] (FreeBSD, pf, Unbound, DHCP, NAT, VPN)
   │
   ├── LAN 192.168.1.0/24 ── SRV-WEB01 (Windows Server 2022 + Huntress + windows_exporter)
   │
   └── LAN 192.168.2.0/24 ── DC-SVR, SVR-MON01, Wazuh-Manager (Ubuntu), Security Onion
                                    │
                                    └── Wazuh Indexer/Dashboard (OpenSearch)

VPN distant ── OpenVPN / WireGuard ── accès mobile au LAN
DNS dynamique ── ddclient → Cloudflare (vpn.daddyconnectit.com)
Monitoring ── Prometheus + exporters → Grafana
```
