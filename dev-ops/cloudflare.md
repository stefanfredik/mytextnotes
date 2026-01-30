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

