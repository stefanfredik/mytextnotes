---
description: Berisi catatan singkat terkati istilah dan masalah pada OLT
icon: ethernet
---

# OLT

## Indikator OLT

Alarms terkait ODN (Optical Distribution Network) mengacu pada masalah yang terjadi dalam jaringan distribusi optik, terutama antara OLT (Optical Line Terminal) dan ONT (Optical Network Terminal). Alarms ini mengindikasikan adanya gangguan atau anomali dalam komunikasi optik, yang dapat disebabkan oleh masalah perangkat keras, sinyal, atau bahkan kabel serat optik yang terputus. Mari kita uraikan maksud dari setiap alarm beserta contoh kasusnya:

#### 1. **0x2e112002: Loss of GEM Channel Delineation (LCDGi) Occurs**

**Deskripsi:** GEM (GPON Encapsulation Method) channel delineation hilang. Hal ini berarti ada masalah dalam penguraian saluran GEM yang digunakan untuk mengirimkan data antara OLT dan ONT.

**Contoh kasus:** Misalkan dalam sebuah jaringan GPON, beberapa pelanggan melaporkan gangguan internet secara tiba-tiba. Setelah dicek, terdeteksi alarm **0x2e112002** pada ONT pelanggan. Ini menunjukkan bahwa ada masalah pada jalur data GEM yang menyebabkan data tidak dapat diterima dengan benar.

***

#### 2. **0x2e112003: Signal Degrade of ONTi (SDi) Occurs**

**Deskripsi:** Terjadi degradasi sinyal di ONT. Degradasi ini menunjukkan bahwa kualitas sinyal optik yang diterima ONT menurun di bawah ambang batas tertentu, meskipun sinyal masih ada.

**Contoh kasus:** Seorang teknisi menemukan bahwa salah satu ONT pelanggan mengalami penurunan kecepatan internet. Alarm **0x2e112003** muncul, mengindikasikan bahwa meskipun sinyal masih ada, kualitasnya sudah menurun, mungkin disebabkan oleh redaman yang tinggi pada kabel optik atau kotoran di konektor optik.

***

#### 3. **0x2e112004: Signal Fail of ONTi (SFi) Occurs**

**Deskripsi:** Sinyal di ONT hilang sama sekali. Ini merupakan alarm yang lebih serius dibandingkan degradasi sinyal, karena menunjukkan bahwa ONT tidak dapat menerima sinyal optik sama sekali.

**Contoh kasus:** Jika sebuah ONT di rumah pelanggan tiba-tiba kehilangan semua koneksi internet dan alarm **0x2e112004** muncul, ini bisa menunjukkan bahwa ada gangguan fisik pada kabel serat optik, seperti kabel yang putus atau koneksi optik yang terlepas.

***

#### 4. **0x2e112006: Loss of Frame of ONTi (LOFi) Occurs**

**Deskripsi:** Terjadi kehilangan frame di ONT. Frame di sini mengacu pada unit data optik yang dikirim antara OLT dan ONT. Kehilangan frame dapat menyebabkan data tidak dapat diproses dengan benar.

**Contoh kasus:** Pelanggan mengeluh bahwa koneksi internet mereka tidak stabil. Setelah dilakukan pengecekan, alarm **0x2e112006** terdeteksi di OLT. Ini menunjukkan bahwa ONT kehilangan frame data, mungkin karena gangguan optik yang sporadis, seperti interferensi atau kabel yang tidak terhubung dengan baik.

***

#### 5. **0x2e11a001: The Feed Fiber is Broken or OLT Cannot Receive Any Expected Optical Signals (LOS)**

**Deskripsi:** Fiber optik utama (feed fiber) yang menghubungkan OLT ke jaringan distribusi terputus, atau OLT tidak dapat menerima sinyal optik yang diharapkan (Loss of Signal - LOS).

**Contoh kasus:** Jaringan di seluruh lingkungan tiba-tiba mengalami gangguan besar. Alarm **0x2e11a001** muncul, menunjukkan bahwa kabel serat optik utama yang menghubungkan OLT ke jaringan backbone terputus, mungkin karena penggalian jalan yang tidak disengaja.

***

#### 6. **0x2e112007: The Distribute Fiber is Broken or the OLT Cannot Receive Expected Optical Signals from the ONT (LOSi/LOBi)**

**Deskripsi:** Serat optik distribusi (distribute fiber) yang menghubungkan OLT ke ONT terputus, atau OLT tidak dapat menerima sinyal optik yang diharapkan dari ONT (Loss of Signal Indicator - LOSi).

**Contoh kasus:** Seorang pelanggan di lokasi tertentu kehilangan koneksi internet. Alarm **0x2e112007** menunjukkan bahwa kabel serat optik distribusi yang menghubungkan OLT ke rumah pelanggan mungkin terputus karena kabel tersebut tertarik atau rusak secara fisik.

***

#### 7. **0x2e314021: There are Illegal Incursionary Rogue ONTs Under the Port**

**Deskripsi:** OLT mendeteksi adanya ONT "rogue" (ONT liar) yang mencoba terhubung ke port jaringan. ONT rogue adalah perangkat ONT yang tidak sah atau berperilaku buruk, yang dapat menyebabkan gangguan pada jaringan GPON.

**Contoh kasus:** Seorang administrator jaringan mendeteksi alarm **0x2e314021** yang menunjukkan adanya ONT rogue di jaringan. ONT rogue ini mungkin merupakan perangkat ONT yang tidak diotorisasi yang dipasang oleh pihak ketiga tanpa izin.

***

#### 8. **0x2e314022: The ONT is a Rogue ONT**

**Deskripsi:** ONT yang bersangkutan dikategorikan sebagai ONT rogue. Ini bisa terjadi jika ONT berperilaku tidak normal atau menyebabkan interferensi di jaringan.

**Contoh kasus:** Setelah dilakukan pemantauan, alarm **0x2e314022** muncul pada salah satu ONT. Ini bisa berarti bahwa perangkat ONT tersebut terinfeksi malware atau berusaha mengirimkan lalu lintas yang tidak sah, sehingga dianggap sebagai ONT rogue oleh OLT.

***

#### Kesimpulan:

Alarms ini membantu dalam memonitor dan menjaga kualitas serta stabilitas jaringan GPON. Dengan memahami setiap alarm, teknisi dapat mendiagnosis masalah dengan lebih efektif dan cepat mengambil tindakan yang tepat.



Berikut penjelasan **detail, terstruktur, dan komprehensif** mengenai **T-CONT** dan **GEM Port** pada OLT (khususnya konteks **GPON / XG-PON / XGS-PON**), dengan sudut pandang **operasional ISP**.

***

### 1. Gambaran Besar Arsitektur GPON

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

### 2. T-CONT (Traffic Container)

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

### 3. GEM Port (GPON Encapsulation Method Port)

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

### 4. Relasi T-CONT dan GEM Port

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
