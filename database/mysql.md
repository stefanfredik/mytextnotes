---
icon: laptop-code
---

# Mysql

### Database User

#### Create User

Add User&#x20;

```bash
CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password';
```

Grant all privileges to new user.

```bash
GRANT ALL PRIVILEGES ON mydatabase.* TO 'newuser'@'localhost';
```

Flush Privileges

```bash
FLUSH PRIVILEGES;
```

## Set Phpmyadmin Upload and POST Size

Untuk meningkatkan ukuran maksimum upload di **phpMyAdmin**, Anda perlu mengubah beberapa konfigurasi di **PHP** dan **phpMyAdmin**. Ikuti langkah-langkah berikut:

***

#### **1. Edit `php.ini`**

File `php.ini` biasanya berada di:

* **Linux (Ubuntu/Debian)**: `/etc/php/{versi}/apache2/php.ini`
* **Linux (CentOS/RHEL)**: `/etc/php.ini`
* **Windows**: `C:\xampp\php\php.ini` (jika menggunakan XAMPP)

Cari dan ubah nilai berikut sesuai kebutuhan:

```ini
upload_max_filesize = 128M
post_max_size = 128M
max_execution_time = 300
max_input_time = 300
memory_limit = 256M
```

* `upload_max_filesize` → Ukuran maksimum file yang bisa diupload.
* `post_max_size` → Harus lebih besar atau sama dengan `upload_max_filesize`.
* `max_execution_time` → Waktu maksimum eksekusi skrip.
* `memory_limit` → Harus lebih besar dari `post_max_size`.

***

#### **2. Edit `config.inc.php` phpMyAdmin (Opsional)**

Jika masih ada batasan di phpMyAdmin, coba edit file `config.inc.php`:

* **Linux**: `/etc/phpmyadmin/config.inc.php`
* **Windows (XAMPP)**: `C:\xampp\phpMyAdmin\config.inc.php`

Tambahkan atau edit baris berikut:

```php
$cfg['UploadDir'] = '';
$cfg['MaxSize'] = '128M';
```

***

#### **3. Restart Web Server**

Setelah mengubah konfigurasi, restart Apache/Nginx agar perubahan diterapkan:

**Untuk Apache:**

```sh
sudo systemctl restart apache2  # Ubuntu/Debian
sudo systemctl restart httpd     # CentOS/RHEL
```

**Untuk Nginx (jika menggunakan PHP-FPM):**

```sh
sudo systemctl restart nginx
sudo systemctl restart php-fpm
```

**Untuk XAMPP (Windows):**\
Restart Apache dari **XAMPP Control Panel**.

***

#### **4. Cek Perubahan**

Buka `phpinfo()` untuk memastikan perubahan sudah diterapkan:

1. Buat file `info.php` di direktori root web server (`/var/www/html` atau `C:\xampp\htdocs\`).
2.  Isi dengan kode berikut:

    ```php
    <?php phpinfo(); ?>
    ```
3.  Buka di browser:

    ```
    http://localhost/info.php
    ```
4. Cari `upload_max_filesize` dan `post_max_size` untuk memastikan nilainya sudah berubah.
