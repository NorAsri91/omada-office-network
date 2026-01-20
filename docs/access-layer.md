# Access Layer Design & Implementation

Dokumen ini menerangkan reka bentuk dan konfigurasi **Access Layer** bagi rangkaian pejabat
yang dibina menggunakan ekosistem TP-Link Omada.

Access Layer merupakan lapisan paling hampir dengan pengguna akhir (end-user),
dan bertanggungjawab menyediakan sambungan rangkaian berwayar dan tanpa wayar
kepada peranti seperti laptop, PC, dan peranti WiFi.

---

## 1. Definisi Access Layer

Dalam seni bina rangkaian berlapis (network hierarchy), Access Layer berfungsi untuk:

- Menyambungkan peranti pengguna ke rangkaian
- Menyediakan sambungan fizikal (Ethernet) dan tanpa wayar (WiFi)
- Menguatkuasakan konfigurasi VLAN pada peringkat port
- Menjadi titik pertama trafik klien sebelum dihantar ke Distribution / Core Layer

Dalam deployment ini, Access Layer direalisasikan melalui:
- TP-Link EAP610 Access Point
- Port access pada switch (SG3428)

---

## 2. Komponen Access Layer

### 2.1 Access Point
Model yang digunakan:
- **TP-Link EAP610** (x2 unit)

Penempatan:
- AP 1st Floor
- AP 2nd Floor

Fungsi utama:
- Menyediakan sambungan WiFi kepada pengguna
- Semua SSID dipetakan kepada VLAN 10
- Diurus sepenuhnya oleh Omada Controller (OC200)

---

### 2.2 Switch Access Ports

Switch yang terlibat:
- Switch 1st Floor (SG3428)
- Switch 2nd Floor (SG3428)

Ciri port Access Layer:
- Port diset sebagai **Access Port**
- VLAN: **VLAN 10**
- Tiada trunk diperlukan pada port klien
- DHCP tidak dijalankan pada switch (dikendalikan oleh ER605)

---

## 3. VLAN Configuration at Access Layer

Access Layer menggunakan **satu VLAN sahaja** buat masa ini:

| VLAN ID | Purpose            | Subnet           |
|-------|--------------------|------------------|
| 10    | Omada / Office LAN | 192.168.10.0/24 |

Semua peranti berikut berada dalam VLAN 10:
- Access Point
- Klien WiFi
- Klien berwayar (PC, laptop, printer)
- Switch management IP
- Omada Controller

Pendekatan single VLAN ini dipilih untuk:
- Memudahkan pengurusan awal
- Mengurangkan kerumitan troubleshooting
- Membina asas yang stabil sebelum segmentasi lanjut

---

## 4. IP Addressing Strategy (Access Devices)

Access Layer menggunakan IP statik untuk peranti infrastruktur,
dan DHCP untuk klien pengguna.

### 4.1 Infrastruktur (Static IP)

| Device            | IP Address       |
|------------------|------------------|
| AP 1st Floor     | 192.168.10.3     |
| AP 2nd Floor     | 192.168.10.4     |
| Switch 1st Floor | 192.168.10.5     |
| Switch 2nd Floor | 192.168.10.6     |

### 4.2 Klien (DHCP)

- Laptop, PC, dan peranti WiFi menerima IP secara DHCP
- DHCP Server: **ER605 Router**
- DHCP Scope: `192.168.10.100 – 192.168.10.254` (contoh)

---

## 5. Wireless Access Design

Reka bentuk WiFi adalah ringkas dan berfokus kepada kestabilan:

- Satu SSID utama untuk kegunaan pejabat
- SSID dipetakan terus ke VLAN 10
- Tiada VLAN tagging pada klien
- Roaming dikendalikan oleh Omada Controller

Kelebihan pendekatan ini:
- Pengalaman WiFi yang konsisten
- Konfigurasi mudah
- Mudah diskalakan pada masa hadapan

---

## 6. Management & Monitoring

Semua peranti Access Layer:
- Diadopt dan diurus oleh **Omada Controller (OC200)**
- Dipantau dari segi:
  - Status sambungan
  - Jumlah klien
  - Penggunaan port
  - Kualiti sambungan WiFi

Sebarang perubahan konfigurasi:
- Dilakukan melalui Controller
- Diedarkan secara automatik ke semua peranti

---

## 7. Design Rationale

Beberapa pertimbangan utama dalam reka bentuk Access Layer ini:

- Mengutamakan kestabilan berbanding kompleksiti
- Mengelakkan over-engineering pada fasa awal
- Menyediakan asas kukuh untuk:
  - Penambahan VLAN (Guest, Management, IoT)
  - Polisi keselamatan
  - QoS dan access control

Access Layer ini direka untuk berkembang,
bukan sekadar berfungsi untuk hari ini.
