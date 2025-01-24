# Docker Script

## Docker Container

### Running Docker Container automation after restart

Agar container Docker dapat hidup kembali secara otomatis setelah restart sistem, Anda perlu mengatur kebijakan restart (restart policy) saat menjalankan atau mengkonfigurasi container. Berikut caranya:

***

#### **1. Gunakan Opsi `--restart` Saat Membuat Container**

Ketika Anda menjalankan container menggunakan `docker run`, tambahkan opsi `--restart`. Ada beberapa mode yang bisa Anda pilih:

* `no`: (default) Container tidak akan restart secara otomatis.
* `always`: Container selalu restart, bahkan jika dihentikan secara manual.
* `unless-stopped`: Container restart otomatis kecuali dihentikan secara manual.
* `on-failure`: Container restart hanya jika exit code menunjukkan kesalahan (exit code ≠ 0).

**Contoh:**

```bash
docker run --name my-container --restart always -d my-image
```

Dalam contoh ini:

* `--restart always` memastikan container akan hidup kembali setelah sistem restart.
* `-d` menjalankan container di mode daemon (background).

***

#### **2. Ubah Kebijakan Restart pada Container yang Sudah Ada**

Jika Anda sudah memiliki container yang berjalan tanpa kebijakan restart, Anda dapat mengubahnya menggunakan perintah `docker update`.

**Contoh:**

```bash
docker update --restart unless-stopped my-container
```

Perintah ini mengatur kebijakan restart pada container bernama `my-container` menjadi `unless-stopped`.

***

#### **3. Verifikasi Kebijakan Restart**

Untuk memastikan bahwa kebijakan restart telah diterapkan, gunakan perintah berikut:

```bash
docker inspect -f '{{ .HostConfig.RestartPolicy.Name }}' my-container
```

Hasilnya akan menunjukkan kebijakan yang diterapkan (misalnya, `always` atau `unless-stopped`).

***

#### **4. Gunakan Docker Compose (Opsional)**

Jika Anda menggunakan Docker Compose untuk mengelola container, Anda dapat menambahkan pengaturan `restart` dalam file `docker-compose.yml`.

**Contoh:**

```yaml
version: "3.8"
services:
  my-service:
    image: my-image
    restart: always
```

Setelah mengatur file tersebut, jalankan kembali Compose:

```bash
docker-compose up -d
```

***

#### **5. Pastikan Docker Service Aktif**

Agar container dapat otomatis berjalan setelah restart, pastikan layanan Docker diaktifkan pada sistem Anda.

**Periksa status Docker:**

```bash
sudo systemctl status docker
```

**Aktifkan Docker agar berjalan otomatis saat boot:**

```bash
sudo systemctl enable docker
```

***

Dengan langkah-langkah di atas, container Anda akan secara otomatis hidup kembali setelah sistem atau Docker di-restart. Jika ada kendala, beri tahu saya, ya! 😊

```bash
```
