---
icon: database
description: All about database tips and trick.
---

# Database

## User Management

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

Selesai! 🚀

