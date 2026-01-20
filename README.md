# Omada Office Network Deployment
--------------------------------------

Bukan sekadar pasang peranti, tetapi satu proses membina rangkaian yang stabil,
mudah diurus, dan bersedia untuk berkembang.

Repository ini mendokumentasikan satu deployment rangkaian pejabat menggunakan
TP-Link Omada ecosystem — dari perancangan IP, pemasangan fizikal, hinggalah
kepada pengurusan terpusat melalui controller.

## Kenapa dokumentasi ini diwujudkan?
Dalam dunia sebenar IT Networking, kerja tidak berhenti pada “internet dah hidup”.
Apa yang lebih penting ialah:
- Struktur rangkaian yang jelas
- IP addressing yang kemas
- Peranti yang mudah diurus & troubleshoot
- Knowledge yang boleh dirujuk semula

Dokumentasi ini diwujudkan sebagai rujukan teknikal dan juga rekod pembelajaran.

---

## Peralatan Digunakan (Omada Stack)
1. **ER605** – Gateway Router  
   - Routing, NAT, DHCP, inter-VLAN foundation

2. **OC200** – Omada Hardware Controller  
   - Centralized management untuk router, switch & access point

3. **SG3428 (x2)** – Managed Switch  
   - Core distribution (1st Floor & 2nd Floor)

4. **EAP610 (x2)** – Wi-Fi 6 Access Point  
   - Wireless coverage berasingan ikut tingkat

---

## Gambaran Topologi (High Level)

Internet (TM Router)  
→ ER605 (WAN)  
→ ER605 (LAN)  
  ├── SG3428 – 1st Floor → AP + Wired Clients  
  ├── SG3428 – 2nd Floor → AP + Wired Clients  
  └── OC200 – Controller

> Semua peranti diurus dari satu dashboard Omada Controller.

---

## Rancangan IP (Ringkas)
- Subnet: `192.168.10.0/24`
- ER605 (LAN Gateway): `192.168.10.1`
- OC200 Controller: `192.168.10.2` (Static)
- Switches: `192.168.10.3 – 192.168.10.4` (Static)
- AP & Client: DHCP

---

## Pengajaran & Nota Penting
- DHCP hanya diaktifkan di **gateway (ER605)**.
- Controller & switch **wajib static IP** untuk elak isu management.
- ISP router (TM) hanya bertindak sebagai upstream, bukan core network.
- Visual topology dalam Omada tidak semestinya 100% fizikal — ia berdasarkan
  MAC learning & uplink detection.

---

## Media & Dokumentasi Tambahan
- Gambar pemasangan fizikal: `docs/photos/`
- IP Plan: `docs/ip-plan.md`
- VLAN / Network Notes: `docs/vlan-plan.md`
- Troubleshooting: `docs/troubleshooting.md`

---

> “Network yang baik bukan yang paling mahal,
> tapi yang paling mudah difahami dan diurus.”
