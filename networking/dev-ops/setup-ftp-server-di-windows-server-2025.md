---
description: Dibuat Oleh Chat GPT
---

# Setup FTP Server di Windows Server 2025

Berikut adalah **langkah-langkah lengkap konfigurasi FTP Server di Windows Server 2024** (juga berlaku untuk Windows Server 2022/2019 dengan antarmuka yang mirip):

***

#### ✅ **1. Install FTP Server via Server Manager**

1. Buka **Server Manager**.
2. Klik **"Add roles and features"**.
3. Klik **Next** hingga masuk ke bagian **"Server Roles"**.
4. Pilih:
   * **Web Server (IIS)** → klik **Add Features** jika diminta.
5. Di bagian **Role Services**, centang:
   * **FTP Server**
     * ✔ FTP Service
     * ✔ FTP Extensibility
   * **Management Tools** (pastikan **IIS Management Console** dicentang juga).
6. Klik **Next → Install**.

***

#### ✅ **2. Buat Folder untuk FTP**

Misal, buat folder di `C:\FTPShare` atau di drive lain, lalu:

* Klik kanan folder → **Properties → Sharing** dan beri hak akses.
* Keamanan (tab **Security**), pastikan user yang digunakan untuk login FTP memiliki akses **Read/Write** sesuai kebutuhan.

***

#### ✅ **3. Buat FTP Site di IIS**

1. Buka **IIS Manager** (`inetmgr`).
2. Klik kanan pada **Sites → Add FTP Site...**
3. Isi:
   * **FTP Site name**: misalnya `MyFTP`
   * **Physical path**: arahkan ke folder yang kamu buat tadi (misalnya `C:\FTPShare`)
4. Di bagian **Binding and SSL Settings**:
   * **IP Address**: pilih IP server
   * **Port**: default 21
   * SSL: pilih **No SSL** (untuk testing lokal), atau **Require SSL** jika ingin aman
5. Di bagian **Authentication and Authorization**:
   * Authentication: **Basic**
   * Authorization:
     * Allow access to: pilih **Specified users** atau **All users**
     * Permissions: centang **Read** atau **Read & Write**

***

#### ✅ **4. Tambahkan Firewall Rule (Jika Perlu)**

Buka PowerShell atau Command Prompt (admin) dan jalankan:

```powershell
# Untuk port FTP default (21)
New-NetFirewallRule -DisplayName "FTP Server Port 21" -Direction Inbound -Protocol TCP -LocalPort 21 -Action Allow

# Untuk Passive FTP (misal port 1025-1040)
New-NetFirewallRule -DisplayName "FTP Passive Ports" -Direction Inbound -Protocol TCP -LocalPort 1025-1040 -Action Allow
```

***

#### ✅ **5. Atur Passive Mode di IIS**

1. Di **IIS Manager**, klik server name (bukan site).
2. Klik **FTP Firewall Support**.
3. Di bagian **Data Channel Port Range**, isi misalnya: `1025-1040`
4. External IP: isi IP publik server (jika diakses dari luar)
5. Klik **Apply**

***

#### ✅ **6. Uji Koneksi FTP**

Gunakan FileZilla atau command line:

```bash
ftp <IP_SERVER>
```

Masukkan username dan password Windows (atau user lokal yang diatur sebelumnya).

***

#### ✅ Opsional: Buat User Khusus FTP

Jika tidak ingin pakai akun administrator:

1.  Buat user lokal baru:

    ```powershell
    net user ftpuser YourPassword123 /add
    ```
2. Beri user tersebut akses ke folder FTP.
3. Ubah pengaturan FTP site untuk izinkan user `ftpuser`.

***

Kalau kamu pakai Proxmox dan ingin menghubungkan VM Windows Server ke jaringan luar agar FTP bisa diakses publik, kamu perlu pastikan:

* VM menggunakan **bridged networking**
* Firewall di Proxmox dan VM mengizinkan port 21 + passive ports

***

Kalau kamu butuh juga setup FTPS (FTP over SSL), atau integrasi dengan Active Directory, tinggal lanjutkan dari konfigurasi dasar ini.

Mau saya bantu buat skrip otomatis atau checklist-nya juga?
