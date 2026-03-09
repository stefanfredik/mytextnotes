# Cloudflare

Siap 👍\
Aku buatkan **panduan step-by-step yang rapi dan praktis** khusus **Linux + Laravel**, supaya web Laravel kamu yang **IP private / lokal** bisa diakses publik lewat:

👉 [**http://undangandigital.sbs**](http://undangandigital.sbs/) (nanti otomatis **HTTPS** dari Cloudflare)

Aku asumsikan:

* Laravel jalan di **Linux**
* Web Laravel jalan di `http://127.0.0.1:8000` **atau** `http://localhost`
* Domain **undangandigital.sbs** sudah aktif

Kalau beda port, tinggal ganti saja.

***

## 🧭 ARSITEKTUR SINGKAT

```
Internet
  ↓
Cloudflare (DNS + HTTPS)
  ↓
Cloudflare Tunnel (cloudflared)
  ↓
Laravel (127.0.0.1:8000)
```

***

### 1️⃣ Pastikan Domain Pakai Cloudflare

1. Login ke **Cloudflare Dashboard**
2. Add site → `undangandigital.sbs`
3. Update **Nameserver** di registrar
4. Tunggu status **Active**

⚠️ Jangan buat A record ke IP publik

***

### 2️⃣ Install cloudflared di Linux

#### Ubuntu / Debian

```bash
wget https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb
sudo dpkg -i cloudflared-linux-amd64.deb
```

Cek:

```bash
cloudflared --version
```

***

### 3️⃣ Login Cloudflare dari Server

```bash
cloudflared tunnel login
```

* Browser terbuka
* Pilih domain **undangandigital.sbs**
* Jika sukses → cert otomatis tersimpan

***

### 4️⃣ Buat Tunnel

```bash
cloudflared tunnel create undangan-laravel
```

Catat:

* **Tunnel UUID**
* File credential `.json`

Contoh:

```
Tunnel ID: 111aaa22-bbbb-cccc-dddd-333eee444
```

***

### 5️⃣ Buat DNS ke Tunnel

Expose root domain:

```bash
cloudflared tunnel route dns undangan-laravel undangandigital.sbs
```

(Cloudflare otomatis bikin **CNAME ke tunnel**)

***

### 6️⃣ Konfigurasi Tunnel ke Laravel

Buat file:

```bash
mkdir -p ~/.cloudflared
nano ~/.cloudflared/config.yml
```

Isi:

```yaml
tunnel: 111aaa22-bbbb-cccc-dddd-333eee444
credentials-file: /home/$USER/.cloudflared/111aaa22-bbbb-cccc-dddd-333eee444.json

ingress:
  - hostname: undangandigital.sbs
    service: http://127.0.0.1:8000
  - service: http_status:404
```

📌 Jika pakai **Apache / Nginx**, ganti ke:

```yaml
service: http://127.0.0.1:80
```

***

### 7️⃣ Jalankan Tunnel

```bash
cloudflared tunnel run undangan-laravel
```

Sekarang akses:

```
https://undangandigital.sbs
```

🎉 Laravel kamu sudah online tanpa IP publik!

***

### 8️⃣ Jalankan sebagai Service (Auto Start)

Supaya tetap hidup setelah reboot:

```bash
sudo cloudflared service install
sudo systemctl start cloudflared
sudo systemctl enable cloudflared
```

Cek status:

```bash
systemctl status cloudflared
```

***

### 9️⃣ Setting Laravel (PENTING)

#### `.env`

```env
APP_URL=https://undangandigital.sbs
```

Clear cache:

```bash
php artisan config:clear
php artisan cache:clear
```

***

### 🔐 (OPSIONAL) Aktifkan HTTPS Full

Cloudflare → **SSL/TLS**

* Mode: **Full**
* Always Use HTTPS: ON

***

### 🧪 Troubleshooting Umum

#### ❌ 502 Bad Gateway

* Laravel tidak jalan
* Port salah

```bash
ss -tulpn | grep 8000
```

***

#### ❌ Blank page / asset rusak

Pastikan:

```env
APP_URL=https://undangandigital.sbs
```

***

#### ❌ Tunnel mati

```bash
journalctl -u cloudflared -f
```

***

### 🔥 Best Practice (Sangat Disarankan)

* Tambahkan **Cloudflare Access (login email)**
* Batasi hanya HTTP/HTTPS



### Setup Cloudflare Tunnel untuk Beda Domain

Cloudflare Tunnel memungkinkan kamu expose layanan lokal ke internet tanpa perlu membuka port, dan bisa dipakai untuk multiple domain sekaligus.

***

#### 1. Install cloudflared

```bash
# Linux (Debian/Ubuntu)
curl -L --output cloudflared.deb https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb
sudo dpkg -i cloudflared.deb

# macOS
brew install cloudflare/cloudflare/cloudflared
```

***

#### 2. Login & Buat Tunnel

```bash
# Login ke Cloudflare account
cloudflared tunnel login

# Buat tunnel baru
cloudflared tunnel create nama-tunnel-kamu
```

Setelah ini akan muncul **Tunnel ID** dan file credentials di `~/.cloudflared/`.

***

#### 3. Buat Config File

Buat file `~/.cloudflared/config.yml` — ini bagian pentingnya untuk **beda domain**:

```yaml
tunnel: <TUNNEL_ID>
credentials-file: /root/.cloudflared/<TUNNEL_ID>.json

ingress:
  # Domain pertama → service A
  - hostname: app1.domain-a.com
    service: http://localhost:3000

  # Domain kedua → service B
  - hostname: app2.domain-b.com
    service: http://localhost:4000

  # Subdomain di domain yang sama
  - hostname: api.domain-a.com
    service: http://localhost:5000

  # Catch-all (wajib ada di akhir)
  - service: http_status:404
```

> **Catatan:** Semua domain yang dipakai **harus sudah ada di Cloudflare** (nameserver sudah pointing ke CF).

***

#### 4. Tambah DNS Record

Untuk setiap hostname, jalankan:

```bash
cloudflared tunnel route dns nama-tunnel-kamu app1.domain-a.com
cloudflared tunnel route dns nama-tunnel-kamu app2.domain-b.com
cloudflared tunnel route dns nama-tunnel-kamu api.domain-a.com
```

Ini otomatis buat CNAME record di Cloudflare DNS.

***

#### 5. Jalankan Tunnel

```bash
# Test dulu
cloudflared tunnel run nama-tunnel-kamu

# Atau jalankan sebagai service (supaya auto-start)
sudo cloudflared service install
sudo systemctl start cloudflared
sudo systemctl enable cloudflared
```

***

#### Tips Tambahan

| Kebutuhan      | Solusi                                           |
| -------------- | ------------------------------------------------ |
| HTTPS otomatis | Cloudflare handle ini secara default             |
| WebSocket      | Tambah `disableChunkedEncoding: true` di ingress |
| Basic Auth     | Gunakan Cloudflare Access di dashboard           |
| Load balancing | Bisa pakai beberapa `service` dengan weight      |

***

#### Troubleshooting

```bash
# Cek status tunnel
cloudflared tunnel info nama-tunnel-kamu

# Lihat log
journalctl -u cloudflared -f

# Test koneksi
cloudflared tunnel --loglevel debug run nama-tunnel-kamu
```

Yang paling sering jadi masalah: domain belum aktif di Cloudflare (masih pakai nameserver lama). Pastikan status domain di dashboard CF sudah **Active** sebelum setup tunnel.

### Setup Cloudflare Tunnel: mbeliling

#### 1. Install & Login

```bash
# Install cloudflared (Linux)
curl -L --output cloudflared.deb https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb
sudo dpkg -i cloudflared.deb

# Login
cloudflared tunnel login
```

***

#### 2. Buat Tunnel

```bash
cloudflared tunnel create mbeliling
```

Catat **Tunnel ID** yang muncul, contoh: `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`

***

#### 3. Buat Config File

```bash
nano ~/.cloudflared/mbeliling-config.yml
```

Isi dengan:

```yaml
tunnel: mbeliling
credentials-file: /root/.cloudflared/<TUNNEL_ID>.json

ingress:
  - hostname: mbeliling.organisasi.online
    service: http://localhost:80

  # Catch-all wajib ada
  - service: http_status:404
```

> Ganti `<TUNNEL_ID>` dengan ID yang didapat di langkah 2. Ganti port `80` sesuai service kamu (misal `3000`, `8080`, dll).

***

#### 4. Tambah DNS Record

```bash
cloudflared tunnel --config ~/.cloudflared/mbeliling-config.yml route dns mbeliling mbeliling.organisasi.online
```

***

#### 5. Jalankan Tunnel

```bash
# Test manual dulu
cloudflared tunnel --config ~/.cloudflared/mbeliling-config.yml run mbeliling
```

Kalau sudah oke, pasang sebagai service:

```bash
# Copy config ke lokasi default service
sudo cp ~/.cloudflared/mbeliling-config.yml /etc/cloudflared/config.yml

sudo cloudflared service install
sudo systemctl start cloudflared
sudo systemctl enable cloudflared
```

***

#### 6. Verifikasi

```bash
# Cek tunnel aktif
cloudflared tunnel info mbeliling

# Cek DNS sudah propagate
dig mbeliling.organisasi.online

# Cek status service
sudo systemctl status cloudflared
```

***

#### Ringkasan Konfigurasi

| Parameter   | Value                                 |
| ----------- | ------------------------------------- |
| Tunnel Name | `mbeliling`                           |
| Config File | `~/.cloudflared/mbeliling-config.yml` |
| Domain      | `organisasi.online`                   |
| Subdomain   | `mbeliling.organisasi.online`         |
| Credentials | `/root/.cloudflared/<TUNNEL_ID>.json` |

Setelah DNS propagate (biasanya 1-5 menit), `mbeliling.organisasi.online` sudah bisa diakses dari internet.





## Multiple Domain

Tentu, Anda bisa menggunakan teknik Mirroring/Redirect melalui Cloudflare. Ini adalah solusi paling stabil jika salah satu domain mengalami kendala SSL atau DNS.

Ada dua cara untuk melakukannya, silakan pilih yang menurut Anda paling nyaman:

Opsi 1: Redirect via Cloudflare Page Rules (Disarankan) Dengan cara ini, saat orang mengetik mbeliling.organisasi.online, mereka akan otomatis diarahkan (Redirect) ke mbeliling.kopiflores.my.id.

1. Buka Dashboard Cloudflare -> Pilih domain organisasi.online.
2. Masuk ke menu Rules -> Page Rules.
3. Klik Create Page Rule.
4. Isi kolom URL dengan: mbeliling.organisasi.online/\*
5. Pilih setting: Forwarding URL -> 301 (Permanent Redirect).
6. Isi target URL dengan: https://mbeliling.kopiflores.my.id/$1
7. Klik Save and Deploy.

Opsi 2: DNS Mirroring (CNAME ke CNAME) Anda tetap bisa menggunakan alamat mbeliling.organisasi.online di bar alamat browser, namun datanya diambil dari mbeliling.kopiflores.my.id.

1. Buka Dashboard Cloudflare -> Pilih domain organisasi.online.
2. Masuk ke menu DNS -> Records.
3. Edit/Hapus record mbeliling yang lama.
4. Tambahkan record baru:
   * Type: CNAME
   * Name: mbeliling
   * Target: mbeliling.kopiflores.my.id
   * Proxy Status: Proxied (Awan Oranye)

***

Mengapa NXDOMAIN Masih Muncul? Jika setelah melakukan salah satu opsi di atas Anda tetap melihat NXDOMAIN, mohon pastikan:

1. Nameserver: Di panel domain (tempat Anda beli domain), pastikan Nameserver sudah diarahkan ke Cloudflare (contoh: dave.ns.cloudflare.com & elsa.ns.cloudflare.com).
2. Status Domain: Pastikan domain organisasi.online berstatus "Active" di Dashboard Cloudflare. Jika statusnya "Pending Nameserver Update", maka pengaturan DNS apa pun tidak akan berpengaruh.

Saran Saya: Gunakan Opsi 1 (Redirect). Ini adalah cara tercepat untuk memastikan user sampai ke aplikasi Anda yang sudah jalan di mbeliling.kopiflores.my.id tanpa pusing masalah SSL.
