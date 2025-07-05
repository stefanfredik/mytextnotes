---
description: All About DNS
---

# DNS

## Menguji DNS Server

Untuk mengetes kecepatan respon DNS server di sistem operasi Linux, Anda dapat menggunakan beberapa tools bawaan atau pihak ketiga. Berikut adalah beberapa metode yang bisa Anda gunakan:

***

#### **1. Menggunakan `dig`**

Tool `dig` (Domain Information Groper) digunakan untuk melakukan query ke DNS server dan mengukur waktu respon.

**Langkah:**

1. Buka terminal.
2.  Gunakan perintah berikut:

    ```bash
    dig @<IP_DNS_SERVER> <domain> +noall +stats
    ```

    Contoh:

    ```bash
    dig @8.8.8.8 google.com +noall +stats
    ```
3.  Output akan memberikan waktu respon di bagian `Query time`:

    ```
    ;; Query time: 23 msec
    ```

***

#### **2. Menggunakan `nslookup`**

`nslookup` juga bisa digunakan untuk mengetes waktu respon DNS server.

**Langkah:**

1.  Jalankan perintah berikut:

    ```bash
    nslookup <domain> <IP_DNS_SERVER>
    ```

    Contoh:

    ```bash
    nslookup google.com 8.8.8.8
    ```
2. Perhatikan waktu yang dibutuhkan untuk mendapatkan hasil. Namun, `nslookup` tidak memberikan waktu respon secara langsung, sehingga Anda perlu mencatat waktu secara manual.

***

#### **3. Menggunakan `ping` untuk Nama Domain**

`ping` bisa digunakan untuk mengecek apakah nama domain dapat di-resolve dan mendapatkan IP dengan cepat.

**Langkah:**

1.  Gunakan perintah:

    ```bash
    ping -c 1 <domain>
    ```

    Contoh:

    ```bash
    ping -c 1 google.com
    ```
2. Output akan menunjukkan waktu respon DNS dalam milidetik (waktu round-trip).

***

#### **4. Menggunakan `dnsperf`**

`dnsperf` adalah tool khusus untuk menguji performa DNS server dengan banyak query.

**Instalasi:**

1.  Instal tool ini (tergantung distro Anda):

    ```bash
    sudo apt install dnsperf
    ```
2.  Gunakan perintah berikut:

    ```bash
    dnsperf -s <IP_DNS_SERVER> -d <file_domain_list> -l <duration>
    ```

    Contoh:

    ```bash
    dnsperf -s 8.8.8.8 -d domains.txt -l 10
    ```

    File `domains.txt` berisi daftar domain yang ingin diuji, satu per baris.

***

#### **5. Menggunakan `systemd-resolve`**

Jika Anda menggunakan distribusi Linux modern dengan systemd, gunakan perintah ini:

**Langkah:**

1.  Jalankan:

    ```bash
    systemd-resolve --statistics
    ```
2. Ini akan menunjukkan statistik termasuk waktu respon cache DNS lokal.

***

#### **6. Menggunakan `drill` (Bagian dari `ldnsutils`)**

`drill` adalah alternatif `dig` dengan output lebih sederhana.

**Langkah:**

1.  Instal `drill` jika belum ada:

    ```bash
    sudo apt install ldnsutils
    ```
2.  Gunakan perintah berikut:

    ```bash
    drill @<IP_DNS_SERVER> <domain>
    ```

    Contoh:

    ```bash
    drill @8.8.8.8 google.com
    ```
3. Perhatikan waktu respon di bagian `Query time`.

***

Metode-metode di atas akan membantu Anda mengukur performa DNS server untuk kebutuhan troubleshooting atau analisis kecepatan respon. Pilih tool yang paling sesuai dengan preferensi Anda.



## Setup Cloudflare DNS

Berikut ini adalah **panduan lengkap dan praktis** untuk **membuat web server Node.js di Ubuntu** agar **bisa diakses melalui subdomain `rwb.kopiflores.my.id` menggunakan HTTPS**, **tanpa IP publik**, menggunakan **Cloudflare Tunnel**.

***

### 🌐 Arsitektur

```
Browser → Cloudflare DNS → Cloudflare Tunnel → Node.js (port 80, di Ubuntu lokal)
```

***

### 🧰 Yang Dibutuhkan

✅ Domain `kopiflores.my.id` sudah dikelola di Cloudflare\
✅ Subdomain `rwb.kopiflores.my.id` akan dibuat\
✅ Server Ubuntu (lokal/VPS) sudah ada\
✅ Node.js App jalan di port `80`

***

### 🪜 LANGKAH 1: Install Node.js & Jalankan Aplikasi

Jika belum punya Node.js:

```bash
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs
```

Contoh `app.js` sederhana:

```js
const http = require('http');
const port = 80;

const server = http.createServer((req, res) => {
  res.writeHead(200, {'Content-Type': 'text/plain'});
  res.end('Halo dari Node.js!');
});

server.listen(port, () => {
  console.log(`Server running at http://localhost:${port}`);
});
```

Jalankan:

```bash
sudo node app.js
```

***

### 🪜 LANGKAH 2: Install `cloudflared`

```bash
wget https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-amd64.deb
sudo dpkg -i cloudflared-linux-amd64.deb
```

***

### 🪜 LANGKAH 3: Login ke Cloudflare

```bash
cloudflared tunnel login
```

* Akan membuka browser
* Pilih domain: `kopiflores.my.id`
* Setelah sukses, file auth tersimpan di `~/.cloudflared/`

***

### 🪜 LANGKAH 4: Buat Tunnel Baru

```bash
cloudflared tunnel create rwb-tunnel
```

> Akan membuat file `rwb-tunnel.json` di `~/.cloudflared`

***

### 🪜 LANGKAH 5: Buat File Konfigurasi Tunnel

Buat file `~/.cloudflared/config.yml`:

```yaml
tunnel: rwb-tunnel
credentials-file: /home/namakamu/.cloudflared/rwb-tunnel.json

ingress:
  - hostname: rwb.kopiflores.my.id
    service: http://localhost:80
  - service: http_status:404
```

> Ganti `/home/namakamu/` sesuai path home kamu (cek dengan `echo $HOME`).

***

### 🪜 LANGKAH 6: Tambah Subdomain di DNS (otomatis)

```bash
cloudflared tunnel route dns rwb-tunnel rwb.kopiflores.my.id
```

> Ini akan otomatis menambahkan record CNAME ke Cloudflare.

***

### 🪜 LANGKAH 7: Jalankan Tunnel

```bash
cloudflared tunnel run rwb-tunnel
```

> Web server kamu sekarang bisa diakses via:\
> `https://rwb.kopiflores.my.id`

✅ Otomatis sudah HTTPS via Cloudflare\
✅ Tidak perlu IP Publik\
✅ Tidak perlu open port di router

***

### 🔁 (Opsional) Setup Autostart Tunnel

Agar tunnel otomatis jalan saat boot:

```bash
sudo cloudflared service install
```

***

### 🛡️ Tips Keamanan

* Pastikan firewall hanya mengizinkan akses dari localhost kalau perlu (web server hanya dibuka untuk cloudflared).
* Gunakan `pm2` untuk menjalankan Node.js jika ingin produksi:

```bash
npm install -g pm2
pm2 start app.js
pm2 startup
pm2 save
```

***

### 🧪 Cek dan Uji

1. Akses `https://rwb.kopiflores.my.id`
2.  Cek log di:

    ```bash
    cloudflared tunnel logs
    ```

***

Kalau kamu ingin integrasi dengan **Docker**, **reverse proxy Nginx**, atau **lebih dari satu subdomain**, tinggal bilang — akan saya bantu buatkan juga. Mau lanjut ke sana?
