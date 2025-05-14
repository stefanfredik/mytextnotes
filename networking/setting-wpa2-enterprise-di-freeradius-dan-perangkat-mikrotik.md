---
description: Dibuat oleh claude.ai
---

# Setting WPA2 Enterprise di Freeradius dan Perangkat Mikrotik

## Panduan Setup FreeRADIUS di Ubuntu dan Konfigurasi Mikrotik HAP Lite dengan WPA2-Enterprise

Saya akan membantu Anda menyiapkan FreeRADIUS di Ubuntu yang akan terintegrasi dengan Mikrotik HAP Lite untuk autentikasi WPA2-Enterprise. Berikut panduan lengkapnya:

### 1. Konfigurasi FreeRADIUS di Ubuntu

#### Persiapan Server

Pastikan FreeRADIUS sudah terpasang di sistem Ubuntu Anda. Jika belum, Anda bisa memasangnya dengan:

```bash
sudo apt update
sudo apt install freeradius freeradius-mysql freeradius-utils
```

#### Konfigurasi Dasar FreeRADIUS

1. Buka file konfigurasi klien:

```bash
sudo nano /etc/freeradius/3.0/clients.conf
```

2. Tambahkan konfigurasi Mikrotik Anda di akhir file:

```
client mikrotik_hap_lite {
        ipaddr = 192.168.88.1  # Sesuaikan dengan IP Mikrotik Anda
        secret = rahasia123     # Password rahasia antara RADIUS dan Mikrotik
        shortname = mikrotik
        nastype = other
}
```

3. Buat pengguna untuk autentikasi:

```bash
sudo nano /etc/freeradius/3.0/users
```

4. Tambahkan pengguna (contoh):

```
testuser Cleartext-Password := "password123"
        Reply-Message = "Hello, %{User-Name}"
```

#### Konfigurasi EAP untuk WPA2-Enterprise

1. Edit file konfigurasi EAP:

```bash
sudo nano /etc/freeradius/3.0/mods-enabled/eap
```

2. Pastikan konfigurasi TLS untuk EAP terlihat seperti ini:

```
eap {
        default_eap_type = peap
        
        tls-config tls-common {
                private_key_password = whatever
                private_key_file = ${certdir}/server.key
                certificate_file = ${certdir}/server.crt
                ca_file = ${cadir}/ca.pem
                dh_file = ${certdir}/dh
                ca_path = ${cadir}
                cipher_list = "DEFAULT"
                cipher_server_preference = yes
        }
        
        peap {
                default_eap_type = mschapv2
        }
        
        mschapv2 {
        }
}
```

#### Membuat Sertifikat SSL

1. Buat sertifikat untuk server RADIUS:

```bash
cd /etc/freeradius/3.0/certs/
sudo make
```

#### Memulai Ulang Layanan

```bash
sudo systemctl restart freeradius
```

#### Uji Konfigurasi RADIUS

```bash
radtest testuser password123 localhost 0 rahasia123
```

Jika berhasil, Anda akan melihat "Access-Accept" dalam respons.

### 2. Konfigurasi Mikrotik HAP Lite

#### Konfigurasi RADIUS Client di Mikrotik

1. Masuk ke Mikrotik melalui WinBox atau WebFig
2. Buka menu RADIUS
3. Tambahkan server RADIUS baru:
   * Buka menu **RADIUS**
   * Klik tombol **+** untuk menambahkan server baru
   * Isi detail berikut:
     * **Address**: \[IP Ubuntu Server Anda]
     * **Secret**: rahasia123 (sama dengan yang dikonfigurasi di clients.conf)
     * **Service**: wireless
     * **Authentication Port**: 1812
     * **Accounting Port**: 1813
   * Klik **OK**

#### Konfigurasi Wireless untuk WPA2-Enterprise

1. Buka menu **Wireless**
2. Pilih interface wireless Anda (biasanya wlan1)
3. Buka tab **Security Profiles**
4. Buat profil keamanan baru atau edit yang sudah ada:
   * **Mode**: dynamic keys
   * **Authentication Types**: WPA2 PSK (hapus tanda centang opsi lain)
   * **Unicast Ciphers**: aes ccm (hapus tanda centang opsi lain)
   * **Group Ciphers**: aes ccm (hapus tanda centang opsi lain)
   * **WPA2 Pre-Shared Key**: (kosongkan)
   * **Supplicant Identity**: (kosongkan)
   * Centang **Management Protection**
   * Centang **WPA2-EAP**
   * Klik **OK**
5. Kembali ke tab **Wireless**
6. Pilih interface wireless Anda
7. Pilih **Security Profile** yang baru Anda buat
8. Di tab **RADIUS**, atur:
   * Centang **Use RADIUS**
   * Pilih server RADIUS yang telah Anda tambahkan sebelumnya
   * Klik **OK**

### 3. Pengujian Koneksi

1. Pada perangkat klien (laptop, smartphone), coba sambungkan ke jaringan WiFi:
   * SSID: \[Nama jaringan WiFi Anda]
   * Security: WPA2-Enterprise
   * EAP Method: PEAP
   * Phase 2 Authentication: MSCHAPv2
   * Identity: testuser (atau username yang Anda buat)
   * Password: password123 (atau password yang Anda tentukan)
2. Perangkat seharusnya dapat terhubung jika konfigurasi sudah benar.

### 4. Pemecahan Masalah

Jika koneksi gagal, cek log FreeRADIUS untuk diagnosis:

```bash
sudo systemctl stop freeradius
sudo freeradius -X
```

Kemudian coba sambungkan lagi dan perhatikan output debug.

### 5. Tambahan Keamanan

1. Sebaiknya buat pengguna di database MySQL untuk skala besar:

```bash
sudo apt install mariadb-server
sudo mysql_secure_installation
```

2. Konfigurasikan FreeRADIUS untuk menggunakan database sebagai backend pengguna.

Dengan panduan ini, Anda seharusnya dapat mengkonfigurasi integrasi FreeRADIUS di Ubuntu dengan Mikrotik HAP Lite untuk autentikasi WPA2-Enterprise. Jika ada pertanyaan lebih lanjut atau masalah spesifik, silakan beri tahu saya.





## Panduan Setup FreeRADIUS Server menggunakan Docker Compose

Berikut adalah panduan lengkap untuk membuat FreeRADIUS server menggunakan Docker Compose dan mengintegrasikannya dengan Mikrotik HAP Lite untuk autentikasi WPA2-Enterprise.

### 1. Persiapan Lingkungan Docker

Pastikan Docker dan Docker Compose sudah terpasang di Ubuntu Anda. Jika belum, install dengan:

```bash
sudo apt update
sudo apt install docker.io docker-compose
sudo systemctl enable docker
sudo systemctl start docker
sudo usermod -aG docker $USER
```

Logout dan login kembali agar perubahan grup pengguna diterapkan.

### 2. Menyiapkan Struktur Direktori

Buat struktur direktori untuk project FreeRADIUS:

```bash
mkdir -p ~/freeradius-docker/{config,certs,users,logs}
cd ~/freeradius-docker
```

### 3. Membuat Docker Compose File

Buat file `docker-compose.yml`:

```bash
nano docker-compose.yml
```

Tambahkan konfigurasi berikut:

### 4. Menyiapkan File Konfigurasi FreeRADIUS

#### Konfigurasi radiusd.conf

```bash
mkdir -p ./config/mods-enabled ./config/sites-enabled
nano ./config/radiusd.conf
```

#### Konfigurasi clients.conf

```bash
nano ./config/clients.conf
```

#### Konfigurasi File Users

```bash
nano ./config/users
```

#### Konfigurasi EAP untuk WPA2-Enterprise

Buat direktori untuk modul enabled dan buat file eap:

```bash
mkdir -p ./config/mods-enabled
nano ./config/mods-enabled/eap
```

#### Konfigurasi Default Site

```bash
mkdir -p ./config/sites-enabled
nano ./config/sites-enabled/default
```

#### Konfigurasi Inner-Tunnel

```bash
nano ./config/sites-enabled/inner-tunnel
```

### 5. Membuat Sertifikat SSL untuk WPA2-Enterprise

Buat skrip untuk menghasilkan sertifikat:

```bash
nano ./generate-certs.sh
```

Jalankan skrip untuk menghasilkan sertifikat:

```bash
mkdir -p ./certs
chmod +x ./generate-certs.sh
./generate-certs.sh
```

### 6. Menjalankan Docker Compose

Setelah semua konfigurasi selesai, jalankan Docker Compose:

```bash
docker-compose up -d
```

Periksa apakah container berjalan dengan baik:

```bash
docker-compose ps
docker logs freeradius
```

### 7. Pengujian FreeRADIUS Server

Pastikan server berjalan dengan benar dengan menguji autentikasi pengguna:

```bash
docker exec -it freeradius radtest testuser password123 localhost 0 rahasia123
```

Jika berhasil, Anda akan melihat pesan "Access-Accept".

### 8. Konfigurasi Mikrotik HAP Lite

#### Konfigurasi RADIUS Client di Mikrotik

1. Masuk ke Mikrotik melalui WinBox atau WebFig
2. Buka menu RADIUS
3. Tambahkan server RADIUS baru:
   * Buka menu **RADIUS**
   * Klik tombol **+** untuk menambahkan server baru
   * Isi detail berikut:
     * **Address**: \[IP Server Ubuntu Docker Anda]
     * **Secret**: rahasia123 (sama dengan yang dikonfigurasi di clients.conf)
     * **Service**: wireless
     * **Authentication Port**: 1812
     * **Accounting Port**: 1813
   * Klik **OK**

#### Konfigurasi Wireless untuk WPA2-Enterprise

1. Buka menu **Wireless**
2. Pilih interface wireless Anda (biasanya wlan1)
3. Buka tab **Security Profiles**
4. Buat profil keamanan baru atau edit yang sudah ada:
   * **Mode**: dynamic keys
   * **Authentication Types**: WPA2 PSK (hapus tanda centang opsi lain)
   * **Unicast Ciphers**: aes ccm (hapus tanda centang opsi lain)
   * **Group Ciphers**: aes ccm (hapus tanda centang opsi lain)
   * **WPA2 Pre-Shared Key**: (kosongkan)
   * **Supplicant Identity**: (kosongkan)
   * Centang **Management Protection**
   * Centang **WPA2-EAP**
   * Klik **OK**
5. Kembali ke tab **Wireless**
6. Pilih interface wireless Anda
7. Pilih **Security Profile** yang baru Anda buat
8. Di tab **RADIUS**, atur:
   * Centang **Use RADIUS**
   * Pilih server RADIUS yang telah Anda tambahkan sebelumnya
   * Klik **OK**

### 9. Mengelola User dengan Database (Opsional)

Jika Anda ingin mengelola user melalui database, buat skema MySQL untuk FreeRADIUS:

Cara menggunakan file SQL ini:

```bash
# Salin file schema ke container
docker cp radius-schema.sql freeradius-db:/tmp/

# Jalankan SQL di database
docker exec -it freeradius-db mysql -uroot -pradiuspass radius -e "source /tmp/radius-schema.sql"
```

### 10. Konfigurasi FreeRADIUS untuk Menggunakan MySQL

Jika Anda ingin menggunakan database MySQL, tambahkan konfigurasi berikut:

Simpan file ini ke `./config/mods-enabled/sql` dan aktifkan modul SQL dengan mengedit file default dan inner-tunnel untuk menghapus tanda `-` dari `-sql`.

### 11. Pengujian Koneksi

1. Pada perangkat klien (laptop, smartphone), coba sambungkan ke jaringan WiFi:
   * SSID: \[Nama jaringan WiFi Anda]
   * Security: WPA2-Enterprise
   * EAP Method: PEAP
   * Phase 2 Authentication: MSCHAPv2
   * Identity: testuser (atau username yang Anda buat)
   * Password: password123 (atau password yang Anda tentukan)
2. Perangkat seharusnya dapat terhubung jika konfigurasi sudah benar.

### 12. Pemecahan Masalah

Jika koneksi gagal, periksa log FreeRADIUS:

```bash
docker logs freeradius
```

Untuk logging yang lebih detail, restart container dengan mode debug:

```bash
docker-compose down
docker-compose up -d
docker exec -it freeradius radiusd -X
```

Dengan panduan ini, Anda telah berhasil menyiapkan FreeRADIUS server menggunakan Docker Compose dan mengintegrasikannya dengan Mikrotik HAP Lite untuk autentikasi WPA2-Enterprise. Apakah ada bagian spesifik yang perlu penjelasan lebih lanjut?
