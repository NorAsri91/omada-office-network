# Internet Edge Configuration (TM Router + DMZ)
----------------------------------------------

Dokumen ini menerangkan konfigurasi *Internet Edge* bagi rangkaian pejabat
yang menggunakan perkhidmatan ISP TM dan TP-Link ER605 sebagai router utama
untuk rangkaian dalaman.

Internet Edge merujuk kepada lapisan paling luar rangkaian yang berfungsi
sebagai pintu masuk dan keluar trafik ke Internet.

---

## 1. Peranan Router TM

Router TM dikekalkan sebagai peranti ISP termination kerana kekangan
perkhidmatan dan polisi ISP. Walau bagaimanapun, router ini **tidak digunakan**
untuk pengurusan rangkaian dalaman.

Peranan router TM dihadkan kepada:
- Menyediakan sambungan Internet daripada ISP
- Memberikan alamat IP WAN kepada ER605
- Bertindak sebagai *upstream gateway* sahaja

Semua fungsi pengurusan rangkaian dalaman dipindahkan sepenuhnya kepada
TP-Link ER605 dan Omada ecosystem.

---

## 2. Kenapa Konfigurasi DMZ Digunakan

Router TM yang digunakan tidak menyokong *full bridge mode*.
Bagi mengelakkan isu *double NAT* dan konflik firewall,
konfigurasi **DMZ (Demilitarized Zone)** digunakan.

DMZ membolehkan semua trafik inbound daripada Internet
dihantar terus ke router utama (ER605).

Pendekatan ini memastikan:
- ER605 menjadi router sebenar untuk rangkaian dalaman
- Semua firewall, NAT dan routing dikawal di satu tempat
- Rangkaian dalaman lebih mudah diurus dan troubleshoot

---

## 3. Konfigurasi DMZ

Maklumat konfigurasi adalah seperti berikut:

- TM Router LAN IP: `192.168.0.1`
- ER605 WAN IP: `192.168.0.99`
- DMZ Host (pada TM Router): `192.168.0.99`

Dengan konfigurasi ini, semua trafik daripada Internet
akan diteruskan terus ke ER605 tanpa ditapis oleh router TM.

---

## 4. Aliran Trafik Internet (Traffic Flow)

Aliran trafik sebenar adalah seperti berikut:

Internet  
→ TM Router (ISP Gateway)  
→ DMZ  
→ ER605 (WAN)  
→ ER605 (LAN – VLAN 10)  
→ Switch / Access Point / Clients  

Walaupun secara teknikal masih terdapat *double NAT*,
kesannya adalah minimum kerana semua kawalan trafik sebenar
dilaksanakan di ER605.

---

## 5. Akses Pengurusan Router TM

Selepas konfigurasi DMZ:
- Router TM **masih boleh diping** dari rangkaian dalaman
- Akses ke Web UI router TM **mungkin disekat** dari subnet lain

Ini adalah tingkah laku normal dan bertujuan untuk keselamatan.
Untuk mengakses dashboard router TM:
- Sambungkan peranti terus ke WiFi atau LAN router TM
- Atau gunakan konfigurasi IP sementara dalam subnet `192.168.0.0/24`

---

## 6. Rationale Rekaan (Design Rationale)

Pendekatan ini dipilih kerana:
- Mengikut senario dunia sebenar rangkaian pejabat di Malaysia
- Tidak melanggar kekangan ISP
- Memberikan kawalan penuh kepada administrator rangkaian
- Mudah dikembangkan tanpa perlu mengubah konfigurasi ISP

Router TM dikekalkan sebagai *Internet Edge*,
manakala ER605 bertindak sebagai **teras sebenar rangkaian (Core Router)**.

---

> "Router ISP bukan musuh.
> Ia perlu difahami, dikawal, dan diasingkan dengan betul."
