# Netzplan der Muster GmbH

Domäne: `muster.local`

## VLANs

| VLAN-ID | Name | Netz | Gateway (OPNsense) | DHCP-Bereich | Zweck |
| --- | --- | --- | --- | --- | --- |
| 10 | Server | 10.10.10.0/24 | 10.10.10.1 | keiner (statische Adressen) | Server und Infrastruktur |
| 20 | Clients | 10.10.20.0/24 | 10.10.20.1 | 10.10.20.100 bis 10.10.20.200 | Arbeitsplätze der Mitarbeitenden |
| 30 | Gäste | 10.10.30.0/24 | 10.10.30.1 | 10.10.30.100 bis 10.10.30.200 | Nur Internetzugang |

## Server und Geräte

| Hostname | System | IP-Adresse | VLAN | Rolle |
| --- | --- | --- | --- | --- |
| FW01 | OPNsense | 10.10.10.1 / .20.1 / .30.1 | alle | Router, Firewall, Gateway |
| DC01 | Windows Server 2022 | 10.10.10.10 | 10 | Active Directory, DNS, DHCP (VLAN 20) |
| SRV01 | Debian 12 + Docker | 10.10.10.20 | 10 | Nextcloud, Wiki |
| MON01 | Debian 12 + Checkmk | 10.10.10.30 | 10 | Monitoring |
| PBS01 | Proxmox Backup Server | 10.10.10.40 | 10 | Datensicherung |
| PC01 | Windows 11 | per DHCP | 20 | Arbeitsplatz |

## Firewall-Regeln (geplant)

| Quelle | Ziel | Erlaubt | Grund |
| --- | --- | --- | --- |
| Clients (VLAN 20) | Server (VLAN 10) | DNS, Anmeldung (AD), HTTPS | Zugriff auf Domäne und Dienste |
| Clients (VLAN 20) | Internet | ja | normale Arbeit |
| Gäste (VLAN 30) | Server / Clients | nein | Trennung vom Firmennetz |
| Gäste (VLAN 30) | Internet | ja | Gastzugang |
| Server (VLAN 10) | Internet | Updates | Aktualisierungen |

## Organisationseinheiten (Active Directory)

```
muster.local
└── Muster GmbH
    ├── Verwaltung
    ├── Vertrieb
    ├── IT
    └── Computer
```
