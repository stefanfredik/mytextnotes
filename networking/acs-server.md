# ACS Server

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

```bash
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
      GENIEACS_MONGODB_CONNECTION_URL: mongodb://admin:Mongodb123@mongo/genieacs?authSource=admin
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
      MONGO_INITDB_ROOT_PASSWORD: Mongodb123
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
      ACS_PASS: admin123
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
      ME_CONFIG_MONGODB_ADMINPASSWORD: Mongodb123
      ME_CONFIG_MONGODB_URL: mongodb://admin:Mongodb123@mongo:27017/genieacs?authSource=admin
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
