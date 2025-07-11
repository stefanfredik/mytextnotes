# Mangle

## 📘 **Penjelasan Lengkap Menu Mangle MikroTik**

Mangle terletak di:

```
/ip firewall mangle
```

Masing-masing rule terdiri dari beberapa parameter yang menentukan **kapan dan bagaimana paket akan ditandai**. Mari kita bahas satu per satu:

***

### 🔹 1. **Chain**

Menentukan **alur pemrosesan** paket:

| Nama Chain    | Kapan digunakan                                                         |
| ------------- | ----------------------------------------------------------------------- |
| `prerouting`  | Sebelum routing diproses (paling umum untuk PBR dan marking)            |
| `input`       | Paket yang ditujukan ke router itu sendiri                              |
| `forward`     | Paket yang lewat router (sumber ≠ router, tujuan ≠ router)              |
| `output`      | Paket yang keluar dari router itu sendiri (misal DNS query dari router) |
| `postrouting` | Setelah routing diproses, sebelum keluar dari router                    |

> 🔧 Paling umum: gunakan `prerouting` untuk **PBR, marking, load balancing**.

***

### 🔹 2. **Src. Address & Dst. Address**

Menyaring berdasarkan alamat IP sumber dan tujuan.

Contoh:

```bash
src-address=192.168.88.10
dst-address=8.8.8.8
```

Bisa juga menggunakan CIDR: `192.168.88.0/24`

***

### 🔹 3. **Src. Address List & Dst. Address List**

Gunakan **address-list** yang sudah dibuat sebelumnya, cocok untuk banyak IP:

```bash
src-address-list=pelanggan_pppoe
```

***

### 🔹 4. **Protocol**

Pilih protokol: TCP, UDP, ICMP, GRE, dll.

Contoh:

```bash
protocol=tcp
```

***

### 🔹 5. **Src. Port & Dst. Port**

Digunakan bersamaan dengan `protocol` untuk membatasi ke port tertentu.

Contoh:

```bash
protocol=tcp dst-port=80
```

***

### 🔹 6. **In Interface & Out Interface**

Untuk menyaring paket berdasarkan **interface asal atau tujuan**.

| Parameter       | Fungsi                            |
| --------------- | --------------------------------- |
| `in-interface`  | Interface **tempat paket masuk**  |
| `out-interface` | Interface **tempat paket keluar** |

Contoh:

```bash
in-interface=ether2
out-interface=ether1
```

***

### 🔹 7. **Connection State**

Filter berdasarkan status koneksi (connection tracking).

| State         | Penjelasan                          |
| ------------- | ----------------------------------- |
| `new`         | Koneksi baru                        |
| `established` | Koneksi yang sudah ada              |
| `related`     | Koneksi tambahan dari koneksi utama |
| `invalid`     | Tidak valid                         |

Contoh:

```bash
connection-state=new
```

***

### 🔹 8. **Dst. Address Type**

Filter berdasarkan tipe alamat tujuan.

| Tipe        | Penjelasan                                     |
| ----------- | ---------------------------------------------- |
| `local`     | Alamat milik router (IP di interface MikroTik) |
| `broadcast` | Broadcast (misal 192.168.88.255)               |
| `multicast` | Multicast                                      |
| `unicast`   | Biasa (non-broadcast)                          |

Contoh untuk mengecualikan LAN:

```bash
dst-address-type=!local
```

***

### 🔹 9. **Action**

🧠 **Inilah bagian terpenting: apa yang dilakukan saat rule cocok.**

| Action                       | Fungsi                                         |
| ---------------------------- | ---------------------------------------------- |
| `mark-connection`            | Tandai koneksi                                 |
| `mark-packet`                | Tandai paket                                   |
| `mark-routing`               | Tandai untuk diarahkan ke route khusus         |
| `accept`                     | Lolos tanpa di-mark (jarang dipakai di mangle) |
| `add-src-to-address-list`    | Tambahkan ke address list                      |
| `add-dst-to-address-list`    | Tambahkan tujuan ke address list               |
| `change-ttl`, `set-priority` | Modifikasi TTL atau prioritas                  |
| `log`                        | Catat log rule ini                             |
| `jump`                       | Lompat ke chain khusus (custom chain)          |

Contoh:

```bash
action=mark-routing new-routing-mark=to_gw2
```

***

### 🔹 10. _New- Fields (Marking)_\*

Jika `action` adalah `mark-*`, maka kamu akan melihat:

| Field                      | Kegunaan                                |
| -------------------------- | --------------------------------------- |
| `new-connection-mark`      | Nama penanda koneksi                    |
| `new-packet-mark`          | Nama penanda paket                      |
| `new-routing-mark`         | Nama penanda routing table              |
| `new-dscp`, `new-priority` | Untuk DSCP dan TOS marking (QoS lanjut) |

***

### 🔹 11. **Passthrough**

| Nilai | Fungsi                                                 |
| ----- | ------------------------------------------------------ |
| `yes` | Rule berikutnya **tetap dijalankan** setelah rule ini  |
| `no`  | Rule ini selesai, rule berikutnya **tidak dijalankan** |

Biasanya diset `yes`, kecuali kalau kamu ingin rule ini menjadi akhir proses.

***

### 🔹 12. **Comment**

Berfungsi sebagai catatan admin:

```bash
comment="Mark routing untuk client Rudi"
```

***

### 🔹 13. **Log**

Jika diaktifkan (`action=log`), rule akan mencatat ke:

```bash
/log print
```

Gunakan untuk debug/testing trafik tertentu.

***

## 📊 Contoh Rule Lengkap

```bash
/ip firewall mangle
add chain=prerouting src-address=192.168.88.10 dst-address-type=!local \
    action=mark-routing new-routing-mark=ke_gw_b passthrough=yes \
    comment="PBR client 192.168.88.10 ke gateway B"
```

***

### 🎓 Tips Praktis:

| Tujuan           | Rekomendasi Chain                                |
| ---------------- | ------------------------------------------------ |
| Routing (PBR)    | `prerouting` + `mark-routing`                    |
| Queue Tree (QoS) | `forward` + `mark-connection` + `mark-packet`    |
| Load Balancing   | `prerouting` + `mark-connection`                 |
| Logging trafik   | `input`, `forward`, atau `output` + `action=log` |

***

### 🔐 Keamanan

> Hindari menandai seluruh trafik tanpa pengecualian.\
> Selalu tambahkan filter `dst-address-type=!local` atau `dst-address=!192.168.0.0/16`\
> untuk **menghindari terputusnya akses LAN** atau **asymmetric routing**.

***

Kalau kamu mau, saya bisa bantu **buat skrip otomatis**:

* Mangle + Route + NAT lengkap
* Template untuk Load Balancing, Failover, Multi-Gateway
* Monitoring trafik berdasarkan packet-mark

Ingin lanjut ke salah satu skenario praktis dengan mangle?



Tentu, Fred. Berikut adalah **penjelasan lengkap, terstruktur, dan profesional** mengenai **MikroTik Mangle** — salah satu fitur paling powerful namun sering disalahpahami.

***

## 📘 **Penjelasan Lengkap MikroTik Mangle**

***

### 🔷 Apa itu Mangle?

**Mangle** adalah fitur di MikroTik (pada firewall) yang digunakan untuk **menandai (mark)** paket data atau koneksi, tanpa langsung mengubah atau memblokirnya.

Mangle tidak melakukan aksi langsung seperti drop atau accept. Sebaliknya, mangle memberi **tanda (mark)** pada paket agar dapat diproses lebih lanjut oleh fitur lain seperti:

* **Routing (routing-mark)**
* **QoS / Bandwidth control (packet-mark / connection-mark)**
* **NAT (address-list / connection-type)**

***

### 🎯 Tujuan Penggunaan Mangle

| Tujuan Umum                | Penjelasan                                                 |
| -------------------------- | ---------------------------------------------------------- |
| Policy-Based Routing (PBR) | Menandai trafik agar diarahkan ke gateway tertentu         |
| QoS / Traffic Shaping      | Menandai jenis trafik untuk diatur bandwidth-nya           |
| Load Balancing             | Menandai koneksi untuk dibagi ke beberapa gateway          |
| Monitoring / Logging       | Untuk identifikasi jenis trafik tertentu                   |
| Blocking tertentu          | Digunakan bersama fitur filter untuk tindakan lebih lanjut |

***

### 🛠️ Komponen Penandaan (Mark Types)

| Jenis Mark              | Fungsi                                              |
| ----------------------- | --------------------------------------------------- |
| **Connection Mark**     | Menandai satu sesi koneksi (TCP/UDP session)        |
| **Routing Mark**        | Menentukan route (via gateway tertentu)             |
| **Packet Mark**         | Menandai setiap paket data (biasanya untuk queue)   |
| **Routing Table (v7+)** | Digunakan dalam `/routing rule`, bukan mangle lagi  |
| **TTL / DSCP Marking**  | Untuk pengaturan lanjutan di jaringan L3 (opsional) |

***

### 📦 Chain pada Mangle

| Chain           | Waktu pemrosesan                                                                       |
| --------------- | -------------------------------------------------------------------------------------- |
| **prerouting**  | Saat paket baru masuk ke router (paling umum digunakan untuk routing dan marking awal) |
| **input**       | Paket yang ditujukan ke router itu sendiri                                             |
| **forward**     | Paket yang hanya melewati router, dari 1 interface ke lainnya                          |
| **output**      | Paket yang berasal dari router itu sendiri                                             |
| **postrouting** | Setelah routing diputuskan, sebelum paket keluar                                       |

***

### 🔍 Struktur Dasar Rule Mangle

```bash
/ip firewall mangle
add chain=prerouting \
    src-address=192.168.88.10 \
    dst-address=!192.168.88.0/24 \
    action=mark-routing new-routing-mark=via-gw2 passthrough=yes
```

| Parameter          | Penjelasan                                 |
| ------------------ | ------------------------------------------ |
| `chain`            | Jalur pemrosesan paket                     |
| `src-address`      | IP sumber                                  |
| `dst-address`      | IP tujuan                                  |
| `action`           | Jenis tanda yang diberikan                 |
| `new-routing-mark` | Nama penanda yang digunakan di `/ip route` |
| `passthrough=yes`  | Biarkan rule selanjutnya tetap berjalan    |

***

### 🧠 Penting: `passthrough=yes` atau `no`?

| Nilai | Efek                                                                         |
| ----- | ---------------------------------------------------------------------------- |
| `yes` | Paket akan tetap lanjut ke rule mangle selanjutnya                           |
| `no`  | Paket berhenti di rule ini (tidak diproses oleh rule mangle lain setelahnya) |

Gunakan `yes` kalau kamu ingin memberi lebih dari satu tanda pada paket tersebut (misal: connection-mark → routing-mark → packet-mark).

***

### 🔁 Hubungan Mangle dengan Routing

Untuk Policy-Based Routing (PBR):

1. Paket masuk ke router (chain `prerouting`)
2. Di-mangle → diberikan `routing-mark`
3. Di `/ip route`, kamu tentukan route untuk `routing-mark` tersebut

Contoh:

```bash
/ip firewall mangle
add chain=prerouting src-address=192.168.88.10 action=mark-routing new-routing-mark=via-gw-b

/ip route
add dst-address=0.0.0.0/0 gateway=192.168.100.2 routing-mark=via-gw-b
```

***

### 🎓 Studi Kasus Penggunaan Mangle

#### 🔸 1. Mengarahkan IP Client ke Gateway tertentu

```bash
/ip firewall mangle
add chain=prerouting src-address=192.168.88.10 \
    dst-address-type=!local \
    action=mark-routing new-routing-mark=ke_GW_B
```

#### 🔸 2. Tandai koneksi untuk Load Balancing

```bash
/ip firewall mangle
add chain=prerouting connection-state=new \
    in-interface=ether2 action=mark-connection \
    new-connection-mark=to_ISP1 passthrough=yes
```

#### 🔸 3. Tandai paket untuk Queue Tree

```bash
/ip firewall mangle
add chain=forward connection-mark=to_ISP1 action=mark-packet \
    new-packet-mark=paket_ISP1 passthrough=no
```

***

### ⚠️ Hal yang Harus Diperhatikan

| Masalah Umum                  | Solusi                                                               |
| ----------------------------- | -------------------------------------------------------------------- |
| **Asymmetric Routing**        | Jangan tandai trafik ke internal network                             |
| **LAN tidak bisa komunikasi** | Tambahkan pengecualian `dst-address=!192.168.0.0/16`                 |
| **Tidak ada efek**            | Pastikan route menggunakan `routing-mark` yang cocok                 |
| **Routing tidak berjalan**    | Mangle di chain yang salah, atau mark tidak digunakan di `/ip route` |

***

### 🧪 Tools Debugging

*   **Torch**:

    ```bash
    /tool torch interface=ether2
    ```
*   **Connection Tracking**:

    ```bash
    /ip firewall connection print
    ```
* **Log Mangle Rule**:\
  Tambahkan `action=log` sebelum testing.

***

### 🧠 Kesimpulan

**Mangle adalah alat bantu untuk memberi tanda** pada paket atau koneksi, agar:

* Routing bisa diarahkan lebih spesifik
* Bandwidth bisa dikontrol lebih presisi
* Trafik bisa dipisahkan sesuai logika yang kita atur

Tapi perlu **perhatian besar**, karena konfigurasi mangle yang salah bisa:

* Memutus komunikasi LAN
* Menyebabkan internet lambat atau tidak jalan
* Mengacaukan koneksi NAT / reply traffic

***

### 📌 Rekomendasi Praktek

* **Selalu gunakan pengecualian untuk trafik internal**
* Gunakan `log` untuk uji rule mangle
* Kombinasikan mangle + routing + NAT dengan urutan benar

***

Kalau kamu mau, saya bisa bantu buatkan **playbook Mangle MikroTik** berbasis skenario:

* Multi-Gateway
* Per VLAN / IP Public
* Mark + Queue Tree
* Mark + Firewall / DPI
* Load balancing + failover

Apakah kamu ingin lanjut ke pembuatan skenario lanjutan itu?
