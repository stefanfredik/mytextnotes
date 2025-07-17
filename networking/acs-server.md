# ACS Server

## About

## Pembahasan Lengkap TR-069 dan ACS

TR-069 dan _Auto Configuration Server_ (ACS) adalah komponen kunci dalam pengelolaan jaringan broadband modern. TR-069 menyediakan protokol standar untuk komunikasi antara perangkat pelanggan (_Customer Premises Equipment_ atau CPE) dan server pengelola, sementara ACS adalah server yang menjalankan perintah untuk mengatur, memperbarui, dan mendiagnosis perangkat tersebut. Berikut adalah pembahasan terstruktur dan mendetail tentang keduanya.

### 1. Pengenalan TR-069

TR-069, yang dikenal sebagai _CPE WAN Management Protocol (CWMP)_, adalah spesifikasi teknis yang dikembangkan oleh _Broadband Forum_ untuk memungkinkan pengelolaan jarak jauh perangkat CPE seperti modem, router, gateway, set-top box, dan telepon VoIP. Protokol ini pertama kali diterbitkan pada Mei 2004, dengan versi terbaru adalah _Amendment 6 Corrigendum 1_ (Juni 2020).

#### **Tujuan TR-069**

* **Konfigurasi Otomatis**: Memungkinkan aktivasi layanan tanpa intervensi manual (_zero-touch_ atau _one-touch provisioning_).
* **Pembaruan Firmware/Perangkat Lunak**: Mendukung pembaruan atau penurunan versi firmware serta backup/restore konfigurasi.
* **Diagnostik dan Monitoring**: Memungkinkan pengujian performa jaringan, seperti throughput dan konektivitas, serta pengambilan log atau parameter perangkat.
* **Efisiensi Operasional**: Mengurangi kebutuhan teknisi lapangan, sehingga menurunkan biaya operasional ISP.
* **Standarisasi**: Menyediakan pendekatan yang konsisten untuk pengelolaan perangkat dari berbagai vendor.

#### **Fitur Utama**

| **Fitur**                 | **Deskripsi**                                                              |
| ------------------------- | -------------------------------------------------------------------------- |
| Aktivasi Layanan          | Mendukung konfigurasi awal dan rekonfigurasi layanan secara otomatis.      |
| Pengelolaan Firmware      | Memungkinkan pembaruan, penurunan versi, dan backup/restore konfigurasi.   |
| Diagnostik dan Monitoring | Memungkinkan pengujian konektivitas, throughput, dan pengambilan data log. |
| Komunikasi Aman           | Menggunakan HTTPS untuk komunikasi yang aman antara CPE dan ACS.           |
| Model Data Hierarkis      | Struktur data standar untuk mendefinisikan parameter perangkat.            |

#### **Mekanisme Transport**

* Berbasis teks, menggunakan protokol HTTP/HTTPS.
* CPE bertindak sebagai klien HTTP, sementara ACS berfungsi sebagai server HTTP.
* Sesi komunikasi selalu diinisiasi oleh CPE, memberikan kontrol alur sesi kepada perangkat.

#### **Model Data**

* Struktur hierarkis dengan root seperti _Device_ atau _InternetGatewayDevice_.
* Didefinisikan dalam format XML dan PDF oleh _Broadband Forum_.
* Mendukung objek multi-instance, meskipun sering terjadi masalah seperti parameter yang hilang atau tingkat akses yang salah.
* Standar model data mencakup TR-181 (perangkat rumah), TR-104 (VoIP), TR-135 (set-top box), dan TR-140 (penyimpanan).

#### **Operasi Umum**

TR-069 mendukung beberapa operasi _Remote Procedure Call (RPC)_, termasuk:

* **GetParameterNames**: Mengambil daftar nama parameter yang tersedia.
* **GetParameterValues**: Mengambil nilai parameter tertentu.
* **SetParameterValues**: Mengatur nilai parameter.
* **GetParameterAttributes**: Mengambil atribut parameter (misalnya, notifikasi aktif).
* **SetParameterAttributes**: Mengatur atribut parameter.
* **Download/Upload**: Untuk transfer firmware atau file konfigurasi.
* **AddObject/DeleteObject**: Menambah atau menghapus objek dalam model data.

#### **Keamanan**

* Menggunakan HTTPS untuk enkripsi komunikasi.
* Verifikasi sertifikat ACS oleh CPE untuk memastikan keaslian server.
* Autentikasi CPE menggunakan _shared secret_ (username/password) yang dinegosiasikan per sesi.
* Default password digunakan untuk kontak pertama atau setelah reset pabrik.
* Mendukung traversal NAT melalui Annex G (STUN/UDP) atau Annex K (XMPP, Amendment 5).

#### **Tantangan Keamanan**

Jika tidak diimplementasikan dengan benar, TR-069 dapat menimbulkan risiko keamanan, seperti:

* Penyadapan basis pelanggan.
* Pengalihan DNS atau pemasangan backdoor melalui firmware.
* Eksploitasi oleh botnet seperti Mirai akibat implementasi ACS yang tidak aman.

#### **Adopsi**

Hingga tahun 2020, TR-069 telah digunakan oleh hampir satu miliar perangkat di seluruh dunia, menunjukkan adopsi yang sangat luas.

### 2. Pengenalan ACS (_Auto Configuration Server_)

ACS adalah server perangkat lunak yang menggunakan protokol TR-069 untuk mengelola perangkat CPE secara jarak jauh. ACS bertindak sebagai pusat kendali yang memungkinkan ISP untuk mengatur, memperbarui, dan mendiagnosis perangkat tanpa perlu teknisi di lokasi.

#### **Fungsi Utama ACS**

* **Konfigurasi Perangkat**: Mengatur parameter seperti pengaturan WiFi, IP, atau firewall.
* **Pembaruan Firmware**: Mengelola pembaruan perangkat lunak atau firmware.
* **Diagnostik Jarak Jauh**: Memantau performa jaringan dan mendeteksi masalah.
* **Otomatisasi**: Mengurangi intervensi manual untuk aktivasi layanan dan pemeliharaan.
* **Skalabilitas**: Mampu mengelola jutaan perangkat secara bersamaan.

#### **Cara Kerja ACS**

* **Inisiasi Sesi**: CPE memulai sesi dengan mengirim pesan _Inform_ ke ACS, yang berisi informasi status perangkat.
* **Autentikasi**: ACS memverifikasi identitas CPE menggunakan _shared secret_ atau sertifikat.
* **Eksekusi Tugas**: ACS mengirim perintah seperti _GetParameterValues_, _SetParameterValues_, atau _Download_ untuk melakukan tugas tertentu.
* **Penutupan Sesi**: Sesi berakhir setelah semua tugas selesai, biasanya hanya berlangsung beberapa detik.

#### **Keamanan ACS**

* Menggunakan HTTPS dan sertifikat SSL/TLS untuk komunikasi aman.
* Autentikasi CPE melalui username/password atau sertifikat klien.
* Aturan firewall ketat untuk membatasi akses ke ACS.
* Pembaruan perangkat lunak rutin dan audit keamanan untuk mencegah ancaman siber.

#### **Implementasi ACS**

* Contoh implementasi populer adalah _GenieACS_, yang mendukung pengelolaan skala besar.
* ACS dapat dijalankan di lingkungan _on-premise_, cloud, atau hybrid menggunakan teknologi seperti Docker dan Kubernetes.

### 3. Hubungan Antara TR-069 dan ACS

TR-069 adalah protokol yang mendefinisikan komunikasi antara CPE dan ACS, sementara ACS adalah server yang menjalankan perintah tersebut. Hubungan ini dapat dijelaskan sebagai berikut:

#### **Mekanisme Komunikasi**

* CPE menginisiasi sesi dengan ACS menggunakan protokol TR-069 melalui HTTPS.
* ACS merespons dengan perintah untuk mengambil data, mengatur parameter, atau melakukan pembaruan.
* Sesi bersifat sementara dan hanya berlangsung selama diperlukan untuk menyelesaikan tugas.

#### **Sesi Provisioning**

Sesi provisioning dimulai dengan pesan _Inform_ dari CPE dan dapat dipicu oleh berbagai alasan, seperti:

| **Alasan Sesi**     | **Penjelasan**                                                         |
| ------------------- | ---------------------------------------------------------------------- |
| BOOTSTRAP           | URL ACS disimpan/berubah atau perangkat di-reset ke pengaturan pabrik. |
| PERIODIC            | Berdasarkan interval yang ditentukan (_PeriodicInformInterval_).       |
| CONNECTION REQUEST  | ACS meminta koneksi segera melalui HTTP.                               |
| VALUE CHANGE        | Parameter dengan notifikasi aktif berubah.                             |
| BOOT                | Perangkat di-reset atau terhubung kembali ke daya.                     |
| SCHEDULED           | ACS menjadwalkan sesi dengan perintah _ScheduleInform_.                |
| transfer complete   | Melaporkan selesainya transfer (download/upload).                      |
| DIAGNOSTIC COMPLETE | Mengonfirmasi penyelesaian diagnostik yang dipesan.                    |

#### **Manfaat Kolaborasi**

* **Kontrol Real-Time**: ACS dapat mengubah pengaturan perangkat secara langsung.
* **Efisiensi Instalasi**: Konfigurasi awal dilakukan secara otomatis.
* **Pemeliharaan Jarak Jauh**: Pembaruan firmware dan diagnostik tanpa kunjungan teknisi.
* **Optimasi Jaringan**: ACS dapat mengatur parameter seperti saluran WiFi untuk performa optimal.
* **Monitoring Otomatis**: ACS mengumpulkan data untuk analisis performa jaringan.

### 4. Kelebihan dan Kekurangan TR-069

#### **Kelebihan**

* Standar yang mapan dengan dukungan luas dari berbagai vendor perangkat.
* Mengurangi biaya operasional melalui otomatisasi.
* Mendukung berbagai jenis CPE, dari modem hingga set-top box.
* Memungkinkan pengelolaan skala besar dengan efisiensi tinggi.

#### **Kekurangan**

* Tidak mendukung transaksi atomik, yang dapat menyebabkan masalah konsistensi data.
* Risiko keamanan jika implementasi tidak dilakukan dengan benar.
* Bergantung pada konektivitas internet yang stabil untuk komunikasi.

### 5. TR-369 sebagai Penerus TR-069

TR-369, atau _User Services Platform (USP)_, diperkenalkan oleh _Broadband Forum_ pada tahun 2018 sebagai penerus TR-069. TR-369 menawarkan:

* **Skalabilitas Lebih Baik**: Mendukung jaringan dengan jumlah perangkat yang lebih besar.
* **Keamanan yang Ditingkatkan**: Mekanisme autentikasi dan enkripsi yang lebih kuat.
* **Komunikasi Real-Time**: Mendukung interaksi yang lebih dinamis.
* **Integrasi Fleksibel**: Kompatibel dengan platform modern seperti IoT dan smart home.

Meskipun TR-369 lebih canggih, TR-069 tetap relevan karena adopsi yang luas dan kompatibilitas model data yang sama.

### 6. Kesimpulan

TR-069 dan ACS adalah fondasi penting dalam pengelolaan jaringan broadband. TR-069 menyediakan protokol standar untuk komunikasi yang aman dan efisien antara CPE dan ACS, sementara ACS memungkinkan ISP untuk mengelola perangkat secara otomatis, mengurangi biaya, dan meningkatkan kualitas layanan. Dengan adopsi yang luas dan fitur seperti konfigurasi otomatis, pembaruan firmware, dan diagnostik jarak jauh, keduanya telah menjadi standar industri. Namun, keamanan harus menjadi prioritas untuk mencegah risiko siber. Dengan munculnya TR-369, pengelolaan jaringan di masa depan akan semakin canggih, tetapi TR-069 tetap menjadi solusi yang andal untuk saat ini.

### 7. Sumber Referensi

* [Wikipedia: TR-069](https://en.wikipedia.org/wiki/TR-069)
* [Axiros: TR-069](https://www.axiros.com/knowledge-base/tr-069)
* [Avsystem: TR-069 Crash Course](https://www.avsystem.com/crashcourse/tr069/)
* [Broadband Forum: TR-069 Standard](https://www.broadband-forum.org/pdfs/tr-069-1-6-1.pdf)

## Setup Genie ACS

Dibuat oleh Grox AI

## Panduan Lengkap Instalasi GenieACS Menggunakan Docker dengan Docker Compose

GenieACS adalah Auto Configuration Server (ACS) berkinerja tinggi yang digunakan untuk mengelola perangkat yang mendukung protokol TR-069, seperti router, modem, dan perangkat broadband lainnya. Dengan menggunakan Docker dan Docker Compose, Anda dapat menginstal dan menjalankan GenieACS dengan cepat dan efisien. Panduan ini memberikan langkah-langkah terperinci untuk menginstal GenieACS menggunakan file `docker-compose.yml`, termasuk prasyarat, konfigurasi, dan tips untuk penggunaan produksi.

### Prasyarat

Sebelum memulai, pastikan Anda memiliki:

* **Docker** dan **Docker Compose** terpasang di sistem Anda. Anda dapat mengunduhnya dari [situs resmi Docker](https://www.docker.com/get-started).
* Sistem operasi yang mendukung Docker, seperti Ubuntu 20.04, 22.04, atau distribusi Linux lainnya.
* Koneksi internet untuk mengunduh repository dan image Docker.
* Akses ke firewall untuk membuka port yang diperlukan (3000 untuk UI, 7547 untuk komunikasi perangkat, 7557 untuk API, dan 7567 untuk file server).
* Pengetahuan dasar tentang perintah terminal Linux.

### Langkah-langkah Instalasi

Berikut adalah langkah-langkah untuk menginstal GenieACS menggunakan Docker Compose:

1.  **Instal Docker dan Docker Compose**\
    Jika belum terpasang, instal Docker dan Docker Compose dengan perintah berikut di Ubuntu:

    ```bash
    sudo apt update
    sudo apt install docker.io docker-compose
    sudo systemctl start docker
    sudo systemctl enable docker
    ```

    Verifikasi instalasi:

    ```bash
    docker --version
    docker-compose --version
    ```
2.  **Kloning Repository GenieACS Docker**\
    Unduh repository resmi GenieACS Docker dari GitHub:

    ```bash
    git clone https://github.com/GeiserX/genieacs-docker.git
    ```

    Repository ini berisi file `docker-compose.yml` yang diperlukan untuk menjalankan GenieACS.
3.  **Pindah ke Direktori Repository**\
    Masuk ke direktori yang telah dikloning:

    ```bash
    cd genieacs-docker
    ```
4.  **Jalankan Docker Compose**\
    Jalankan perintah berikut untuk memulai semua layanan GenieACS:

    ```bash
    docker compose up -d
    ```

    Perintah ini akan:

    * Mengunduh image Docker untuk GenieACS dan MongoDB.
    * Menjalankan kontainer untuk layanan GenieACS (CWMP, NBI, FS, dan UI) serta database MongoDB.
    * Menjalankan layanan di latar belakang (`-d` untuk detached mode).
5.  **Verifikasi Kontainer**\
    Pastikan semua kontainer berjalan dengan perintah:

    ```bash
    docker ps
    ```

    Anda akan melihat beberapa kontainer, termasuk untuk GenieACS dan MongoDB.
6.  **Akses Antarmuka Web**\
    Buka browser dan akses antarmuka web GenieACS di:

    ```
    http://<alamat-ip-server-anda>:3000
    ```

    Ganti `<alamat-ip-server-anda>` dengan alamat IP atau hostname mesin Anda. Port 3000 adalah port default untuk antarmuka pengguna (UI).
7. **Konfigurasi Awal melalui Wizard**\
   Saat pertama kali mengakses UI, Anda akan diarahkan ke wizard inisialisasi database. Wizard ini akan membantu Anda:
   * Mengatur konfigurasi awal database MongoDB.
   * Membuat pengguna admin untuk login ke UI.
   * Mengatur parameter dasar untuk pengelolaan perangkat.
8.  **Konfigurasi Perangkat untuk Terhubung ke GenieACS**\
    Untuk perangkat yang mendukung TR-069, atur URL ACS di perangkat tersebut ke:

    ```
    http://<alamat-ip-server-anda>:7547
    ```

    Port 7547 adalah port default untuk layanan CWMP (komunikasi dengan perangkat).

### Port yang Digunakan

GenieACS menggunakan beberapa port untuk layanan yang berbeda. Pastikan port-port ini terbuka di firewall Anda:

| Layanan          | Port Default | Deskripsi                                      |
| ---------------- | ------------ | ---------------------------------------------- |
| CWMP             | 7547         | Komunikasi dengan perangkat TR-069             |
| NBI (API)        | 7557         | Antarmuka API untuk integrasi eksternal        |
| File Server (FS) | 7567         | Server untuk unduhan firmware dan file lainnya |
| UI               | 3000         | Antarmuka web pengguna                         |

Untuk membuka port di Ubuntu menggunakan `ufw`, contohnya:

```bash
sudo ufw allow 3000
sudo ufw allow 7547
sudo ufw allow 7557
sudo ufw allow 7567
```

### Konfigurasi Tambahan untuk Produksi

Untuk penggunaan produksi, pertimbangkan langkah-langkah berikut:

* **Konfigurasi HTTPS**: Untuk keamanan, aktifkan HTTPS dengan menambahkan sertifikat SSL. Anda dapat mengatur variabel lingkungan seperti `GENIEACS_UI_SSL_CERT` dan `GENIEACS_UI_SSL_KEY` di file `docker-compose.yml` untuk menentukan lokasi sertifikat dan kunci.
*   **Cadangan Database**: Sebelum melakukan pembaruan atau perubahan, cadangkan database MongoDB Anda untuk mencegah kehilangan data. Gunakan perintah seperti:

    ```bash
    docker exec <nama-kontainer-mongodb> mongodump --db genieacs --out /backup
    ```
* **Variabel Lingkungan**: File `docker-compose.yml` mungkin memerlukan penyesuaian variabel lingkungan, seperti:
  * `GENIEACS_MONGODB_CONNECTION_URL`: URL koneksi ke MongoDB (default: `mongodb://mongo/genieacs`).
  * `GENIEACS_EXT_DIR`: Direktori untuk skrip ekstensi (default: `/opt/genieacs/ext`).
  * `GENIEACS_UI_JWT_SECRET`: Kunci rahasia untuk autentikasi UI.

### Pemecahan Masalah

* **Kontainer MongoDB Crash**: Jika kontainer MongoDB (misalnya, versi 7.0) sering crash, periksa kompatibilitas versi MongoDB dengan GenieACS. Anda mungkin perlu menurunkan versi MongoDB di file `docker-compose.yml` ke versi yang lebih stabil, seperti 6.x.
*   **Port Tidak Dapat Diakses**: Pastikan port tidak diblokir oleh firewall atau digunakan oleh aplikasi lain. Gunakan `netstat` atau `ss` untuk memeriksa:

    ```bash
    ss -tuln
    ```
* **Dokumentasi Tambahan**: Jika Anda mengalami masalah, kunjungi [forum komunitas GenieACS](https://forum.genieacs.com/) atau [wiki GenieACS](https://github.com/genieacs/genieacs/wiki) untuk bantuan lebih lanjut.



## Menggunakan Docker Compose

```yaml
services:
  ### GenieACS Service ###
  genieacs:
    depends_on:
      - mongo
    image: drumsergio/genieacs:latest
    restart: always
    container_name: genieacs
    environment:
      GENIEACS_UI_JWT_SECRET: changeme
      GENIEACS_CWMP_ACCESS_LOG_FILE: /var/log/genieacs/genieacs-cwmp-access.log
      GENIEACS_NBI_ACCESS_LOG_FILE: /var/log/genieacs/genieacs-nbi-access.log
      GENIEACS_FS_ACCESS_LOG_FILE: /var/log/genieacs/genieacs-fs-access.log
      GENIEACS_UI_ACCESS_LOG_FILE: /var/log/genieacs/genieacs-ui-access.log
      GENIEACS_DEBUG_FILE: /var/log/genieacs/genieacs-debug.yaml
      GENIEACS_EXT_DIR: /opt/genieacs/ext
      GENIEACS_MONGODB_CONNECTION_URL: mongodb://admin:Homenetdb123@mongo/genieacs?authSource=admin
    ports:
      - "7547:7547" # CWMP
      - "7557:7557" # NBI
      - "7567:7567" # FS
      - "3000:3000" # UI
    volumes:
      - opt_volume:/opt
    networks:
      - genieacs_network

  ### MongoDB Service ###
  mongo:
    image: mongo:8.0
    restart: always
    container_name: mongo-genieacs
    environment:
      MONGO_INITDB_ROOT_USERNAME: admin
      MONGO_INITDB_ROOT_PASSWORD: Homenetdb123
      MONGO_INITDB_DATABASE: genieacs
      MONGO_DATA_DIR: /data/db
      MONGO_LOG_DIR: /var/log/mongodb
    volumes:
      - data_db:/data/db
      - data_configdb:/data/configdb
    expose:
      - 27017
    networks:
      - genieacs_network

  ### GenieACS Simulator (Optional for Testing) ###
  genieacs-sim:
    depends_on:
      - genieacs
    image: drumsergio/genieacs-sim:latest
    container_name: genieacs-sim
    networks:
      - genieacs_network

  ### GenieACS MCP Server (AI, Optional) ###
  genieacs-mcp:
    depends_on:
      - genieacs
    image: drumsergio/genieacs-mcp:latest
    container_name: genieacs-mcp
    environment:
      ACS_URL: http://genieacs:7557
      ACS_USER: admin
      ACS_PASS: Homenet@123
    ports:
      - "8080:8080"
    networks:
      - genieacs_network

  ### Mongo Express Service ###
  mongo-express:
    depends_on:
      - mongo
    image: mongo-express:latest
    restart: always
    container_name: mongo-express
    environment:
      ME_CONFIG_MONGODB_ADMINUSERNAME: admin
      ME_CONFIG_MONGODB_ADMINPASSWORD: Homenetdb123
      ME_CONFIG_BASICAUTH: true
      ME_CONFIG_BASICAUTH_USERNAME: admin
      ME_CONFIG_BASICAUTH_PASSWORD: Homenet@123
      ME_CONFIG_MONGODB_URL: mongodb://admin:Homenetdb123@mongo:27017/genieacs?authSource=admin
    ports:
      - "8081:8081"
    networks:
      - genieacs_network

volumes:
  data_db:
  data_configdb:
  opt_volume:

networks:
  genieacs_network:
    driver: bridge
```

### Sumber Daya Tambahan

* **Dokumentasi Resmi**: [docs.genieacs.com](http://docs.genieacs.com/) (catatan: situs ini mungkin tidak selalu dapat diakses; periksa wiki sebagai alternatif).
* **Wiki GenieACS**: [github.com/genieacs/genieacs/wiki](https://github.com/genieacs/genieacs/wiki).
* **Repository Docker**: [github.com/GeiserX/genieacs-docker](https://github.com/GeiserX/genieacs-docker).
* **Docker Hub**: [hub.docker.com/r/drumsergio/genieacs](https://hub.docker.com/r/drumsergio/genieacs).

### Catatan Penutup

Panduan ini memberikan langkah-langkah dasar untuk menginstal GenieACS menggunakan Docker Compose. Untuk skenario produksi atau konfigurasi lanjutan, seperti integrasi dengan sistem eksternal atau pengelolaan perangkat skala besar, Anda mungkin perlu menyesuaikan file `docker-compose.yml` atau menambahkan konfigurasi khusus. Komunitas GenieACS sangat aktif, dan Anda dapat menemukan dukungan tambahan melalui forum atau kontribusi di repository GitHub.
