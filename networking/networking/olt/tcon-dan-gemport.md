---
icon: network-wired
---

# TCON dan Gemport

## Gambaran Besar Arsitektur GPON

Pada GPON, ada dua jalur logis utama:

| Arah                       | Fungsi utama                         |
| -------------------------- | ------------------------------------ |
| **Downstream (OLT → ONU)** | Broadcast (1 ke banyak)              |
| **Upstream (ONU → OLT)**   | TDMA (Time Division Multiple Access) |

Untuk mengatur **upstream** agar rapi, adil, dan sesuai SLA, GPON menggunakan:

* **T-CONT (Traffic Container)** → _manajemen bandwidth upstream_
* **GEM Port (GPON Encapsulation Method Port)** → _container logis untuk data user (upstream & downstream)_

> **Ringkas:**\
> **T-CONT = kapan & berapa besar ONU boleh kirim data (upstream)**\
> **GEM Port = data apa yang dikirim & ke layanan mana**

***

## T-CONT (Traffic Container)

#### 2.1 Definisi

**T-CONT** adalah **entitas logis pada ONU** yang digunakan OLT untuk:

* Mengatur **alokasi bandwidth upstream**
* Mengimplementasikan **Dynamic Bandwidth Allocation (DBA)**

Setiap T-CONT:

* Memiliki **T-CONT ID (Alloc-ID)**
* Diatur sepenuhnya oleh **OLT**
* Hanya berlaku untuk **upstream traffic**

***

#### 2.2 Fungsi Utama T-CONT

| Fungsi              | Penjelasan                          |
| ------------------- | ----------------------------------- |
| Bandwidth control   | Mengatur kecepatan upload pelanggan |
| QoS upstream        | Menentukan prioritas traffic        |
| SLA enforcement     | Menjamin CIR dan membatasi PIR      |
| Collision avoidance | Menghindari tabrakan upstream       |

***

#### 2.3 Tipe-Tipe T-CONT (ITU-T G.984)

**T-CONT Type 1 – Fixed Bandwidth**

* Bandwidth **tetap & terjamin**
* Tidak dipengaruhi traffic lain
* Cocok untuk:
  * Voice (VoIP)
  * Signaling
  * Management

Contoh:

```
VoIP SIP, OMCI, TR-069
```

***

**T-CONT Type 2 – Assured Bandwidth**

* Memiliki **jaminan minimum (CIR)**
* Bisa mendapatkan tambahan bandwidth jika tersedia
* Cocok untuk:
  * IPTV
  * Video conference

***

**T-CONT Type 3 – Assured + Best Effort**

* Kombinasi:
  * CIR (jaminan)
  * PIR (maksimum)
* Paling umum digunakan ISP

Contoh:

```
Paket 50 Mbps Up:
CIR = 10 Mbps
PIR = 50 Mbps
```

***

**T-CONT Type 4 – Best Effort**

* Tanpa jaminan bandwidth
* Menggunakan sisa bandwidth
* Cocok untuk:
  * Internet reguler
  * Traffic background

***

**T-CONT Type 5 – Mixed**

* Gabungan beberapa tipe
* Digunakan untuk layanan kompleks (jarang diimplementasikan penuh)

***

#### 2.4 Relasi T-CONT dengan DBA

Alur DBA:

1. ONU mengirim **buffer occupancy report**
2. OLT menghitung alokasi waktu
3. OLT mengirim **BWmap**
4. ONU mengirim data sesuai slot waktu

> Tanpa T-CONT → upstream akan chaos

***

#### 2.5 Best Practice Implementasi ISP

| Layanan            | T-CONT     |
| ------------------ | ---------- |
| Management         | Type 1     |
| Voice              | Type 1 / 2 |
| IPTV               | Type 2     |
| Internet pelanggan | Type 3 / 4 |

***

## GEM Port (GPON Encapsulation Method Port)

#### 3.1 Definisi

**GEM Port** adalah **logical tunnel** di GPON untuk:

* Membawa data user
* Mengidentifikasi layanan
* Melakukan multiplexing traffic

GEM Port:

* Berlaku **dua arah** (upstream & downstream)
* Memiliki **GEM Port ID**
* Dapat dienkripsi (AES-128)

***

#### 3.2 Fungsi Utama GEM Port

| Fungsi             | Penjelasan                      |
| ------------------ | ------------------------------- |
| Service separation | Memisahkan internet, IPTV, VoIP |
| VLAN transport     | Membawa VLAN ID                 |
| QoS marking        | Pemetaan priority               |
| Security           | Encryption per ONU              |

***

#### 3.3 Jenis GEM Port

| Jenis                  | Keterangan             |
| ---------------------- | ---------------------- |
| **Unicast GEM Port**   | Data spesifik satu ONU |
| **Multicast GEM Port** | IPTV multicast         |

***

#### 3.4 Relasi GEM Port dengan Service

Contoh pemetaan:

| Service  | VLAN | GEM Port |
| -------- | ---- | -------- |
| Internet | 100  | GEM 4001 |
| IPTV     | 200  | GEM 4002 |
| VoIP     | 300  | GEM 4003 |

***

#### 3.5 Downstream vs Upstream

| Arah       | Peran GEM                                    |
| ---------- | -------------------------------------------- |
| Downstream | OLT broadcast, ONU filter berdasarkan GEM ID |
| Upstream   | GEM Port dipetakan ke T-CONT                 |

***

## Relasi T-CONT dan GEM Port

Ini bagian paling penting dan sering membingungkan.

#### 4.1 Hubungan Logis

```
Service
  ↓
GEM Port
  ↓
T-CONT
  ↓
DBA Scheduler
```

#### 4.2 Diagram Sederhana

```
[Internet VLAN]
     |
   GEM 4001
     |
   T-CONT 3 (Assured + BE)
     |
   OLT DBA
```

***

#### 4.3 Mapping Many-to-One

* Banyak **GEM Port** bisa masuk ke **1 T-CONT**
* Tapi **1 GEM Port hanya ke 1 T-CONT**

Contoh:

```
GEM 4001 (Internet)
GEM 4002 (IPTV)
GEM 4003 (VoIP)
       ↓
     T-CONT 3
```

***

### 5. Implementasi Nyata di OLT Vendor (ZTE/Huawei/FiberHome)

Umumnya alur konfigurasi:

1. Buat **DBA profile**
2. Buat **T-CONT**
3. Buat **GEM Port**
4. Mapping VLAN ↔ GEM
5. Bind GEM ↔ T-CONT
6. Apply ke ONU profile

***

### 6. Kesalahan Umum di Lapangan

| Masalah                     | Penyebab            |
| --------------------------- | ------------------- |
| Upload kecil                | T-CONT salah tipe   |
| IPTV patah-patah            | GEM multicast salah |
| Bandwidth tidak sesuai      | DBA profile salah   |
| ONU online tapi no internet | GEM–VLAN mismatch   |

***

### 7. Ringkasan Super Singkat

| Elemen       | Fungsi                               |
| ------------ | ------------------------------------ |
| **T-CONT**   | Mengatur _kapan & berapa_ ONU upload |
| **GEM Port** | Mengatur _data apa & layanan apa_    |
| **DBA**      | Otak pengatur upstream               |
| **VLAN**     | Identitas layanan                    |

***



Berikut **diagram implementasi T-CONT dan GEM Port** yang **lengkap, berlapis, dan realistis**, disusun dari sudut pandang **implementasi ISP FTTH (GPON/XGS-PON)**.

Saya bagi menjadi **4 level diagram** agar mudah dipahami dari konsep → implementasi nyata.

***

### 1️⃣ Diagram Konsep Dasar (Logical View)

```
           ┌──────────────┐
           │   SERVICES   │
           │──────────────│
           │ Internet     │
           │ IPTV         │
           │ VoIP         │
           └──────┬───────┘
                  │
          (Service Identification)
                  │
           ┌──────▼───────┐
           │   GEM PORTS  │
           │──────────────│
           │ GEM 4001     │ → Internet
           │ GEM 4002     │ → IPTV
           │ GEM 4003     │ → VoIP
           └──────┬───────┘
                  │
          (Upstream Scheduling)
                  │
           ┌──────▼───────┐
           │   T-CONT     │
           │──────────────│
           │ Type 1       │ → Voice / Mgmt
           │ Type 2       │ → IPTV
           │ Type 3       │ → Internet
           └──────┬───────┘
                  │
           ┌──────▼───────┐
           │     DBA      │
           │  Scheduler   │
           └──────┬───────┘
                  │
           ┌──────▼───────┐
           │   GPON PON   │
           └──────────────┘
```

🔑 **Inti konsep:**

* **GEM Port = identitas traffic**
* **T-CONT = kontrol bandwidth upstream**
* **DBA = pengatur waktu kirim ONU**

***

### 2️⃣ Diagram Implementasi ONU (Real-World ONU Perspective)

```
┌───────────────────────────────────────┐
│                 ONU                   │
│                                       │
│  ┌──────────┐   ┌──────────┐          │
│  │ Ethernet │   │  WiFi    │          │
│  │  LAN     │   │  Clients │          │
│  └────┬─────┘   └────┬─────┘          │
│       │              │                │
│       └──────┬───────┘                │
│              │                        │
│        ┌─────▼─────┐                  │
│        │ VLAN Tag  │                  │
│        │────────── │                  │
│        │ VLAN 100  │ → Internet       │
│        │ VLAN 200  │ → IPTV           │
│        │ VLAN 300  │ → VoIP           │
│        └─────┬─────┘                  │
│              │                        │
│        ┌─────▼─────┐                  │
│        │ GEM PORT  │                  │
│        │────────── │                  │
│        │ GEM 4001  │ → Internet       │
│        │ GEM 4002  │ → IPTV           │
│        │ GEM 4003  │ → VoIP           │
│        └─────┬─────┘                  │
│              │                        │
│        ┌─────▼─────┐                  │
│        │ T-CONT    │                  │
│        │────────── │                  │
│        │ T1 Voice  │ Type 1           │
│        │ T2 IPTV   │ Type 2           │
│        │ T3 Data   │ Type 3           │
│        └─────┬─────┘                  │
│              │                        │
│        ┌─────▼─────┐                  │
│        │ GPON PHY  │                  │
│        └───────────┘                  │
└───────────────────────────────────────┘
```

🔎 **Catatan penting:**

* ONU **tidak menentukan bandwidth**
* ONU hanya **mengirim sesuai izin OLT (DBA)**

***

### 3️⃣ Diagram OLT – ONU (End-to-End Flow)

```
                    OLT
┌─────────────────────────────────────────────┐
│                                             │
│  ┌──────────────┐     ┌─────────────────┐  │
│  │ Service Port │────▶│ VLAN Mapping    │  │
│  │──────────────│     │ 100/200/300     │  │
│  └──────────────┘     └───────┬─────────┘  │
│                                │            │
│                        ┌───────▼────────┐   │
│                        │ GEM PORT TABLE │   │
│                        │─────────────── │   │
│                        │ 4001 Internet  │   │
│                        │ 4002 IPTV      │   │
│                        │ 4003 VoIP      │   │
│                        └───────┬────────┘   │
│                                │            │
│                        ┌───────▼────────┐   │
│                        │ T-CONT TABLE   │   │
│                        │─────────────── │   │
│                        │ Alloc-ID 1024  │   │
│                        │ Type 3 (50M)   │   │
│                        └───────┬────────┘   │
│                                │            │
│                        ┌───────▼────────┐   │
│                        │ DBA Scheduler  │   │
│                        └───────┬────────┘   │
│                                │            │
└────────────────────────────────┼────────────┘
                                 │
                                 ▼
                      GPON PON (1:32 / 1:64)
                                 │
                                 ▼
                               ONU
```

***

### 4️⃣ Diagram Mapping Nyata ISP (Paling Realistis)

```
CUSTOMER PACKAGE: 50 Mbps

┌─────────────────────────────────────┐
│ INTERNET SERVICE                    │
│ VLAN 100                            │
│ GEM PORT 4001                       │
│ T-CONT Type 3                       │
│ CIR 10 Mbps / PIR 50 Mbps           │
└──────────────┬──────────────────────┘
               │
┌──────────────▼──────────────────────┐
│ IPTV SERVICE                         │
│ VLAN 200                             │
│ GEM PORT 4002 (Multicast)            │
│ T-CONT Type 2                        │
│ Assured 5 Mbps                      │
└──────────────┬──────────────────────┘
               │
┌──────────────▼──────────────────────┐
│ VOIP SERVICE                         │
│ VLAN 300                             │
│ GEM PORT 4003                       │
│ T-CONT Type 1                       │
│ Fixed 256 Kbps                      │
└──────────────┬──────────────────────┘
               │
               ▼
        DBA OLT Scheduler
```

***

### 5️⃣ Ringkasan Diagram dalam 1 Kalimat

> **VLAN mengidentifikasi layanan → GEM Port membawa traffic → T-CONT mengatur bandwidth upstream → DBA menentukan kapan ONU boleh mengirim data**

***



## DBA Profil

Berikut penjelasan **sangat detail, terstruktur, dan komprehensif** tentang **DBA (Dynamic Bandwidth Allocation)** pada **OLT GPON / XG-PON / XGS-PON**, disusun dari sudut pandang **desain, implementasi, operasi, dan troubleshooting ISP**.

***

## 1. Apa itu DBA (Dynamic Bandwidth Allocation)

**DBA** adalah **mekanisme inti di OLT** yang bertugas:

* Mengatur **alokasi bandwidth upstream**
* Menentukan **urutan & waktu transmisi ONU**
* Menjamin **QoS dan SLA layanan**

Tanpa DBA:

* Semua ONU akan mengirim upstream bersamaan
* Terjadi collision
* GPON tidak bisa berfungsi

> **DBA = “Traffic Controller” upstream di GPON**

***

## 2. Kenapa DBA Sangat Penting di GPON

Karakteristik upstream GPON:

* Media **shared**
* Topologi **point-to-multipoint**
* Kapasitas terbatas (2.5 Gbps GPON, 10 Gbps XGS-PON)

DBA memastikan:

* Fairness antar pelanggan
* Prioritas layanan real-time
* Optimalisasi kapasitas

***

## 3. Komponen DBA di OLT

```
ONU Buffers → Bandwidth Report → DBA Engine → BWmap → ONU Transmission
```

#### Komponen utama:

| Komponen         | Fungsi                          |
| ---------------- | ------------------------------- |
| Buffer occupancy | Informasi jumlah data di ONU    |
| T-CONT           | Kontainer traffic upstream      |
| DBA Profile      | Policy alokasi bandwidth        |
| DBA Engine       | Algoritma pengambilan keputusan |
| BWmap            | Jadwal waktu kirim ONU          |

***

## 4. Alur Kerja DBA (Step-by-Step)

#### 4.1 Buffer Reporting (ONU → OLT)

ONU melaporkan:

* Panjang antrian per T-CONT
* Prioritas traffic

Format laporan:

* Embedded in upstream burst
* Dikirim periodik

***

#### 4.2 DBA Processing (OLT Internal)

OLT melakukan:

1. Membaca laporan buffer
2. Mengacu ke:
   * T-CONT type
   * DBA profile
   * SLA
3. Menghitung:
   * Slot waktu
   * Ukuran burst

***

#### 4.3 BWmap Distribution (OLT → ONU)

OLT mengirim:

* **BWmap** di downstream frame
* Berisi:
  * Alloc-ID
  * Start time
  * Duration

ONU hanya boleh transmit sesuai BWmap.

***

#### 4.4 Upstream Transmission (ONU → OLT)

ONU:

* Mengirim data tepat waktu
* Tidak boleh melebihi alokasi
* Tidak overlap dengan ONU lain

***

## 5. Jenis DBA Berdasarkan T-CONT

### 5.1 T-CONT Type 1 – Fixed DBA

| Karakteristik | Keterangan       |
| ------------- | ---------------- |
| Bandwidth     | Tetap            |
| Latency       | Sangat rendah    |
| Contoh        | Voice, signaling |

📌 **Selalu dialokasikan meskipun tidak ada data**

***

### 5.2 T-CONT Type 2 – Assured DBA

| Karakteristik | Keterangan    |
| ------------- | ------------- |
| CIR           | Dijamin       |
| Extra BW      | Jika tersedia |
| Contoh        | IPTV, Video   |

📌 **Prioritas di atas Best Effort**

***

### 5.3 T-CONT Type 3 – Hybrid DBA

| Karakteristik | Keterangan      |
| ------------- | --------------- |
| CIR           | Dijamin         |
| PIR           | Maksimum        |
| Best practice | Internet access |

📌 **Paling umum di ISP FTTH**

***

### 5.4 T-CONT Type 4 – Best Effort DBA

| Karakteristik | Keterangan            |
| ------------- | --------------------- |
| Guarantee     | Tidak ada             |
| Allocation    | Sisa bandwidth        |
| Contoh        | Internet low priority |

***

Dalam arsitektur GPON, T-CONT dan GEM Port adalah dua komponen logis yang berfungsi sebagai "jalan" atau "wadah" untuk mengirimkan data antara OLT (sisi ISP) dan ONT (sisi pelanggan).

Analogi sederhananya: Jika data Anda adalah barang kiriman, maka GEM Port adalah kardus/paketnya, sedangkan T-CONT adalah truk pengangkutnya.

***

### 1. T-CONT (Transmission Container)

T-CONT adalah unit dasar untuk manajemen bandwidth pada jalur Upstream (dari pelanggan ke pusat). Fungsinya adalah untuk mengatur alokasi kecepatan (bandwidth) agar tidak terjadi tabrakan data antar pelanggan.

* Tujuan Utama: Mengelola QoS (Quality of Service) dan DBA (Dynamic Bandwidth Allocation).
* Cara Kerja: OLT memberikan "izin" (timeslot) kepada setiap T-CONT untuk mengirimkan data pada waktu tertentu.
* Tipe T-CONT: Ada 5 tipe standar yang menentukan prioritas bandwidth:
  1. Type 1 (Fixed): Bandwidth tetap, biasanya untuk layanan sangat kritis seperti VOIP.
  2. Type 2 (Assured): Bandwidth yang dijamin tersedia.
  3. Type 3 (Non-Assured): Bandwidth minimal dijamin, tapi bisa lebih jika trafik longgar.
  4. Type 4 (Best Effort): Tidak ada jaminan, hanya menggunakan sisa bandwidth (biasanya untuk Internet biasa).
  5. Type 5 (Superset): Gabungan dari semua tipe di atas.

***

### 2. GEM Port (GPON Encapsulation Method Port)

GEM Port adalah unit terkecil yang membawa layanan (_service_) spesifik. Jika T-CONT mengatur "kapan" data dikirim, GEM Port mengatur "data apa" yang sedang dikirim.

* Tujuan Utama: Melakukan enkapsulasi (pembungkusan) frame Ethernet ke dalam format GPON.
* Identifikasi: Setiap GEM Port memiliki GEM Port ID yang unik (0–4095) dalam satu interface PON.
* Fungsi Layanan: Biasanya satu jenis layanan dipisahkan oleh GEM Port yang berbeda. Contoh:
  * GEM Port 1: Membawa data Internet (VLAN 100).
  * GEM Port 2: Membawa data IPTV (VLAN 200).
  * GEM Port 3: Membawa data VOIP (VLAN 300).

***

### 3. Hubungan Mapping: T-CONT vs GEM Port

Di dalam konfigurasi OLT, keduanya saling terikat. Berikut adalah aturan pemetaannya:

1. Satu T-CONT bisa menampung banyak GEM Port.
2. Satu GEM Port hanya bisa dimiliki oleh satu T-CONT.

#### Alur Data (Upstream):

1. Data dari PC pelanggan masuk ke ONT.
2. ONT membungkus data tersebut ke dalam GEM Port berdasarkan VLAN-nya.
3. Beberapa GEM Port dikelompokkan ke dalam satu T-CONT.
4. OLT memberikan izin kepada T-CONT untuk mengirimkan seluruh "isi truk" tersebut ke jaringan pusat sesuai dengan profil bandwidth yang sudah diatur.

***

### Perbandingan Ringkas

| **Fitur**    | **T-CONT**                 | **GEM Port**                       |
| ------------ | -------------------------- | ---------------------------------- |
| Fungsi Fokus | Manajemen Bandwidth & QoS  | Enkapsulasi Layanan & VLAN         |
| Identifikasi | Alloc-ID                   | GEM Port ID                        |
| Analogi      | Truk / Kontainer           | Kardus / Paket Layanan             |
| Arah Utama   | Sangat krusial di Upstream | Digunakan di Upstream & Downstream |

Apakah Anda ingin saya buatkan contoh perintah konfigurasi (CLI) untuk membuat T-CONT dan GEM Port ini di OLT (seperti Huawei atau ZTE)?

## DBA Profile (Konfigurasi di OLT)

DBA Profile berisi:

| Parameter  | Fungsi            |
| ---------- | ----------------- |
| Fixed BW   | Bandwidth tetap   |
| Assured BW | CIR               |
| Max BW     | PIR               |
| Priority   | Urutan scheduling |
| Weight     | Fairness          |

***

### Contoh DBA Profile ISP

```
DBA-PROFILE: INTERNET_50M
Fixed     : 0
Assured   : 10 Mbps
Max       : 50 Mbps
Priority  : Medium
```

***

## 7. DBA Scheduling Algorithm

Vendor menggunakan algoritma proprietary, umumnya kombinasi:

| Algoritma           | Fungsi           |
| ------------------- | ---------------- |
| Strict Priority     | Layanan kritikal |
| WRR                 | Fairness         |
| WFQ                 | SLA granular     |
| Deficit Round Robin | Efisiensi        |

📌 Biasanya:

```
Voice → IPTV → Internet
```

***

## 8. DBA pada XG-PON & XGS-PON

| Teknologi | Upstream  |
| --------- | --------- |
| GPON      | 1.25 Gbps |
| XG-PON    | 2.5 Gbps  |
| XGS-PON   | 10 Gbps   |

Prinsip DBA **sama**, hanya:

* Kapasitas lebih besar
* Granularitas lebih tinggi
* Lebih banyak T-CONT per ONU

***

## 9. Dampak DBA ke Pengalaman Pelanggan

| Salah Konfigurasi      | Dampak             |
| ---------------------- | ------------------ |
| CIR terlalu kecil      | Upload lambat      |
| PIR terlalu besar      | Oversubscription   |
| Priority salah         | VoIP delay         |
| DBA profile sama semua | SLA tidak tercapai |

***

## 10. Monitoring & KPI DBA

Parameter penting:

| KPI                  | Indikator    |
| -------------------- | ------------ |
| Upstream utilization | >70% waspada |
| DBA delay            | Latency      |
| T-CONT drops         | Congestion   |
| BWmap efficiency     | Scheduling   |

***

## 11. Troubleshooting DBA (Lapangan)

#### Case 1: Upload tidak sesuai paket

* Cek T-CONT type
* Cek DBA profile
* Cek oversubscription

#### Case 2: IPTV freeze

* Pastikan Type 2
* Prioritas lebih tinggi
* Multicast GEM benar

#### Case 3: ONU online tapi upload nol

* Alloc-ID mismatch
* DBA profile tidak bind

***

## 12. Best Practice ISP FTTH

✅ Pisahkan T-CONT per layanan\
✅ Jangan satu DBA profile untuk semua paket\
✅ Monitor peak hour\
✅ Gunakan CIR realistis\
✅ Simpan capacity buffer 20–30%

***

## 13. Ringkasan Eksekutif

> **DBA adalah otak upstream GPON.**\
> **T-CONT adalah wadahnya.**\
> **DBA Profile adalah aturannya.**\
> **BWmap adalah jadwal mainnya.**

