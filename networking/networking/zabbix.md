# Zabbix

## Upgrade Zabbix from 7.2 - 7.4

## Panduan Lengkap Upgrade Zabbix 7.2 ke 7.4

Panduan ini memberikan langkah-langkah terperinci untuk mengupgrade Zabbix dari versi 7.2 ke 7.4, berdasarkan dokumentasi resmi Zabbix dan sumber terpercaya lainnya. Proses ini mencakup persiapan, backup, upgrade, dan verifikasi, dengan perhatian khusus pada pengguna Proxmox yang berbasis Debian.

### Langkah 1: Persiapan

Sebelum memulai, lakukan persiapan berikut untuk memastikan proses upgrade berjalan lancar dan aman:

1. **Baca Catatan Rilis dan Catatan Upgrade**:
   * Tinjau [Catatan Rilis Zabbix 7.4](https://www.zabbix.com/rn/rn7.4.0) untuk memahami fitur baru dan perubahan.
   * Baca [Catatan Upgrade Zabbix 7.4](https://www.zabbix.com/documentation/current/en/manual/installation/upgrade_notes) untuk mengetahui perubahan penting, seperti penggantian PCRE dengan PCRE2 dan perubahan manajemen media pengguna.
2. **Optimalkan Kinerja Database**:
   * Pastikan database dalam kondisi baik. Jika database besar, proses upgrade mungkin memakan waktu lama, jadi optimalkan terlebih dahulu (misalnya, indeks atau pembersihan data lama).
3. **Verifikasi Kompatibilitas Komponen**:
   * Pastikan versi komponen seperti PHP, database (MySQL/MariaDB, PostgreSQL, dll.), dan modul lain sesuai dengan [matriks kompatibilitas Zabbix 7.4](https://www.zabbix.com/documentation/current/en/manual/installation/requirements).
   * Periksa versi dengan perintah:
     * PHP: `php-fpm -v`
     * PostgreSQL: `psql -V`
4. **Backup Konfigurasi dan Database**:
   *   Backup file konfigurasi Zabbix dengan perintah:

       ```bash
       cp -R /etc/zabbix/ /<backup directory>/
       cp -R /usr/lib/zabbix/alertscripts/ /<backup directory>/
       cp -R /usr/lib/zabbix/externalscripts/ /<backup directory>/
       cp -R /usr/share/zabbix/ /<backup directory>/
       cp /etc/httpd/conf/httpd.conf /<backup directory>/
       cp /etc/httpd/conf.d/zabbix.conf /<backup directory>/
       ```
   *   Backup database (contoh untuk MySQL/MariaDB):

       ```bash
       mysqldump -u <username> -p zabbix > zabbix_backup.sql
       ```

       Untuk PostgreSQL:

       ```bash
       pg_dump -U <username> zabbix > zabbix_backup.sql
       ```
   * Simpan backup di lokasi aman di luar server.
5. **Nonaktifkan High Availability (HA)**:
   * Jika menggunakan HA pada Zabbix server, nonaktifkan untuk mencegah konflik selama upgrade.

### Langkah 2: Matikan Layanan

Hentikan layanan Zabbix server dan web server untuk mencegah penulisan data selama proses upgrade:

```bash
systemctl stop zabbix-server
systemctl stop httpd  # Untuk RHEL/CentOS
systemctl stop apache2  # Untuk Debian/Ubuntu
```

### Langkah 3: Update Database (Opsional)

Jika menggunakan PostgreSQL dengan TimescaleDB, perhatikan potensi masalah dengan versi tertentu:

* **PostgreSQL 17 dan TimescaleDB 2.17**:
  * Ada masalah dengan data lama. Pilih salah satu opsi berikut:
    * Turunkan ke TimescaleDB 2.16.2.
    * Naikkan ke TimescaleDB 2.18.
    *   Tambahkan parameter `timescaledb.enable_vectorized_aggregation = off` ke file `postgresql.conf` dan restart database:

        ```bash
        systemctl restart postgresql
        ```
  * Untuk detail, lihat [Panduan Update PostgreSQL](https://www.initmax.com/wiki/how-to-update-postgresql-to-the-latest-version/).
* **MySQL/MariaDB**:
  *   Jika binary logging diaktifkan tanpa hak superuser, atur parameter sementara:

      ```sql
      SET GLOBAL log_bin_trust_function_creators = 1;
      ```

      Setelah upgrade, nonaktifkan kembali:

      ```sql
      SET GLOBAL log_bin_trust_function_creators = 0;
      ```

### Langkah 4: Upgrade Zabbix

Proses upgrade berbeda tergantung pada sistem operasi. Karena Anda menggunakan Proxmox (berbasis Debian), langkah untuk Debian/Ubuntu lebih relevan, tetapi langkah untuk RHEL/CentOS juga disertakan untuk kelengkapan.

#### Untuk Debian 12/Ubuntu 24.04 (Proxmox)

1.  **Unduh Repositori Zabbix 7.4**:

    ```bash
    wget https://repo.zabbix.com/zabbix/7.4/release/debian/pool/main/z/zabbix-release/zabbix-release_latest_7.4+debian12_all.deb
    ```
2.  **Install Repositori**:

    ```bash
    dpkg -i zabbix-release_latest_7.4+debian12_all.deb
    ```
3.  **Update Daftar Paket**:

    ```bash
    apt update
    ```
4.  **Upgrade Paket Zabbix**:

    ```bash
    apt install --only-upgrade $(dpkg -l | grep zabbix | awk '{print $2}')
    ```
5.  **Start Zabbix Server**:

    ```bash
    systemctl start zabbix-server.service
    ```
6.  **Monitor Log**:

    ```bash
    tail -f /var/log/zabbix/zabbix_server.log
    ```
7.  **Start Web Server**:

    ```bash
    systemctl start apache2
    ```

#### Untuk RHEL 9/CentOS Stream 9

1.  **Unduh Repositori Zabbix 7.4**:

    ```bash
    rpm -Uvh https://repo.zabbix.com/zabbix/7.4/release/rocky/9/noarch/zabbix-release-latest.el9.noarch.rpm
    ```
2.  **Bersihkan Cache**:

    ```bash
    dnf clean all
    ```
3.  **Update Paket Zabbix**:

    ```bash
    dnf update zabbix-* -y
    ```
4.  **Start Zabbix Server**:

    ```bash
    systemctl start zabbix-server.service
    ```
5.  **Monitor Log**:

    ```bash
    tail -f /var/log/zabbix/zabbix_server.log
    ```
6.  **Start Web Server**:

    ```bash
    systemctl start httpd
    ```

### Langkah 5: Verifikasi dan Finalisasi

1. **Verifikasi Versi**:
   *   Periksa versi Zabbix server:

       ```bash
       zabbix_server --version
       ```
   * Pastikan versi menunjukkan 7.4.x.
2. **Periksa Log untuk Error**:
   * Perhatikan error seperti "user limit of 1024 file descriptors is insufficient...". Jika muncul, ikuti resolusi di [dokumentasi resmi](https://www.zabbix.com/documentation/current/en/manual/installation/upgrade_notes#p_user_limit_of_1024_file_descriptors_is_insufficient_for_running_zabbix_server).
3. **Uji Fungsionalitas**:
   * Login ke antarmuka web Zabbix dan pastikan semua fungsi monitoring, seperti pengumpulan data dan notifikasi, berjalan normal.
   * Periksa integrasi Proxmox untuk memastikan tidak ada masalah setelah upgrade.

### Catatan Penting

#### Perubahan Utama di Zabbix 7.4

Berikut adalah beberapa perubahan penting yang perlu diperhatikan:

| **Perubahan**                | **Detail**                                                                                                                                                                                                                                                                                                                                              |
| ---------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **PCRE ke PCRE2**            | Dukungan PCRE dihentikan, digantikan oleh PCRE2. Pastikan sistem Anda mendukung PCRE2.                                                                                                                                                                                                                                                                  |
| **Manajemen Media Pengguna** | Pengguna kini dapat mengelola media mereka sendiri secara default, dengan izin diatur melalui peran pengguna ("Create and edit own media", "Create and edit user media").                                                                                                                                                                               |
| **Backslash Escaping**       | Parameter fungsi history dengan panjang >255 karakter mungkin menyebabkan masalah trigger. Pindahkan ke user macros sebelum upgrade. Lihat [Known Issues](https://www.zabbix.com/documentation/current/en/maintenance/known_issues/db_upgrade_escaping).                                                                                                |
| **MSSQL Plugin**             | Plugin MSSQL Zabbix agent 2 harus diupdate ke versi ≥7.4.0 untuk perubahan template.                                                                                                                                                                                                                                                                    |
| **libssh2**                  | Versi minimum libssh2 kini 1.8.0.                                                                                                                                                                                                                                                                                                                       |
| **Host Prototypes**          | Upgrade mungkin menambahkan prototype dari template pada host yang ditemukan. Periksa dengan query SQL: `SELECT h.hostid,ht.templateid FROM hosts_templates ht JOIN hosts h ON ht.hostid=h.hostid WHERE h.flags=4 AND EXISTS (SELECT NULL FROM items i,host_discovery hd WHERE i.hostid=ht.templateid AND hd.parent_itemid=i.itemid) ORDER BY hostid;`. |
| **DBPort dan DBSocket**      | Parameter koneksi database ini saling eksklusif.                                                                                                                                                                                                                                                                                                        |
| **History Cache**            | Perintah baru `history_cache_clear=target` ditambahkan, dengan pembersihan otomatis saat item/host dinonaktifkan. Pantau dengan `zabbix[wcache]`.                                                                                                                                                                                                       |
| **Host Wizard**              | Memerlukan upgrade template setelah upgrade Zabbix. Lihat [Template Upgrade](https://www.zabbix.com/documentation/current/en/manual/config/templates).                                                                                                                                                                                                  |

#### Catatan untuk Proxmox

* Karena Proxmox berbasis Debian, ikuti langkah untuk Debian 12/Ubuntu 24.04.
* Pastikan integrasi Proxmox di Zabbix (seperti item "API service status") tetap berfungsi setelah upgrade. Jika ada masalah, periksa konfigurasi API Proxmox dan autentikasi di Zabbix.

#### Peringatan

* **Backup Wajib**: Selalu backup sebelum upgrade untuk mencegah kehilangan data.
* **Waktu Upgrade Database**: Database besar mungkin memerlukan waktu lama untuk upgrade.
* **Zabbix Proxy dengan SQLite**: Jika proxy menggunakan SQLite3, database lama mungkin dihapus otomatis saat startup, menyebabkan kehilangan data history. Pastikan versi proxy sesuai dengan database.

### Sumber

* [Dokumentasi Resmi Zabbix - Prosedur Upgrade](https://www.zabbix.com/documentation/current/en/manual/installation/upgrade)
* [Dokumentasi Resmi Zabbix - Catatan Upgrade untuk 7.4.0](https://www.zabbix.com/documentation/current/en/manual/installation/upgrade_notes)
* [Panduan Initmax - Upgrade ke Zabbix 7.4](https://www.initmax.com/wiki/zabbix-upgrade-to-the-latest-version-7-4/)



## Integrasi Proxmox ke Zabbix

## Panduan Lengkap Integrasi Proxmox dengan Zabbix Server untuk Monitoring

Panduan ini memberikan langkah-langkah terperinci untuk mengintegrasikan Proxmox Virtual Environment (VE) dengan Zabbix Server agar Anda dapat memantau node Proxmox, mesin virtual (VM), container (LXC), dan sumber daya seperti CPU, memori, dan penyimpanan melalui Zabbix. Integrasi ini menggunakan API Proxmox berbasis HTTP, sehingga tidak memerlukan instalasi agen Zabbix di node Proxmox. Panduan ini dirancang untuk Zabbix versi 7.4 atau lebih tinggi dan mencakup troubleshooting berdasarkan masalah umum seperti yang terlihat pada lampiran Anda.

### Prasyarat

Sebelum memulai, pastikan Anda memiliki:

| **Komponen**          | **Persyaratan**                                                                                                                   |
| --------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| **Zabbix Server**     | Versi 7.4 atau lebih tinggi, terinstal dan berjalan dengan akses ke antarmuka web.                                                |
| **Proxmox VE**        | Versi apa saja dengan API aktif (port default 8006). Akses ke antarmuka web Proxmox diperlukan.                                   |
| **Jaringan**          | Port 8006 terbuka untuk komunikasi antara Zabbix dan Proxmox.                                                                     |
| **Backup**            | Backup konfigurasi Zabbix (`/etc/zabbix/`) dan database, serta konfigurasi Proxmox.                                               |
| **Pengetahuan Dasar** | Pemahaman tentang API Proxmox ([Dokumentasi API](https://pve.proxmox.com/pve-docs/api-viewer/index.html)) dan konfigurasi Zabbix. |

**Catatan**: Selalu lakukan backup sebelum memulai untuk mencegah kehilangan data.

### Langkah 1: Konfigurasi Proxmox untuk Monitoring

Untuk memungkinkan Zabbix mengakses data Proxmox melalui API, Anda perlu membuat pengguna khusus dan token API dengan izin yang sesuai.

1. **Buat Pengguna Khusus untuk Monitoring**:
   * Login ke antarmuka web Proxmox.
   * Buka **Datacenter** > **Permissions** > **Users** > **Add**.
   * Buat pengguna baru, misalnya, `zabbix_monitor@pve`.
   * Berikan izin minimal berikut melalui **Permissions** > **Add** > **User Permission**:
     * **Path**: `/`
       * **Role**: `Sys.Audit` (untuk memantau status sistem).
     * **Path**: `/storage`
       * **Role**: `Datastore.Audit` (untuk memantau penyimpanan).
     * **Path**: `/vms`
       * **Role**: `VM.Audit` (untuk memantau VM dan container).
   *   Contoh perintah CLI (opsional):

       ```bash
       pveum user add zabbix_monitor@pve
       pveum acl modify / -user zabbix_monitor@pve -role Sys.Audit
       pveum acl modify /storage -user zabbix_monitor@pve -role Datastore.Audit
       pveum acl modify /vms -user zabbix_monitor@pve -role VM.Audit
       ```
2. **Buat Token API**:
   * Buka **Datacenter** > **Permissions** > **API Tokens** > **Add**.
   * Pilih pengguna `zabbix_monitor@pve` dan beri nama token (misalnya, `ZabbixMonitoring01`).
   * Pastikan opsi **Privilege Separation** tidak dicentang untuk kemudahan.
   * Catat **Token ID** (misalnya, `zabbix_monitor@pve!ZabbixMonitoring01`) dan **Token Secret** yang muncul sekali saja.
   * Berikan izin yang sama seperti pengguna:
     * **Path**: `/`, **Role**: `Sys.Audit`
     * **Path**: `/storage`, **Role**: `Datastore.Audit`
     * **Path**: `/vms`, **Role**: `VM.Audit`
3. **Uji Akses API**:
   *   Dari server Zabbix, uji koneksi API menggunakan `curl`:

       ```bash
       curl -k -H "Authorization: PVEAPIToken=zabbix_monitor@pve!ZabbixMonitoring01=<Token Secret>" https://<Proxmox_IP>:8006/api2/json/nodes
       ```
   *   Respons harus berupa JSON yang valid, seperti:

       ```json
       {
         "data": [
           {"node": "node8", "status": "online"},
           {"node": "node9", "status": "online"}
         ]
       }
       ```
   * Jika respons menunjukkan error (misalnya, 403 Forbidden), periksa izin pengguna atau token.

### Langkah 2: Konfigurasi Zabbix

Setelah Proxmox dikonfigurasi, langkah selanjutnya adalah mengatur Zabbix untuk memantau Proxmox menggunakan template resmi.

1. **Unduh dan Impor Template Proxmox**:
   * Unduh template "Proxmox VE by HTTP" dari [Zabbix Share](https://git.zabbix.com/projects/ZBX/repos/zabbix/browse/templates/app/proxmox?at=release/7.4) (file: `template_app_proxmox.xml`).
   * Di Zabbix, buka **Configuration** > **Templates** > **Import**.
   * Unggah file template dan pastikan impor berhasil.
2. **Buat Host Baru untuk Proxmox**:
   * Buka **Configuration** > **Hosts** > **Create host**.
   * Isi detail host:
     * **Host name**: Misalnya, "HomeNet Node 8".
     * **Visible name**: Opsional, bisa sama dengan host name.
     * **Groups**: Pilih grup seperti "Linux servers" atau buat grup baru "Proxmox Nodes".
     * **Interfaces**:
       * Tambahkan interface **HTTP Agent**:
         * **Connect to**: IP atau DNS Proxmox (misalnya, `192.168.1.100`).
         * **Port**: `8006`.
       * Interface Agent (port 10050) tidak diperlukan untuk metode HTTP ini.
3. **Setel Macro untuk Host**:
   *   Di tab **Macros** host, tambahkan:

       | **Macro**             | **Nilai**        | **Deskripsi**                                                                  |
       | --------------------- | ---------------- | ------------------------------------------------------------------------------ |
       | `{$PVE.URL.HOST}`     | `<Proxmox_IP>`   | IP atau hostname Proxmox (misalnya, `192.168.1.100`).                          |
       | `{$PVE.URL.PORT}`     | `8006`           | Port API Proxmox (default).                                                    |
       | `{$PVE.TOKEN.ID}`     | `<Token ID>`     | Token ID dari langkah 1.2 (misalnya, `zabbix_monitor@pve!ZabbixMonitoring01`). |
       | `{$PVE.TOKEN.SECRET}` | `<Token Secret>` | Token Secret dari langkah 1.2.                                                 |
4. **Tautkan Template ke Host**:
   * Di tab **Templates**, klik **Link new templates** dan pilih "Proxmox VE by HTTP".
   * Simpan konfigurasi host.
5. **Aktifkan Monitoring**:
   * Pastikan host diaktifkan (status "Enabled").
   * Tunggu beberapa menit agar Zabbix mulai mengumpulkan data.

### Langkah 3: Verifikasi Monitoring

Setelah konfigurasi selesai, verifikasi apakah Zabbix berhasil memantau Proxmox:

1. **Periksa Data Terbaru**:
   * Buka **Monitoring** > **Latest data**.
   * Cari host "HomeNet Node 8" dan periksa item seperti:
     * **API service status**: Harus menunjukkan "OK (200)".
     * **Node \[nodeX]: CPU, loadavg**: Menunjukkan beban CPU.
     * **Node \[nodeX]: Memory, usage**: Menunjukkan penggunaan memori.
   * Jika item menunjukkan "Not supported" atau error, lanjutkan ke troubleshooting.
2. **Lihat Grafik**:
   * Klik link "Graph" pada item untuk melihat tren data (misalnya, penggunaan CPU atau memori).
3. **Periksa Log Zabbix**:
   *   Buka log Zabbix untuk memeriksa error:

       ```bash
       tail -f /var/log/zabbix/zabbix_server.log
       ```

### Langkah 4: Troubleshooting Masalah Umum

Berdasarkan lampiran dan log error yang Anda berikan, berikut adalah solusi untuk masalah umum:

| **Masalah**                                                                                                                                     | **Penyebab**                                  | **Solusi**                                                                                                                                                                                                                                              |
| ----------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Error "Not supported"** (contoh: `cannot extract value from json by path "$.data.[?(@.name == 'node8' && @.type == 'node')].online.first()"`) | Respons API tidak valid atau path JSON salah. | <p>- Uji API dengan <code>curl</code> untuk memastikan respons JSON valid.<br>- Periksa preprocessing item di Zabbix dan sesuaikan path JSON dengan struktur respons API.<br>- Pastikan node (misalnya, <code>node8</code>) ada di cluster Proxmox.</p> |
| **Error "Response code 403"** (contoh: `Response code "403" did not match any of the required status codes "200"`)                              | Izin API tidak mencukupi.                     | <p>- Periksa izin pengguna dan token di Proxmox (lihat langkah 1.1 dan 1.2).<br>- Tambahkan izin seperti <code>Sys.Audit</code> untuk path <code>/</code>.<br>- Uji API dengan <code>curl</code> untuk memverifikasi akses.</p>                         |
| **Perubahan Nilai Negatif** (contoh: `-320` pada "API service status")                                                                          | Masalah preprocessing atau data tidak sesuai. | <p>- Periksa log Zabbix untuk detail error.<br>- Pastikan item menggunakan unit yang benar (misalnya, status code vs nilai numerik).</p>                                                                                                                |

**Langkah Troubleshooting Tambahan**:

*   **Periksa Log Proxmox**:

    ```bash
    tail -f /var/log/pve/proxy.log
    ```

    Cari error terkait permintaan API.
* **Uji Item Secara Manual**:
  * Di Zabbix, buka konfigurasi item, klik "Test", dan masukkan data JSON uji untuk memverifikasi preprocessing.
* **Perbarui Template**:
  * Jika template lama, impor ulang versi terbaru dari [Zabbix Share](https://git.zabbix.com/projects/ZBX/repos/zabbix/browse/templates/app/proxmox?at=release/7.4).
* **Periksa Firewall**:
  *   Pastikan port 8006 terbuka:

      ```bash
      nc -zv <Proxmox_IP> 8006
      ```

### Langkah 5: Best Practices

* **Gunakan Pengguna Khusus**: Selalu gunakan pengguna dan token API khusus untuk keamanan.
* **Pantau Log Secara Berkala**: Periksa log Zabbix dan Proxmox untuk mendeteksi masalah dini.
* **Gunakan Tag**: Manfaatkan tag seperti `component: system`, `node: node8` untuk mengelompokkan data.
* **Perbarui Secara Rutin**: Pastikan Zabbix dan template selalu diperbarui ke versi terbaru.

### Contoh Kasus dari Lampiran

Berdasarkan lampiran yang Anda unggah:

* **Lampiran 0**: Item "API service status" menunjukkan "OK (200)" tetapi dengan perubahan nilai "-320". Ini mungkin menunjukkan masalah preprocessing. Periksa log Zabbix untuk detail dan pastikan unit data sesuai.
* **Lampiran 1**: Status "Not supported" pada beberapa item menunjukkan respons API tidak valid atau path JSON salah. Uji API dan sesuaikan preprocessing.
* **Lampiran 2**: Error "Response code 403" pada item "Node \[node8]: CPU, loadavg" menunjukkan masalah izin. Perbarui izin pengguna di Proxmox.

### Sumber dan Referensi

* [Dokumentasi Resmi Zabbix untuk Integrasi Proxmox](https://www.zabbix.com/integrations/proxmox)
* [Template Resmi Proxmox di Zabbix Share](https://git.zabbix.com/projects/ZBX/repos/zabbix/browse/templates/app/proxmox?at=release/7.4)
* [Dokumentasi API Proxmox](https://pve.proxmox.com/pve-docs/api-viewer/index.html)
* [Panduan Geek is the Way untuk Zabbix Agent](https://geekistheway.com/2022/12/31/monitoring-proxmox-ve-using-zabbix-agent/)
* [Tutorial RDR-IT untuk Proxmox dan Zabbix](https://rdr-it.com/en/proxmox-supervision-with-zabbix/)
