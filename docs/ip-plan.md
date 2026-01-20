# IP Address Plan – Omada Office Network
--------------------------------------

Dokumen ini merekodkan perancangan IP Address bagi rangkaian pejabat
berasaskan TP-Link Omada ecosystem.

Tujuan utama IP plan ini ialah:
- Elakkan IP conflict
- Mudahkan troubleshooting
- Pastikan peranti kritikal sentiasa boleh diakses
- Menjadi rujukan jangka panjang

---

## Network Summary
- Subnet: `192.168.10.0/24`
- Subnet Mask: `255.255.255.0`
- Total IP Available: 254 hosts

---

## Core Network Devices (Static IP)

| Device | Model | IP Address | Notes |
|------|------|-----------|------|
| Gateway Router | ER605 | 192.168.10.1 | Default Gateway |
| Omada Controller | OC200 | 192.168.10.2 | Central Management |
| Switch 1 | SG3428 (1st Floor) | 192.168.10.3 | Distribution Switch |
| Switch 2 | SG3428 (2nd Floor) | 192.168.10.4 | Distribution Switch |

---

## Wireless Infrastructure

| Device | Model | IP Mode | Notes |
|------|------|--------|------|
| Access Point 1 | EAP610 (1st Floor) | DHCP | Wi-Fi 6 |
| Access Point 2 | EAP610 (2nd Floor) | DHCP | Wi-Fi 6 |

---

## Client Devices

| Device Type | IP Mode | Remarks |
|-----------|--------|--------|
| Laptops | DHCP | Office Users |
| Desktops | DHCP / Reserved | Fixed workstation |
| Printers | DHCP / Reserved | Network Printing |

> DHCP reservations digunakan untuk peranti yang memerlukan IP konsisten
tanpa konfigurasi manual pada client.

---

## DHCP Configuration
- DHCP Server: **ER605**
- DHCP Range: `192.168.10.50 – 192.168.10.200`
- DNS Server: `192.168.10.1` (Gateway)

---

## Design Notes
- DHCP **tidak diaktifkan** pada switch
- Controller & switch menggunakan **Static IP**
- ISP Router (TM) hanya bertindak sebagai upstream internet
- Semua pengurusan dilakukan melalui Omada Controller

---

> "Network yang kemas bermula dengan IP plan yang jelas."
