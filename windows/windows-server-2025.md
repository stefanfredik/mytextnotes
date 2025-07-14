# Windows Server 2025

## Recconection Sesi di Windows Server 2025

Untuk mencapai **konfigurasi di Windows Server 2025** agar **setiap user hanya memiliki satu sesi RDP**, dan sesi itu **bisa berpindah perangkat tanpa membuat sesi baru** (dikenal sebagai _Session Reconnection / Session Roaming_), kamu bisa mengaturnya dengan kombinasi pengaturan di **Group Policy** dan **Remote Desktop Session Host Configuration**.

#### 🎯 Tujuan:

* Satu user = satu sesi RDP.
* Bila user login dari perangkat lain, sesi lama tetap digunakan (_reconnect_), bukan membuat sesi baru.

***

### ✅ Langkah-Langkah Konfigurasi

#### 1. **Install Remote Desktop Services Role (jika belum)**

Agar pengaturan ini berfungsi secara penuh, kamu harus menjadikan server sebagai **Remote Desktop Session Host (RDSH)**.

1. Buka **Server Manager**
2. Pilih `Add Roles and Features`
3. Pilih **Remote Desktop Services Installation**
4. Install **Remote Desktop Session Host**

***

#### 2. **Aktifkan Group Policy: Satu Sesi per User**

**a. Buka Group Policy Editor**

```bash
gpedit.msc
```

**b. Navigasi ke:**

```
Computer Configuration →
Administrative Templates →
Windows Components →
Remote Desktop Services →
Remote Desktop Session Host →
Connections
```

**c. Ubah pengaturan berikut:**

*   **Restrict Remote Desktop Services users to a single Remote Desktop Services session** → `Enabled`

    > Ini memastikan satu user hanya punya satu sesi aktif, dan jika login lagi dari perangkat lain, akan _reconnect_ ke sesi yang sama.

***

#### 3. **Aktifkan Session Reconnection dari Perangkat Berbeda**

**a. Di lokasi yang sama (`Remote Desktop Session Host → Connections`), ubah:**

* **Allow reconnections from any client** → `Enabled`
* **Set time limit for active but idle Remote Desktop Services sessions** → `Not Configured` atau sesuai kebijakan idle kamu.

***

#### 4. **(Opsional) Aktifkan melalui Registry**

Jika kamu menggunakan script otomatis atau ingin memastikan lewat registry:

```reg
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server]
"fSingleSessionPerUser"=dword:00000001
```

***

#### 5. **Restart atau Apply Policy**

Setelah semua selesai:

```bash
gpupdate /force
```

Dan **restart server** atau service RDP:

```bash
Restart-Service TermService
```

***

### 🔎 Catatan Penting:

* Pastikan **Firewall mengizinkan RDP** dan **Network Level Authentication** diaktifkan untuk keamanan.
* Untuk sesi roaming bisa berjalan lancar, perangkat klien (Laptop, HP) **harus mendukung reconnect ke session ID yang sama**, dan **network harus bisa menjangkau server** dengan koneksi stabil.
* Untuk pengelolaan banyak user dan load balancing, biasanya digunakan **RD Connection Broker**, namun untuk kasus ringan cukup dengan langkah di atas.

***

### ✅ Verifikasi

* Login user dari **Laptop** ke Windows Server via RDP.
* Biarkan sesi tetap aktif (jangan logout).
* Login dari **HP** menggunakan user yang sama.
* Harusnya: sesi di Laptop **ter-reconnect** di HP (bukan membuat sesi baru).

***

Kalau kamu pakai **Active Directory + RDS deployment dengan Connection Broker**, kamu bisa dapat manajemen sesi lebih canggih, termasuk reconnect otomatis.

Jika kamu butuh skrip PowerShell untuk automasi pengaturannya, bisa saya bantu juga.
