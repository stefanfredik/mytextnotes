---
description: Berisi penjelesan singkat istilah dalam jaringan komputer
---

# Jaringan Komputer

## TTL

TTL (Time To Live) adalah sebuah nilai dalam paket IP yang menunjukkan batas waktu atau jumlah lompatan (hop) maksimum yang diizinkan sebelum paket tersebut dibuang. TTL digunakan untuk mencegah paket berputar-putar secara terus menerus di dalam jaringan yang mengalami masalah routing. Setiap kali sebuah paket melewati router, nilai TTL akan dikurangi satu. Jika nilai TTL mencapai nol, router akan membuang paket tersebut dan mengirim pesan ICMP "Time Exceeded" kepada pengirim, yang menunjukkan bahwa paket tidak bisa mencapai tujuan.

#### Detail TTL

TTL adalah bagian dari header paket IP, dan nilai awal TTL biasanya ditentukan oleh sistem operasi perangkat pengirim, misalnya:

* **Windows**: 128
* **Linux/Unix**: 64
* **Router**: 255

Ketika sebuah paket dikirim, TTL diisi dengan nilai awal yang besar, dan setiap router yang dilewati akan mengurangi nilai ini dengan 1. Jika TTL mencapai 0 sebelum sampai ke tujuannya, router membuang paket tersebut untuk mencegah jaringan mengalami flooding akibat adanya paket yang terus beredar tanpa henti.

#### Fungsi TTL:

1. **Mencegah Paket Berputar Tak Terbatas**: Jika ada masalah routing, TTL memastikan bahwa paket tidak akan terus beredar dalam jaringan.
2. **Mendeteksi Jarak (Jumlah Hop)**: TTL bisa digunakan untuk menghitung berapa banyak hop yang dilalui paket untuk mencapai tujuan. Tools seperti `traceroute` atau `tracert` memanfaatkan ini untuk menunjukkan jalur paket melalui jaringan.
3. **Mendeteksi Kegagalan Jaringan**: Jika ada masalah di rute, TTL dapat menunjukkan di mana paket dibuang, yang membantu dalam troubleshooting jaringan.

#### Contoh Penerapan TTL di Mikrotik

Mikrotik menyediakan beberapa cara untuk memanipulasi TTL pada jaringan, misalnya untuk melakukan optimasi atau sekadar pengaturan lalu lintas yang lebih baik.

1. **Mengubah TTL di Firewall NAT**\
   Mikrotik memungkinkan kita untuk mengubah nilai TTL melalui pengaturan di firewall, khususnya pada fitur NAT. Berikut adalah contoh untuk mengubah nilai TTL pada semua paket yang masuk melalui router:

```bash
/ip firewall mangle add chain=prerouting action=change-ttl new-ttl=set:64
```

Pada contoh di atas, kita mengatur agar TTL semua paket yang masuk diubah menjadi 64. Penggunaan chain `prerouting` menunjukkan bahwa aturan ini diterapkan pada paket yang akan diteruskan (forwarded) sebelum di-routing.

2. **Mengabaikan TTL untuk NAT** Ketika Anda menggunakan NAT (Network Address Translation), sering kali TTL akan berubah setelah melewati router, karena router dianggap sebagai hop. Pada Mikrotik, Anda bisa mengkonfigurasi router agar tidak mengurangi TTL dari paket yang melewatinya, misalnya dengan menambahkan rule firewall:

```bash
/ip firewall mangle add chain=postrouting action=change-ttl new-ttl=increment:1
```

Rule ini menambahkan 1 ke nilai TTL dari paket yang dikirimkan setelah routing dilakukan. Hal ini berguna jika Anda ingin membuat seolah-olah paket tidak melewati hop tambahan saat melewati NAT.

3. **Menerapkan TTL untuk Mencegah Pencurian Bandwidth** Penerapan TTL juga dapat digunakan untuk membatasi atau mencegah pencurian bandwidth pada jaringan wireless atau jaringan lokal. Misalnya, pengguna bisa meneruskan koneksi mereka ke perangkat lain tanpa izin (misalnya menggunakan tethering), yang dapat memengaruhi jaringan ISP. Untuk mencegah ini, TTL bisa diubah atau dipantau. Misalnya, ISP dapat menerapkan aturan agar perangkat yang menggunakan tethering akan memiliki TTL lebih kecil dan dikenali oleh router.

Anda bisa mengatur rule firewall untuk mendeteksi TTL dan menerapkan kebijakan tertentu:

```bash
/ip firewall filter add chain=forward protocol=tcp ttl=1 action=drop
```

Pada rule di atas, router Mikrotik akan membuang semua paket dengan TTL 1, yang biasanya menunjukkan bahwa paket tersebut berasal dari perangkat tethering.

#### Kesimpulan

TTL adalah komponen penting dalam protokol jaringan yang digunakan untuk mencegah loop paket dan memberikan pandangan terhadap kondisi routing di jaringan. Dalam Mikrotik, kita bisa memanipulasi TTL untuk berbagai tujuan seperti optimasi, troubleshooting, dan keamanan jaringan.
