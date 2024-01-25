---
description: >-
  Berisi catatan-catatan yang biasa digunakan untuk kongfigurasi perangkat
  mikrotik.
---

# 🎛 Mikrotik



## Vlan Bridge

Ketika sudah di routing, tagged vlan akan tetap tembus walapun beda networkg

### Untagged / Access Vlan

Dalam mikrotik terdapat beberapa cara untuk untagged vlan pada interface tertentu.&#x20;

#### Menggunakan  Bridge Port

Salah satu caranya adalah menggunakan bridge port. Ketika menggunakan bridge maka interface vlan dan ether yang akan di unttaged akan dimasukan ke dalam bridge yang sama.

* Buat vlan baru dan tambahkan pada interface masuk/upstream
* Buat bridge baru
* Tambahkan port interface vlan dan interface yang mau di unttaged atau dikasih access pada interface Bridge yang baru dibuat.

#### Menggunakan  Bridge Filtering

Menggunakan bridge filtering, tidak perlu dilakukan pembuatan interface vlan pada mikrotik. Vlan id akan langsung di tag langsung pada port interface pada bridge. Pada jenis kongfigurasi in, akan di aktifkan ingres filtering pada bridge. Ketika mengaktifkan ingres filtering maka secara otomatis mikrotik tidak dapat diakses kembali. Oleh sebab itu akan dibuatkan sebuah vlan untuk management pada bridge interface yang digunakan.

Berikut cara membuat uttaged/access vlan menggunakan bridge filtering&#x20;

*   Skenario

    Terdapat 2 buah vlan. Ya itu vlan 100 sebagai vlan service hotpsot dan vlan 90 sebagai vlan management. Eth 1 sebagai trunk interface dan eth2 sebagai access port.
* Buat sebuah interface bridge baru dengan nama BR-Lokal
* Tambahkan interface eth1 sebagai trunk dan interface 2 sebagai access port vlan
*   Pada bridge port eth2&#x20;

    Vlan :&#x20;

    Pvid : 100

    Frame Types : admit only unttaged and priority tagged
* Pada setiap bridge interface port hilangkan hardware load. Ini berfungsi agar akses bridge tidak diambil ahli oleh switch chip.
*   Masuk ke tab VLAN lalu setting sesuai ketentuan berikut :&#x20;

    Tambah Bridge Vlan&#x20;

    Bridge : BR-Hotspot

    Vlan ID : 100

    Tagged : eth1

    Untagged : eth2
*   Kemudian tambah Bridge Vlan management dengan ketentuan sebagai berikut :&#x20;

    Bridge : BR-Hotpsot

    Vlan id : 90

    Tagged : eth1, BR-Hotspot

    Uttaged : -

    Untuk uttaged port biarkan kosong.
* Buat vlan managemet vlan 90 dan tambahkan pada interface BR-Hotspot.
* Kemudian masuk ke interface BR-Hotspot. Centang atau aktifkan Vlan Filtering. Langkah ini harus dilakukan terkahir kali. Karena ketika mengaktifkan vlan filtering maka router tidak akan bisa diakses lagi. Oleh karena itu wajib membuat vlan management baru mengaktifkan vlan filtering pada interface BR-Hotspot.
*

## Wireless

### Mode Akses Point

