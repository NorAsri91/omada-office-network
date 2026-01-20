# Core Network Architecture (ER605 + OC200)

## Pengenalan

Core network merupakan “otak” kepada keseluruhan rangkaian pejabat ini.  
Dalam deployment ini, TP-Link ER605 berfungsi sebagai **router teras (core router)** manakala OC200 digunakan sebagai **centralized network controller** untuk mengurus semua peranti Omada.

Pendekatan ini dipilih bagi memastikan rangkaian:
- Stabil dan mudah dikawal
- Mudah diskalakan (scalable)
- Tidak bergantung kepada konfigurasi manual setiap peranti

---

## Peranan Peranti Teras

### 1. TP-Link ER605 – Core Router

ER605 berfungsi sebagai:
- Default Gateway untuk semua VLAN dalaman
- Penghubung antara rangkaian dalaman dan Internet (TM)
- Penguatkuasaan polisi rangkaian (NAT, firewall, routing)

Fungsi utama:
- WAN menerima IP daripada router TM
- LAN menjadi gateway untuk VLAN 10 (Omada ecosystem)
- Routing antara rangkaian dalaman dan Internet

> ER605 tidak menggantikan router TM sepenuhnya, tetapi bertindak sebagai router utama untuk rangkaian dalaman.

---

### 2. OC200 – Omada Controller

OC200 bertindak sebagai:
- Centralized controller untuk semua peranti Omada
- Single point of management
- Penyimpan konfigurasi dan polisi rangkaian

Fungsi utama:
- Mengurus ER605, switch (SG3428), dan AP (EAP610)
- Mengawal VLAN, SSID, port profile, dan client policy
- Memberi visual network topology dan status peranti

OC200 **tidak mengawal trafik data**, tetapi mengawal:
- Konfigurasi
- Polisi
- Monitoring

---

## Kenapa Controller Dipisahkan?

Controller OC200 diletakkan sebagai peranti berasingan kerana:
- Tidak membebankan router
- Kekal berfungsi walaupun router reboot
- Konfigurasi rangkaian tidak hilang jika router reset
- Lebih sesuai untuk environment pejabat / enterprise kecil

---

## Core Network IP Design

Core network menggunakan **VLAN 10** sebagai VLAN pengurusan utama:

| Komponen | IP Address |
|--------|-----------|
| ER605 (Gateway) | 192.168.10.1 |
| OC200 | 192.168.10.2 |
| Switch 1 | 192.168.10.5 |
| Switch 2 | 192.168.10.6 |
| Access Point | DHCP / Reserved |

Subnet:
192.168.10.0/24

---

## Aliran Trafik (Traffic Flow)

1. Client → Access Point / Switch
2. Trafik masuk ke VLAN 10
3. Default gateway → ER605 (192.168.10.1)
4. ER605 NAT → Router TM
5. Router TM → Internet

Aliran ini memastikan:
- Semua polisi dikawal di ER605
- Router TM hanya berfungsi sebagai ISP edge

---

## Topologi Fizikal vs Logikal

Secara fizikal:
- Sesetengah peranti (printer, PC) disambung terus ke ER605
- Sebahagian lain melalui switch tingkat

Secara logikal (Omada):
- Semua peranti tetap berada dalam VLAN yang sama
- Controller memaparkan topologi berdasarkan **uplink logik**, bukan semata-mata fizikal

Ini menyebabkan sesetengah peranti kelihatan “terus di bawah ER605” dalam dashboard, walaupun secara fizikal melalui switch.

Ini adalah **tingkah laku normal dalam Omada Controller**.

---

## Kesimpulan

Rekaan core network ini:
- Memisahkan fungsi ISP dan internal routing
- Menggunakan centralized management
- Menyediakan asas yang kukuh untuk penambahan VLAN, polisi keselamatan, dan skala rangkaian pada masa hadapan

Core network ini menjadi asas kepada:
- Access layer
- Wireless deployment
- Client management
- Security policy
