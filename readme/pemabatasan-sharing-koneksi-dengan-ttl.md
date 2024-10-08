---
description: >-
  Dengan cara berikut kita bisa membatasi sharing koneksi. Sehingga pelanggan
  tidak dapat sharing kembali jaringan kita.
---

# Pemabatasan Sharing Koneksi dengan TTL

Untuk melakukan pembatasan sharing koneksi dalam hal ini dilakukan menggunakan  firewall mangle menggunakan teknik pembatasan TTL, untuk membatasi pelanggan agar menyebarkan atau menjual kembali koneksi dari kita.

## Skenario

Pada studi kasus kali ini, kita akan menerapkan pada koneksi ROS pelanggan dengan kriteria-kriteria atau skenario berikut :&#x20;

* **`Eth 1`** : Input Interface
  * Adalah interface yang menuju internet/ uplink
* **`Wlan1`** : Output interface&#x20;
  * Adalah interface yang menuju client atau perangkat pelanggan.
  * Koneksi pelanggan akan keluar melalui Wlan1
* Kita akan membatasi sharing koneksi dengan mengubah atau membatasi jumlah TTL pada interface Wlan1 karena interface tersebut yang menuju client.
* Jumlah TTL yang keluar dari interface WLAN 1 akan di set menjadi nilai 1 sehingga ketika pelanggan menambahkan router, semua client yang terhubung ke router tersebut tidak dapat terhubung ke internet.

## Config

* Masuk ke IP - Firewall - Mangle
*   Tambah mangle baru dengan setingan sebagai berikut :&#x20;

    * Chain : postrouting
    * Out Interface : Wlan1
    * Action : Change TTL
    * TTL Action : Change
    * New TTL : 1
    * Pastrough : no



## Script

Untuk mempermudah proses setingan kita dapat menggunakan script berikut :&#x20;

{% code overflow="wrap" %}
```bash
/ip firewall mangle add chain=postrouting out-interface=wlan1 action=change-ttl new-ttl=set:1 passthrough=no 
```
{% endcode %}
