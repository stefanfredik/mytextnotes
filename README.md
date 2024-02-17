---
description: Berisi tutorial, tips dan kongfigurasi Docker.
---

# Docker

## Pengenalan Container

Docker adalah sebuah container manager yang populer saat ini. Container merupakan virtualisasi yang membungkus aplikasi untuk berjalan di atas sebuah system operasi namun aplikasi tersebut tidak terintegrasi atau terkait langsung dengan system operasi utama.&#x20;

Container berbeda dengan Virtual Mesin. Virtual machine menjalakan beberapa system operasi dan aplikasi berjalan didalamnya. Sedangkan Container menjalan hanya satu system operasi dan aplikasi berjalan diatas satu system operasi tersebut namun secara terpisah, seolah-olah aplikasi berjalan di system operasi yang berbeda.

Aplikasi yang terinstal di docker  disebut images.

Berikut beberapa istilah pada docker :&#x20;

### Virtual Machine (VM)

* Dalam dunia infrastruktur sudah tidak asing lagi dengan Vitrual Machine
* VM Berjalan di atas sebuah system operasi, dan di dalam VM berjalan system operasi pula.
* Masalah  yang sering muncul ketika menggunakan VM adalah proses yang lama ketika membuat VM baru dan membutuhkan waktu yang cukup lama ketika merestart VM.&#x20;

#### Diagram VM

![](https://lh7-us.googleusercontent.com/H32Tj24jS7tKwSa0XSslFI2sWFT\_EKOm4s-VWctd-MiWk0j1rvEw3tRihmyX\_XhjSTMG5ZbqcvTXicw7Pkh98-N6PYUNou0jopv9YBPElYtIhTEuJlbggCjwqmzqBb3AsG9eh0ugE9Fg3b6GyvGVydXXhA=s2048)

### Container

* Berbeda dengan VM, Container sendiri berjalan di sisi aplikasi.
* Container sendiri sebenarnya berjalan di atas aplikasi container manager pada system operasi utama.
* Yang berbeda dengan VM adalah kita bisa mempackage aplikasi berserta depencinya tanpa harus menggabungkanya dari aplikasi. Aplikasi yang terinstal terpisah dari system operasi atau berdiri sendiri.
* Container bisasa menggunakan system operasi hots atau system operasi utamanya dimana container managernya berjalan. Oleh karena itu container akan lebih hemat resource dan lebih cepat jalanya karena tidak butuh system operasi sendiri.&#x20;
* Ukuran container biasanya lebih kecil dan biasnya hanya kisaran berapa MB, berbeda dengan VM yang ukuranya cukup besar sampai bergiga giga karena ada system operasi di dalam nya.

#### Diagram Container

![](https://lh7-us.googleusercontent.com/jnaf6IuPJ5e\_VNcZO9bRpNM1lWf81eKta3noSTMb1M0CN0AzJ\_AbQroQTB2qHKvGjfWti372IzBRdsfrkwtYe4N639aS5eXupsdIms-KOFCusW4rhCLyVPHx0wLXf79oGBZ42KYCfJNRF2iBqRlIPto8cg=s2048)

## Docker Images

Mirip seperti installer apliakasi. Di dalam docker image terdapat aplikasi, depedency dan congfigrasi terkait aplikasi tersebut. Sebelum kita menjalankan aplikasi docker kita harus memastikan bahwa image sudah ada dalam ocker agar kita bisa menginstal apliasi tersebut .  Docker image bisa kita download di https://hub.docker.com



## Docker Container

Jika sebuah image merupakan installer aplikasi, maka docker container adalah aplikasi yang sudah terinstal. Satu docker image dapat membuat beberapa docker container asalkan namanya berbeda. Ketika docker container dibuat makan docker image tidak dapat dihapus lagi karena docker container tidak mengcopy docker image namun menggunakan isinya saja.

### Status Container

Sama seperti aplikasi,jika kita tidak membuka atau menjalankanya maka aplikasi tersebut pun tidak berjalan sama seperti container ketika container sudah dibuat maka aplikasi tidak secara otomatis berjalan. Kita perlu menjalankan container terlebih dahulu agar apliasi bisa berjalan.

### Melihat Container

Kita dapat melihat semua container yang sudah terpasang/terbuat. Melihat container yang aktif maupun tidak. Bisa menggunakan perintah berikut :&#x20;

#### Melihat Semua Container

```bash
docker container ls -a
```

#### Melihat Container yang sedang berjalan saja

```bash
docker container ls
```



#### Docker Registry

Merupakan tempat penyimpanan image. Dengan menggunakan docker registry kita bisa menyimpan docker image yang teah kita buat dan dapat digunakan kembali secara berulanng selama kita terhubung dengan registry.









## Perintah

### Check Images

Mengecek images  yang sudah terinstall pada docker.

```bash
docker images all
```

### Pull/ Download Docker Image

Docker image bisa kita download dari http://hub.docker.com



```bash
docker pull [nama images]:[tag]

#ex : 
docker pull nginx:latest
```

Beberapa tag yang digunakan saat mendownload docker image

`latest` untuk mendowload versi terbaru

`[major].[minor].[patch]` atau versi :  digunakan untuk mendowload versi tertentu. Ex : 2.1.3

`stable`  : untuk mendownload versi stabil yang terbaru

`beta` : untuk mendownload versi beta yang terbaru

`rc`  : untuk mendowload versi relaease candidate yang terbaru.

### Hapus Docker Image

Jika tindak ingin menggunakan image yang sudah ada, bisa kita hapus menggunakan perintah berikut :&#x20;

```bash
docker image rm [nama image]:[tag]

#ex
docker image rm nginx:latest
```
