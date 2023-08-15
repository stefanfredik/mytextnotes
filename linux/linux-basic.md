---
description: All About Basic of Linux
---

# Linux Basic

## Linux Basic

### User

#### Root User

Root user adalah user paling tinggi yang memiliki akses penuh pada system linux.

Root user memiliki directory home dengan nama /root

```bash
root@kopi-flores#
```

#### User Biasa / Regular User

User biasa adalah user yang digunakan untuk kebutuhan standar di dalam Linux. Akses reguler user memiliki keterbatasan dan tidak memiliki askses penuh ke dalam system.

Reguler user memiliki directory home dengan nama user **`/home/nama_user`**

#### System User

Merupakan user yang dibuat oleh aplikasi tertentu untuk kebutuhan aplikasi tersebut.

System user tidak memiliki directory home. Id system user biasanya 1-999.

Tipe user ini tidak dapat digunakan oleh pengguna biasa. Sehingga kita tidak dapat login ke user tersebut.

Contoh : _`www-data`_   digunakan oleh aplikasi Apache2.



#### Cek Info User

Mengecek informasi id dari user

```bash
id [namauser]
```

### System

#### Command

```bash
losetup
```

Merupakan sebah perintah untuk melihat list loop device

```bash
lsbk
```

Melihat informasi disk pada device linux.

#### fsdisk

Merupakan perintah untuk mengelolah disk pada system linux.

#### parted

Merupakan perintah alternatif dari fsdisk

#### mkfs

Perintah yang digunakn untuk mengelolah file system, termasuk menformat partisi.

#### df

Merupakan perintah yang digunakan untuk melihat informasi ruang penyimpanan disk

```bash
bash f -h    #untuk menampilakan list informasi ruang penyimpanan disk yang format human (mudah dibaca)
```

#### file

Digunakan untuk menampilkan/ mengidentifikasi jenis file.

```bash
file [namafile]
```

#### stat

Perintah stat memberikan informasi seperti ukuran file, izin akses, ID user dan ID group, waktu akses, serta waktu lahir file. Perintah stat memiliki fitur lain yang juga dapat memberikan informasi filesystem. Perintah ini baik digunakan ketika kita menginginkan informasi dari file apa pun.

```bash
stat [namafile]
```

