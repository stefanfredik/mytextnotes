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



### Swith LTS to Normal Mode

Open File

```bash
sudo nano /etc/update-manager/release-upgrades

#change 
Prompt=lts

#to
Prompt=normal
```

Upgrade System

```
sudo do-release-upgrade -c
sudo do-release-upgrade
```

## Checking Your Login With Whoami

Di dalam linux, root adalah user yang sangat powerfull, dengan user root kita bisa menambahkan banyak user, ganti paswword, merubah hak akses, dan masih banyak lagi. Tentu anda tidak ingin ada user lain yang memiliki akses dengan kemampun user root. Sebagai seorang hacker, kita tentu ingin mendapatkan semua akses tersebut. Kita bisa menggunakan perintah whoami untuk mengetahu user yang ada saat ini.

```bash
whoami
```

## Navigasi System Linux

Untuk berpindah atau berganti direktori pada terminal linux kita dapat menggunakan perintah change directory atau cd

#### Pindah Folder/ Direktory

```bash
#Masuk ke folder tertentu
cd [namafolder]
```

### Berpindah ke folder sebelumnya

Untuk berpindah ke beberapa tingkat folder sebelumnya dapat menggunakan perintah berikut :&#x20;

```bash
#Kembali ke tingkat folder sebelumnya
cd ..

#Kembali ke 2 tingkat folder sebelumnya
cd .. ..

#Kembali ke 4 tingkat folder sebelumnya
cd .. ... ..
```

## Menampilkan list file atau folder menggunaakan ls

Dengan menggunakan perintah ls kita dapat menampilkan daftar/list file dan folder yang ingin kita tampilkan.

Berikut cara penggunaan perintah ls berseta opsinya

### Menampilkan folder dan file pada lokasi saat ini.

Kita dapat menggunakan perintah berikut untuk menampilkan daftar file dan folder pada lokasi kita saat ini.

```bash
ls
```

### Menampilkan folder dan File pada Folder Tertertu

Dari lokasi saat ini, kita juga bisa menampilkan file dan folder yang ada di dalam folder atau lokasi tertentu. Berikut adalah contoh perintah yang dapat digunakan.

```bash
#menampilkan listing pada satu folder yang ada di dalam lokasi saat ini.
ls [namafolder]

#menampilkan isi file dan folder pada lokasi tertentu

ls [lokasi/path]
#Example : 
ls /home/fred/documents/

```

### Opsi&#x20;

Kita dapat menggunakan perintah ls dengan berabagai opsi sesuai kebutuhan kita. Beberapa opsi yang digunakan dalam perintah ls adalah sebagai berikut :&#x20;

* **-a** : Menampilkan semua file dan folder, termasuk yang tersembunyi. Contoh: `ls -a`
* **-l** : Menampilkan file dan folder dengan format panjang, termasuk izin, jumlah link, pemilik, grup, ukuran, dan tanggal modifikasi. Contoh: `ls -l`
* **-h** : Saat digunakan dengan opsi -l, menampilkan ukuran file dalam format yang lebih mudah dibaca manusia (misalnya, 1K, 234M, 2G). Contoh: `ls -lh`
* **-r** : Membalik urutan hasil, yang berguna saat ingin melihat file terbaru atau terlama terlebih dahulu. Contoh: `ls -lr`
* **-t** : Mengurutkan file dan folder berdasarkan waktu modifikasi terakhir, dengan file yang paling baru muncul pertama kali. Contoh: `ls -lt`

## Mempelajari perintah menggunakan manual page

Di dalam linux kita bisa mempelajari manual atau cara menggunakan perintah beserta opsinya menggunakan perintah man. Dengan man kita bisa melihat penjelasan, fungsi dan cara penggunaan perintah yang ingin kita gunakan. Berikut contoh menggunakan man.

```bash
man [namaperintah]

#Example : menampilkan manual pages dari ls
man ls
```

## Mencari lokasi sebuah perintah

Di dalam linux kita kadang pusing saat ingin menemukan dimana letak perintah atau aplikasi atau file system yang ingin kita gunakan. Untuk menemukannya dengan mudah kita bisa menggunakan perintah find.

### Mencari berdasarkan lokasi

Perintah yang paling mudah kita gunakan adalah perintah locate. Kita dapat menggunakan perintah locate untuk menemukan lokasi dari file/aplikasi yang ingin kita cari di dalam keseluruhan system.

```bash
locate [katakunci]

#Example : 
locate aircrack-ng
```

Perintah tersebut kadang memberikan terlalu banyak informasi yang berlebihan dan tidak kita butuhkan, karena perintah locate akan menemukan file dan folder apapun yang memiliki kata kunci sama sesai yang kita cari.

### Mencari Binary dengan menggunakan whereis

Kita bisa mencari file bynari menggunakan perintah whereis. Binary file adalah file binary yang bisa dieksekusi atau dengan kata lain adalah perintah/aplikasi dari linux itu sendiri.

Untuk mencari lokasi dari file biner suatu program, Anda bisa menggunakan perintah `whereis`. Ini sangat berguna untuk mengetahui di mana program tersebut diinstal di sistem Linux Anda. Berikut adalah format umum dari penggunaan perintah ini:

```bash
whereis [nama_program]
# Contoh:
whereis nginx
```

Perintah ini akan mengembalikan path lokasi dari binary, source, dan man page (jika ada) dari program yang Anda cari. Ini jauh lebih spesifik dibanding menggunakan perintah `locate` karena `whereis` dikhususkan untuk mencari binary files, source files, dan manual pages saja.

### Mencari Binary berdasarkan lokasi/path menggunakan perintah which

Setelah memahami cara menggunakan perintah `whereis` untuk menemukan file biner, source, dan man pages, kita dapat mempelajari cara mencari binary berdasarkan lokasi/path menggunakan perintah `which`. Perintah `which` digunakan untuk menemukan lokasi eksekusi dari perintah yang kita gunakan dalam shell. Ini hanya mengembalikan lokasi dari binary file saja.

Format penggunaan perintah ini cukup sederhana:

```bash
which [nama_program]
# Contoh:
which nginx
```

Perintah ini akan mengembalikan path lokasi binary dari program yang Anda cari. Sangat berguna ketika Anda ingin mengetahui versi executable mana yang dipanggil saat Anda menjalankan suatu program dari terminal.

Jika Anda menjalankan beberapa versi dari program yang sama, perintah `which` akan menunjukkan path dari versi yang akan dipanggil pertama kali berdasarkan `$PATH` environment variable Anda. Ini membantu dalam memastikan bahwa Anda menggunakan versi program yang tepat.



## Mencari File menggunakan Find

Perintah find adalah perintah utilitas yang sangat powerful dan flexible. Kita bisa menemukan apa saja dengan find menggunakan berbagai parameter yang berbeda seperti menggunakan nama file, tanggal atau waktu pembuatan atau modifikasi file, user owner, group user, permission, dan ukuran.

Berikut adalah perintah dasar menggunkan find :&#x20;

```bash
find [namadirektory] option expression
```

Sebagai contoh saya ingin mencari nama apache2 dengan titik pencarian awal adalah pada folder root. Maka perintah yang bisa digunakan adalah sebagai berikut :&#x20;



