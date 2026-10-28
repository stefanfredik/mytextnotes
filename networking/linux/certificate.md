# Certificate

Berikut adalah **tutorial lengkap membuat sertifikat SSL dari Let’s Encrypt di Ubuntu** dan **menggunakannya di Mikrotik User Manager v5 (Userman)**.

***

### 🎯 Tujuan

* Mendapatkan sertifikat SSL gratis dari Let’s Encrypt di server Ubuntu.
* Meng-export sertifikat ke format yang bisa digunakan oleh MikroTik (Userman).
* Meng-upload dan mengaktifkan sertifikat di MikroTik.

***

### 🛠️ Persiapan

* Server Ubuntu dengan akses `root` atau `sudo`.
* Domain publik yang mengarah ke IP public Mikrotik (misalnya `userman.domainkamu.com`).
* MikroTik dengan Userman v5 aktif.
* Port 80 terbuka ke server Ubuntu (untuk HTTP validation Let’s Encrypt).

***

### 🔧 Langkah 1: Instalasi Certbot di Ubuntu

```bash
sudo apt update
sudo apt install certbot
```

***

### 🔐 Langkah 2: Dapatkan Sertifikat dari Let’s Encrypt

Misalnya, domain kamu adalah `userman.domainkamu.com`. Gunakan:

```bash
sudo certbot certonly --standalone -d userman.domainkamu.com
```

Certbot akan:

* Menyimpan sertifikat di:
  * `/etc/letsencrypt/live/userman.domainkamu.com/fullchain.pem`
  * `/etc/letsencrypt/live/userman.domainkamu.com/privkey.pem`

***

### 🔄 Langkah 3: Konversi Sertifikat ke Format `.pem` dan `.key` untuk MikroTik

#### MikroTik butuh file:

* `.crt` (certificate)
* `.key` (private key)

Langkah:

```bash
sudo openssl x509 -in /etc/letsencrypt/live/userman.domainkamu.com/fullchain.pem -out /tmp/userman.crt
sudo cp /etc/letsencrypt/live/userman.domainkamu.com/privkey.pem /tmp/userman.key
```

Lalu ubah permission agar bisa diakses:

```bash
sudo chmod 644 /tmp/userman.*
```

***

### 📦 Langkah 4: Export Sertifikat ke Format `.pem` dan Upload ke MikroTik

Gabungkan sertifikat dan key menjadi satu file `.pem` (opsional):

```bash
cat /tmp/userman.crt /tmp/userman.key > /tmp/userman.pem
```

**Upload ke MikroTik:**

* Gunakan Winbox / Webfig:
  * Menu: **Files**
  * Upload file `userman.crt` dan `userman.key` (atau `userman.pem` jika digabung).

**Alternatif SCP Upload:**

```bash
scp /tmp/userman.crt admin@192.168.88.1:
scp /tmp/userman.key admin@192.168.88.1:
```

***

### 🧩 Langkah 5: Import Sertifikat di MikroTik

#### Via Terminal MikroTik:

```bash
/certificate import file-name=userman.crt
/certificate import file-name=userman.key
```

Saat ditanya password, tekan `Enter` saja jika tidak ada.

***

### ✅ Langkah 6: Aktifkan Sertifikat di User Manager

1. Buka **Userman Web Interface**.
2. Masuk ke **Settings / Server**.
3. Pilih sertifikat yang telah diimport (biasanya namanya sesuai file `.crt`).
4. Simpan perubahan.

***

### 🔁 (Opsional) Otomatisasi Renew Let’s Encrypt + Konversi Sertifikat

Edit crontab:

```bash
sudo crontab -e
```

Tambahkan:

```bash
0 2 * * * certbot renew --quiet && openssl x509 -in /etc/letsencrypt/live/userman.domainkamu.com/fullchain.pem -out /tmp/userman.crt && cp /etc/letsencrypt/live/userman.domainkamu.com/privkey.pem /tmp/userman.key
```

***

### 🧪 Tes

* Akses Userman via `https://userman.domainkamu.com`
* Pastikan sertifikat valid dan koneksi HTTPS aman.

***

Jika kamu menggunakan **RouterOS v7**, dukungan terhadap TLS dan sertifikat jauh lebih baik, dan kamu juga bisa menggunakan `Let's Encrypt Client` di MikroTik melalui skrip — tapi metode di atas tetap lebih stabil via Ubuntu.
