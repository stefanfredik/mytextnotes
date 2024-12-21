# Linux User

## User Management

### Create User

```bash
sudo adduser [nama user]
```

Selanjutnya akan muncul promp password dan informasi name dan lainya. Silahkan diisi sesuai dengan kriteria user yang akan dibuat.

### Mengubah status user menjadi sudo user

User yang baru dibuat defaultnya tidak dapat mengakses root permission. Untuk bisa mengakses root mode maka diperlukan untuk menbah user tersebut ke dalam group user sudo dengan perintah berikut : &#x20;

```bash
sudo usermod -aG sudo [nama user]
```

Setelah menambahkan user ke group sudo, user tersebut telah mendapatkan akses ke root permissions. Ini berarti mereka dapat menjalankan perintah dengan hak akses root menggunakan `sudo` di depan perintah yang ingin dijalankan.

### Modifikasi User

Untuk melakukan modifikasi pada pengguna, kita dapat menggunakan perintah `usermod` yang memungkinkan kita untuk mengubah berbagai atribut pengguna. Berikut adalah contoh untuk mengganti nama pengguna:

```bash
sudo usermod -l newusername oldusername
```

Perintah di atas mengganti nama pengguna dari `oldusername` ke `newusername`. Pastikan untuk mengganti parameter sesuai dengan kebutuhan.

Selain itu, untuk mengubah direktori home dari pengguna, Anda dapat menggunakan perintah berikut:

```bash
sudo usermod -d /path/to/new/home -m username
```

Parameter `-m` memastikan bahwa konten dari direktori home lama dipindahkan ke lokasi yang baru.
