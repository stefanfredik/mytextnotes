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
