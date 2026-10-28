---
icon: laptop-code
---

# Mysql

Untuk membuat user baru di MariaDB, ikuti langkah-langkah berikut:

#### 1. **Login ke MariaDB**

Jalankan perintah berikut untuk masuk ke MariaDB sebagai root:

```sh
mysql -u root -p
```

Masukkan password root saat diminta.

***

#### 2. **Buat User Baru**

Gunakan perintah berikut untuk membuat user baru:

```sql
CREATE USER 'nama_user'@'localhost' IDENTIFIED BY 'password_user';
```

* Gantilah `'nama_user'` dengan nama user yang ingin Anda buat.
* Gantilah `'password_user'` dengan password yang diinginkan.
* `'localhost'` berarti user hanya bisa login dari server lokal. Jika ingin mengizinkan akses dari mana saja, gunakan `'%'` sebagai host.

***

#### 3. **Beri Hak Akses ke Database**

Jika ingin memberi akses penuh ke database tertentu:

```sql
GRANT ALL PRIVILEGES ON nama_database.* TO 'nama_user'@'localhost';
```

Jika hanya ingin memberi akses tertentu, misalnya SELECT, INSERT, dan UPDATE:

```sql
GRANT SELECT, INSERT, UPDATE ON nama_database.* TO 'nama_user'@'localhost';
```

***

#### 4. **Simpan Perubahan**

Jalankan perintah berikut agar perubahan langsung berlaku:

```sql
FLUSH PRIVILEGES;
```

***

#### 5. **Cek User yang Ada (Opsional)**

Untuk memastikan user telah dibuat:

```sql
SELECT User, Host FROM mysql.user;
```

***

#### 6. **Logout dan Uji Login User Baru**

Keluar dari MariaDB:

```sql
EXIT;
```

Lalu coba login dengan user yang baru dibuat:

```sh
mysql -u nama_user -p
```

Masukkan password yang telah dibuat.

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

## Akses Database

Agar database MariaDB bisa diakses dari luar localhost, ikuti langkah-langkah berikut:

#### 1. **Edit Konfigurasi MariaDB**

Buka file konfigurasi MariaDB (`my.cnf` atau `mysqld.cnf`), biasanya terletak di:

* **Debian/Ubuntu:** `/etc/mysql/mariadb.conf.d/50-server.cnf`
* **CentOS/RHEL:** `/etc/my.cnf.d/server.cnf`
* **Windows:** `C:\Program Files\MariaDB 10.x\data\my.ini`

Cari baris berikut:

```ini
bind-address = 127.0.0.1
```

Ubah menjadi:

```ini
bind-address = 0.0.0.0
```

Atau jika tidak ada, tambahkan di bagian `[mysqld]`.

#### 2. **Beri Akses ke User MariaDB**

Masuk ke MariaDB:

```sh
mysql -u root -p
```

Buat atau ubah user agar bisa diakses dari luar, misalnya untuk user `myuser`:

```sql
GRANT ALL PRIVILEGES ON *.* TO 'myuser'@'%' IDENTIFIED BY 'mypassword' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

> **Note:** `'%'` berarti bisa diakses dari mana saja. Anda bisa menggantinya dengan IP tertentu untuk keamanan, misalnya `'192.168.1.100'`.

#### 3. **Cek Firewall**

Jika menggunakan **UFW (Ubuntu/Debian)**, izinkan akses ke port MariaDB (default: 3306):

```sh
sudo ufw allow 3306/tcp
sudo ufw reload
```

Jika menggunakan **firewalld (CentOS/RHEL)**:

```sh
sudo firewall-cmd --add-service=mysql --permanent
sudo firewall-cmd --reload
```

#### 4. **Restart MariaDB**

Setelah mengubah konfigurasi, restart MariaDB agar perubahan diterapkan:

```sh
sudo systemctl restart mariadb  # Untuk Linux
net stop MariaDB && net start MariaDB  # Untuk Windows (Command Prompt sebagai Administrator)
```

#### 5. **Coba Koneksi dari Remote**

Dari komputer lain, coba koneksi menggunakan:

```sh
mysql -h IP_SERVER -u myuser -p
```

Atau dengan aplikasi seperti **DBeaver, MySQL Workbench, atau HeidiSQL**.

***

Jika masih tidak bisa:

* Pastikan MariaDB berjalan (`sudo systemctl status mariadb`).
* Cek apakah ada firewall eksternal (misalnya di cloud provider).
* Gunakan `netstat -tulnp | grep 3306` untuk memastikan MariaDB mendengarkan koneksi di semua antarmuka (`0.0.0.0:3306`).

Coba langkah-langkah ini dan beri tahu jika masih ada kendala! 🚀
