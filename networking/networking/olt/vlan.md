---
icon: hubspot
---

# VLAN

Implementasi VLAN dalam jaringan GPON sedikit berbeda dengan jaringan Ethernet biasa (switch-to-switch). Di sini, VLAN berfungsi untuk memisahkan layanan (Internet, IPTV, VoIP) dalam satu jalur kabel fiber yang dipakai bersama.

Implementasi ini melibatkan pemetaan (_mapping_) dari sisi ONU/ONT hingga ke sisi OLT.

***

### 1. Implementasi VLAN di Sisi ONU (Customer Side)

ONU berfungsi sebagai jembatan antara perangkat pelanggan (PC, Router) dan jaringan fiber. Ada dua metode utama penanganan VLAN di sini:

* VLAN Tagging (Mode Access): ONU menerima paket data tanpa tag dari router pelanggan, lalu ONU memberikan Tag VLAN (misal: VLAN 100) sebelum mengirimkannya ke OLT.
* VLAN Transparent: ONU tidak melakukan perubahan apa pun pada tag VLAN. Biasanya digunakan jika pelanggan memiliki router sendiri yang sudah melakukan tagging.
* VLAN Translation: ONU mengubah Tag VLAN dari pelanggan menjadi Tag VLAN lain yang dikenali oleh ISP.

***

### 2. Pemetaan VLAN ke GEM Port

Ini adalah langkah krusial. Data yang sudah diberi label VLAN di ONU tidak bisa langsung dikirim begitu saja.

1. VLAN to GEM Port Mapping: Di dalam konfigurasi ONU, kita menentukan bahwa VLAN X harus masuk ke GEM Port Y.
2. Encapsulation: Frame Ethernet yang memiliki Tag VLAN dibungkus (_encapsulated_) ke dalam paket GEM.
3. Transmission: Paket GEM tersebut kemudian diletakkan di dalam T-CONT untuk dikirim ke OLT melalui jalur _upstream_.

***

### 3. Implementasi VLAN di Sisi OLT (Provider Side)

Di sisi OLT, terjadi proses sebaliknya dan pengarahan trafik ke jaringan inti (_Core Network_).

#### A. Sisi Service Port (Interface PON)

OLT menerima paket dari ONU, membuka bungkus GEM Port-nya, dan melihat Tag VLAN di dalamnya. OLT menggunakan Service Port untuk menghubungkan GEM Port tertentu dengan VLAN tertentu di sisi penyedia.

#### B. VLAN Switching/Forwarding

Ada tiga cara OLT menangani VLAN tersebut sebelum dibuang ke Uplink:

* VLAN Stacking (QinQ): OLT menambahkan tag VLAN baru (Outer Tag) di atas tag VLAN asli dari pelanggan (Inner Tag). Ini digunakan agar ISP bisa mengelola ribuan pelanggan dengan efisien.
* VLAN Translation: OLT mengubah VLAN pelanggan (misal VLAN 10) menjadi VLAN layanan (misal VLAN 1000).
* VLAN Aggregation: Menggabungkan beberapa VLAN dari ONU-ONU yang berbeda ke dalam satu VLAN yang sama untuk menghemat penggunaan VLAN di sisi Core.

***

### 4. Alur End-to-End (Contoh Layanan Internet)

Berikut adalah urutan teknis bagaimana VLAN bekerja dari rumah pelanggan ke internet:

1. Router Pelanggan: Mengirim data ke port LAN ONU.
2. ONU: Memberi tag VLAN 100 (Internet) ke data tersebut.
3. ONU Mapping: Memasukkan VLAN 100 ke GEM Port 11.
4. Uplink: GEM Port 11 dikirim melalui T-CONT 2 menuju OLT.
5. OLT (Service Port): Menerima data dari GEM Port 11, melihat ada VLAN 100.
6. OLT (Uplink): Meneruskan VLAN 100 (atau membungkusnya dengan QinQ) keluar melalui port Uplink (SFP 10G) menuju Router Core/BNG.

***

### Ringkasan Konfigurasi (Logika CLI)

Jika Anda melakukan konfigurasi (misal pada OLT Huawei), logikanya adalah:

1. Buat VLAN di OLT: `vlan 100 smart`
2. Tambahkan VLAN ke Uplink: `port vlan 100 0/19 0`
3. Buat Service Profile di ONU untuk menentukan bagaimana port LAN menangani VLAN.
4.  Buat Service Port di OLT:

    service-port vlan 100 gpon 0/1/0 ont 1 gemport 11 multi-service user-vlan 100
