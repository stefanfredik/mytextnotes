# Vlan

**VLAN** (Virtual Local Area Network) adalah teknologi jaringan yang digunakan untuk membuat **segregasi atau pemisahan logis** di dalam sebuah jaringan fisik. Dengan VLAN, perangkat di jaringan yang sama secara fisik dapat dipisahkan menjadi beberapa jaringan logis, sehingga mereka tidak dapat saling berkomunikasi langsung, kecuali melalui mekanisme tertentu seperti routing.

#### Konsep Dasar VLAN

1. **Segregasi Logis**: VLAN memungkinkan pembagian jaringan besar menjadi beberapa jaringan lebih kecil tanpa harus menambah perangkat fisik, seperti switch atau router. Setiap VLAN berfungsi seolah-olah menjadi jaringan terpisah meskipun perangkatnya menggunakan switch fisik yang sama.
2. **Identifikasi VLAN**: Setiap VLAN diidentifikasi oleh sebuah **VLAN ID**, yang merupakan angka dari 1 hingga 4094. Ini memungkinkan pengelolaan beberapa jaringan terpisah pada satu infrastruktur fisik.
3. **Tagging VLAN**: VLAN bekerja dengan menambahkan **tag** (disebut VLAN tag) pada paket data yang lewat di jaringan. Tag ini menandakan VLAN mana yang terkait dengan paket tersebut. Proses tagging ini mengikuti standar **IEEE 802.1Q**, yang merupakan protokol untuk mengatur VLAN dalam jaringan Ethernet.

#### Manfaat VLAN

1. **Keamanan**: Dengan memisahkan jaringan secara logis, VLAN membantu mencegah akses tidak sah antar bagian dari jaringan. Misalnya, Anda dapat membuat VLAN terpisah untuk departemen HR dan departemen IT, sehingga mereka tidak dapat saling mengakses data secara langsung.
2. **Efisiensi Jaringan**: VLAN membantu meminimalkan broadcast domain, yaitu area di mana paket broadcast disebarkan. Dengan memecah jaringan menjadi VLAN yang lebih kecil, Anda dapat mengurangi jumlah broadcast dan meningkatkan kinerja jaringan.
3. **Manajemen Lalu Lintas**: VLAN memungkinkan pengelolaan yang lebih baik atas lalu lintas jaringan. Anda dapat memberikan prioritas yang berbeda pada VLAN tertentu, misalnya dengan menerapkan **Quality of Service (QoS)** untuk membatasi atau memprioritaskan bandwidth pada VLAN yang berbeda.
4. **Fleksibilitas**: VLAN memungkinkan organisasi untuk mengelompokkan perangkat berdasarkan fungsi, bukan lokasi fisik. Misalnya, perangkat yang terletak di gedung berbeda tetapi memiliki fungsi yang sama dapat ditempatkan dalam VLAN yang sama.

#### Jenis VLAN

1. **Default VLAN**: VLAN ini sering kali digunakan oleh perangkat switch untuk semua port yang belum secara eksplisit ditugaskan ke VLAN tertentu. Misalnya, VLAN 1 adalah default di sebagian besar switch.
2. **Data VLAN**: VLAN yang mengatur lalu lintas data pengguna. Ini adalah VLAN standar yang dipakai untuk memisahkan perangkat berdasarkan kelompok kerja atau departemen.
3. **Voice VLAN**: VLAN yang diprioritaskan untuk lalu lintas suara (VoIP). Biasanya diberikan prioritas lebih tinggi untuk menjamin kualitas layanan (Quality of Service).
4. **Management VLAN**: VLAN yang digunakan untuk mengelola switch atau perangkat jaringan lainnya. VLAN ini sering diisolasi untuk menjaga keamanan pengelolaan perangkat.
5. **Native VLAN**: VLAN yang digunakan untuk menangani traffic yang tidak bertagging pada trunk link. Default-nya adalah VLAN 1, tapi bisa diubah untuk meningkatkan keamanan.

#### Mode VLAN

1. **Access Port**: Port yang hanya dapat terhubung ke satu VLAN. Biasanya digunakan untuk menghubungkan perangkat seperti komputer atau printer ke jaringan.
2. **Trunk Port**: Port yang bisa membawa traffic dari beberapa VLAN sekaligus. Trunk port digunakan untuk menghubungkan switch ke switch lain atau ke router, sehingga traffic dari berbagai VLAN dapat dibawa dalam satu koneksi fisik.

#### Tagging dan Untagging VLAN

* **Tagged VLAN**: Paket yang melewati trunk port akan diberi tag VLAN ID, sehingga perangkat di ujung lain dapat mengetahui VLAN asal paket tersebut. Ini diterapkan pada trunk port yang membawa beberapa VLAN.
* **Untagged VLAN**: Paket yang tidak diberi tag, biasanya pada access port. Access port hanya membawa traffic dari satu VLAN, sehingga tidak perlu ada tag pada paketnya.

#### Proses Kerja VLAN

1. **VLAN Access Port**:
   * Perangkat di VLAN tertentu terhubung ke switch melalui **access port**.
   * Paket yang keluar atau masuk melalui access port tidak memiliki tag VLAN, karena port ini hanya menangani satu VLAN.
2. **VLAN Trunk Port**:
   * Jika Anda menghubungkan dua switch melalui trunk port, maka trunk port dapat membawa paket dari berbagai VLAN. Paket-paket tersebut diberi tag VLAN (802.1Q) oleh switch sebelum dikirimkan melalui trunk.
   * Switch penerima akan membaca tag VLAN dan meneruskannya ke VLAN yang sesuai.
3. **Inter-VLAN Routing**:
   * Untuk memungkinkan komunikasi antar VLAN, dibutuhkan **router** atau perangkat Layer 3 seperti **Layer 3 switch**. Proses ini disebut **Inter-VLAN Routing**. Router akan meneruskan paket dari satu VLAN ke VLAN lain berdasarkan tabel routing.

#### Contoh Penerapan VLAN di Mikrotik

Pada Mikrotik, Anda bisa dengan mudah mengkonfigurasi VLAN menggunakan perintah di CLI atau menggunakan **Winbox**. Misalkan, Anda ingin membuat dua VLAN: VLAN 10 untuk departemen IT dan VLAN 20 untuk departemen Finance.

**1. Menambahkan VLAN pada Interface**

Misalkan Anda memiliki interface fisik `ether1`, dan ingin menambahkan dua VLAN (10 dan 20):

```bash
/interface vlan
add name=vlan10 interface=ether1 vlan-id=10
add name=vlan20 interface=ether1 vlan-id=20
```

**2. Menambahkan IP Address untuk Masing-Masing VLAN**

Untuk memberikan IP address pada masing-masing VLAN:

```bash
/ip address
add address=192.168.10.1/24 interface=vlan10
add address=192.168.20.1/24 interface=vlan20
```

**3. Mengonfigurasi Trunk Port**

Misalnya, Anda ingin `ether2` menjadi trunk port yang membawa traffic dari VLAN 10 dan 20:

```bash
/interface bridge
add name=bridge1
/interface bridge port
add bridge=bridge1 interface=ether2
/interface vlan
add interface=bridge1 name=vlan10-trunk vlan-id=10
add interface=bridge1 name=vlan20-trunk vlan-id=20
```

**4. Mengonfigurasi Access Port**

Misalnya, Anda ingin `ether3` menjadi access port untuk VLAN 10:

```bash
/interface bridge port
add bridge=bridge1 interface=ether3 pvid=10
```

#### Kesimpulan

VLAN adalah solusi efisien untuk memisahkan jaringan secara logis dalam satu infrastruktur fisik. Dengan VLAN, organisasi dapat mengoptimalkan kinerja jaringan, meningkatkan keamanan, dan mengelola lalu lintas dengan lebih baik. Di Mikrotik, konfigurasi VLAN dapat dilakukan dengan mudah menggunakan interface dan command line, memungkinkan administrator jaringan untuk mengelola lalu lintas sesuai kebutuhan.



Dalam konfigurasi VLAN, terdapat dua jenis utama port yang digunakan untuk mengatur lalu lintas jaringan, yaitu **Access Port** dan **Trunk Port**. Masing-masing port memiliki fungsi yang berbeda dalam bagaimana mereka menangani lalu lintas VLAN.

#### 1. **Access Port VLAN**

**Access Port** adalah port yang menghubungkan perangkat end-user (seperti komputer, printer, atau telepon IP) ke jaringan VLAN. Access port hanya dapat terhubung ke **satu VLAN** tertentu, dan perangkat yang terhubung melalui access port tidak menyadari keberadaan VLAN.

**Karakteristik Access Port:**

* **Satu VLAN**: Access port hanya membawa lalu lintas dari satu VLAN saja. Paket yang melewati access port **tidak memiliki VLAN tag** (tag 802.1Q).
* **Untagged Traffic**: Lalu lintas yang masuk dan keluar dari access port tidak ditandai dengan tag VLAN. Ketika perangkat mengirim data ke switch melalui access port, switch akan otomatis menetapkan paket tersebut ke VLAN yang sesuai berdasarkan konfigurasi port.
* **Untuk Perangkat End-User**: Access port biasanya digunakan untuk menghubungkan perangkat akhir seperti komputer, printer, atau telepon VoIP yang tidak mendukung atau tidak perlu mengetahui VLAN.

**Contoh Konfigurasi Access Port:**

Jika sebuah komputer terhubung ke port switch yang dikonfigurasi sebagai access port untuk VLAN 10, paket yang dikirim dari komputer itu akan diterima oleh switch dan langsung diperlakukan sebagai bagian dari VLAN 10.

**Contoh dalam Mikrotik:**

Misalkan Anda ingin membuat port `ether2` sebagai access port untuk VLAN 10. Dalam Mikrotik, Anda bisa mengonfigurasi **pvid (Port VLAN ID)** untuk port tersebut agar menjadi access port:

```bash
/interface bridge
add name=bridge1

/interface bridge port
add bridge=bridge1 interface=ether2 pvid=10
```

* **pvid=10**: Menetapkan port `ether2` sebagai access port untuk VLAN 10. Lalu lintas yang keluar dari port ini akan **untagged**, namun switch akan mengenali semua paket yang diterima dari port ini sebagai bagian dari VLAN 10.

#### 2. **Trunk Port VLAN**

**Trunk Port** adalah port yang menghubungkan switch ke switch lain atau ke perangkat lain yang juga mendukung VLAN (seperti router atau firewall). Trunk port dapat membawa lalu lintas dari **banyak VLAN** sekaligus. Trunk port bekerja dengan menambahkan **VLAN tag** pada setiap paket yang dikirimkan, sehingga perangkat penerima dapat mengetahui VLAN asal paket tersebut.

**Karakteristik Trunk Port:**

* **Mendukung Banyak VLAN**: Trunk port dapat membawa lalu lintas dari beberapa VLAN secara bersamaan. Paket dari berbagai VLAN yang melewati trunk port akan diberi **tag VLAN** (VLAN ID) menggunakan standar **IEEE 802.1Q**.
* **Tagged Traffic**: Setiap paket yang melewati trunk port diberi **tag** berupa VLAN ID, yang memungkinkan switch atau perangkat lain di ujung trunk mengenali VLAN mana yang terkait dengan paket tersebut.
* **Inter-Switch Communication**: Trunk port sering digunakan untuk menghubungkan beberapa switch atau switch dengan router, sehingga beberapa VLAN dapat dibawa dalam satu koneksi fisik antar perangkat. Ini menghemat port dan kabel fisik.

**Native VLAN:**

Trunk port biasanya juga memiliki satu **Native VLAN**. Paket yang tidak ditandai (untagged traffic) yang dikirim melalui trunk port akan dianggap berasal dari Native VLAN. Default Native VLAN di banyak perangkat biasanya adalah VLAN 1, tetapi bisa diubah untuk alasan keamanan atau kebutuhan jaringan lainnya.

**Contoh Penggunaan Trunk Port:**

Jika dua switch saling terhubung melalui trunk port, dan ada beberapa VLAN (misalnya VLAN 10 dan VLAN 20) yang diterapkan pada kedua switch, trunk port akan membawa lalu lintas dari kedua VLAN tersebut dalam satu koneksi fisik, dan setiap paket dari VLAN 10 atau VLAN 20 akan diberi tag VLAN.

**Contoh dalam Mikrotik:**

Untuk mengonfigurasi trunk port pada Mikrotik, Anda perlu menambahkan interface VLAN pada trunk port yang membawa lalu lintas dari berbagai VLAN. Misalkan Anda ingin membuat `ether3` sebagai trunk port untuk VLAN 10 dan VLAN 20:

```bash
/interface bridge
add name=bridge1

/interface bridge port
add bridge=bridge1 interface=ether3

/interface vlan
add name=vlan10 interface=ether3 vlan-id=10
add name=vlan20 interface=ether3 vlan-id=20
```

Dalam contoh ini:

* `ether3` adalah trunk port yang membawa lalu lintas dari VLAN 10 dan VLAN 20.
* VLAN ID (10 dan 20) ditambahkan ke paket-paket yang melewati trunk port tersebut, sehingga perangkat lain yang terhubung ke ujung lain trunk port dapat mengenali VLAN asal paket tersebut.

#### 3. **Perbedaan Access Port dan Trunk Port**

| Atribut                     | **Access Port**                                        | **Trunk Port**                                                             |
| --------------------------- | ------------------------------------------------------ | -------------------------------------------------------------------------- |
| **Jumlah VLAN yang dibawa** | Satu VLAN saja                                         | Banyak VLAN                                                                |
| **Tagging VLAN**            | Tidak ada (Untagged)                                   | Paket ditandai dengan VLAN ID (Tagged)                                     |
| **Penggunaan**              | Untuk perangkat end-user seperti komputer atau printer | Untuk menghubungkan switch ke switch, router, atau perangkat jaringan lain |
| **Fungsi**                  | Memisahkan perangkat ke dalam satu VLAN                | Membawa traffic dari beberapa VLAN sekaligus                               |
| **Contoh Penggunaan**       | Komputer di VLAN 10                                    | Menghubungkan dua switch yang membawa VLAN 10, 20, 30                      |

#### 4. **Trunk Port dan Access Port dalam Inter-VLAN Routing**

Dalam **Inter-VLAN Routing**, trunk port memainkan peran penting. Jika Anda memiliki beberapa VLAN yang terhubung ke router atau Layer 3 switch untuk memungkinkan komunikasi antar-VLAN, maka trunk port digunakan untuk mengirim lalu lintas VLAN ke router, di mana router akan mengarahkan paket antar-VLAN.

Sebagai contoh, jika Anda memiliki VLAN 10 untuk `IT` dan VLAN 20 untuk `Finance`, dan Anda ingin mereka bisa saling berkomunikasi, trunk port akan mengirimkan lalu lintas tagged dari kedua VLAN tersebut ke router, di mana router akan mengatur **routing antar VLAN**.

#### 5. **Keamanan pada Trunk Port**

Trunk port dapat menimbulkan risiko keamanan jika tidak dikonfigurasi dengan benar. Misalnya, jika Native VLAN pada trunk port tidak dikonfigurasi dengan baik, maka perangkat yang tidak berwenang dapat mengirimkan lalu lintas tanpa tag yang akan diterima oleh trunk port, yang dapat menyebabkan masalah keamanan (serangan VLAN hopping). Oleh karena itu, sangat penting untuk:

* Mengubah Native VLAN dari default (VLAN 1) ke VLAN yang aman.
* Menggunakan fitur **VLAN pruning** untuk membatasi VLAN yang diizinkan lewat pada trunk port.

#### Kesimpulan

* **Access Port**: Hanya mendukung satu VLAN dan digunakan untuk menghubungkan perangkat end-user. Traffic yang melewati access port tidak ditag dengan VLAN ID.
* **Trunk Port**: Digunakan untuk membawa traffic dari beberapa VLAN sekaligus, terutama untuk menghubungkan switch ke switch atau switch ke router. Traffic yang lewat pada trunk port diberi VLAN tag agar perangkat di ujung trunk port dapat mengetahui VLAN asal paket.

Dengan memahami perbedaan dan penggunaan **Access Port** dan **Trunk Port**, Anda dapat mengatur dan mengelola jaringan VLAN dengan lebih efektif, baik untuk menghubungkan perangkat end-user maupun untuk menghubungkan antar perangkat jaringan dengan skala yang lebih besar.\


## Tagged and Unttaged VLAN

Dalam jaringan VLAN, istilah **tagged** dan **untagged** merujuk pada bagaimana lalu lintas data diidentifikasi saat melewati switch atau perangkat jaringan lain. Pemahaman yang baik tentang **tagged VLAN** dan **untagged VLAN** sangat penting untuk mengelola dan mengonfigurasi jaringan yang menggunakan teknologi VLAN, khususnya dalam skenario yang melibatkan beberapa VLAN yang berjalan di atas satu infrastruktur fisik.

#### 1. **Tagged VLAN**

**Tagged VLAN** adalah metode di mana paket data yang melewati jaringan **diberi identifikasi (tag)** untuk menunjukkan VLAN mana yang berhubungan dengan paket tersebut. Tag ini disisipkan ke dalam header Ethernet dari setiap paket, dan perangkat jaringan yang memahami VLAN akan memeriksa tag tersebut untuk menentukan VLAN asal atau tujuan paket.

* **Tag VLAN** ditambahkan menggunakan standar **IEEE 802.1Q**. Tag ini biasanya berupa **4-byte field** yang disisipkan ke dalam header Ethernet untuk mengidentifikasi **VLAN ID** dari paket tersebut.
* **VLAN ID** (yang merupakan bagian dari tag) adalah nomor unik antara **1 hingga 4094** yang digunakan untuk membedakan antara VLAN yang berbeda di jaringan.
* Paket yang melewati **trunk port** sering kali ditag karena trunk port membawa lalu lintas dari **beberapa VLAN** sekaligus, dan setiap paket harus diidentifikasi agar perangkat penerima tahu dari VLAN mana paket tersebut berasal.

**Bagaimana Tagging Bekerja:**

Saat paket data melewati switch atau perangkat lain yang mengaktifkan VLAN tagging:

1. **Tag VLAN** akan ditambahkan pada paket tersebut saat dikirimkan dari trunk port. Tag ini berisi **VLAN ID** yang menandakan VLAN asal paket.
2. Perangkat penerima (biasanya switch atau router lain) akan membaca **VLAN tag** tersebut untuk mengetahui ke VLAN mana paket itu terkait.
3. Setelah paket mencapai tujuan di jaringan, tag VLAN dapat **dihapus** oleh perangkat penerima, khususnya ketika paket dikirim ke perangkat yang tidak memahami VLAN (misalnya, komputer atau printer).

**Contoh Tagged VLAN:**

Jika sebuah switch memiliki port trunk yang membawa VLAN 10 dan VLAN 20, paket yang melewati trunk port akan ditag dengan VLAN ID 10 atau 20. Switch di sisi lain akan membaca tag ini dan menentukan ke VLAN mana paket tersebut harus diarahkan.

**Pada Mikrotik:**

Misalnya, Anda ingin mengonfigurasi VLAN tagging pada trunk port `ether2` untuk VLAN 10 dan VLAN 20:

```bash
/interface vlan
add interface=ether2 name=vlan10 vlan-id=10
add interface=ether2 name=vlan20 vlan-id=20
```

Di sini, `ether2` akan membawa lalu lintas dari VLAN 10 dan VLAN 20, dan setiap paket yang dikirimkan melalui port ini akan ditag dengan ID VLAN 10 atau 20.

#### 2. **Untagged VLAN**

**Untagged VLAN** merujuk pada lalu lintas jaringan yang **tidak memiliki tag VLAN** di header Ethernet-nya. Untagged VLAN biasanya diterapkan pada **access port**, yaitu port yang menghubungkan perangkat end-user (seperti komputer, printer, atau perangkat IoT) ke jaringan.

* Access port hanya dapat membawa lalu lintas dari satu VLAN, jadi paket yang keluar dari port ini tidak perlu ditag, karena port tersebut hanya terkait dengan satu VLAN.
* Switch secara otomatis mengenali VLAN asal paket berdasarkan konfigurasi port dan menambahkan informasi VLAN yang relevan **tanpa menambahkan tag pada paket**.
* Jika untagged traffic melewati trunk port, itu akan diperlakukan sebagai bagian dari **native VLAN** (VLAN default untuk port tersebut).

**Bagaimana Untagged VLAN Bekerja:**

1. Perangkat end-user (misalnya komputer) mengirim paket melalui **access port**, dan paket tersebut **tidak memiliki tag VLAN**.
2. Switch menerima paket tersebut dan secara otomatis mengetahui bahwa paket itu terkait dengan VLAN yang dikonfigurasi untuk port tersebut, tanpa perlu menambahkan tag.
3. Paket dikirim ke VLAN yang sesuai di jaringan, atau jika harus melewati trunk port, paket tersebut bisa saja **ditag** di trunk untuk ditransmisikan ke perangkat lainnya.

**Contoh Untagged VLAN:**

Jika sebuah komputer terhubung ke port `ether3`, yang dikonfigurasi untuk VLAN 10, semua paket yang dikirim dari komputer melalui port ini tidak akan ditag. Namun, switch tahu bahwa lalu lintas tersebut termasuk ke VLAN 10 berdasarkan konfigurasi port.

**Pada Mikrotik:**

Untuk mengonfigurasi `ether3` sebagai access port untuk VLAN 10 (tidak menggunakan tagging):

```bash
/interface bridge
add name=bridge1
/interface bridge port
add bridge=bridge1 interface=ether3 pvid=10
```

* `pvid=10` menetapkan bahwa port `ether3` adalah access port untuk VLAN 10, dan lalu lintas yang masuk dari port ini akan dianggap sebagai bagian dari VLAN 10 tanpa penambahan tag VLAN.

#### 3. **Perbedaan Tagged VLAN dan Untagged VLAN**

| **Karakteristik**         | **Tagged VLAN**                                                                 | **Untagged VLAN**                                           |
| ------------------------- | ------------------------------------------------------------------------------- | ----------------------------------------------------------- |
| **VLAN ID dalam paket**   | Paket diberi tag dengan VLAN ID menggunakan 802.1Q                              | Paket tidak diberi tag, VLAN diidentifikasi oleh port       |
| **Jenis Port**            | Biasanya diterapkan pada trunk port yang membawa banyak VLAN                    | Biasanya diterapkan pada access port yang membawa satu VLAN |
| **Penggunaan**            | Untuk membawa traffic dari banyak VLAN di antara switch atau perangkat jaringan | Untuk menghubungkan perangkat end-user ke satu VLAN         |
| **Komunikasi antar VLAN** | Menggunakan tag untuk memastikan paket sampai ke VLAN yang benar                | VLAN diidentifikasi melalui port, tidak perlu tagging       |
| **Contoh Perangkat**      | Switch ke switch, switch ke router (trunking)                                   | Komputer, printer, perangkat end-user                       |

#### 4. **Native VLAN dan Hubungannya dengan Tagged dan Untagged Traffic**

Dalam konfigurasi trunk port, ada konsep yang disebut **Native VLAN**. Native VLAN adalah VLAN yang digunakan untuk menangani **untagged traffic** yang melewati trunk port. Secara default, jika paket tidak ditag saat melewati trunk port, itu akan dianggap sebagai bagian dari Native VLAN.

* Pada trunk port, semua VLAN lain akan membawa traffic yang ditag, kecuali Native VLAN, yang membawa traffic yang tidak ditag.
* **Native VLAN default** biasanya adalah VLAN 1, tetapi dapat diubah sesuai dengan kebutuhan untuk alasan keamanan atau operasional.

**Native VLAN dalam Keamanan:**

Native VLAN bisa menjadi potensi kelemahan keamanan dalam jaringan VLAN, terutama dalam **serangan VLAN hopping**, di mana attacker dapat menyisipkan paket yang tidak ditag ke trunk port untuk mencoba mengakses VLAN lain.

Untuk mencegah hal ini:

* Disarankan untuk mengubah Native VLAN dari VLAN 1 ke VLAN yang tidak digunakan di jaringan.
* Menggunakan fitur **VLAN pruning** untuk membatasi VLAN yang dapat melewati trunk port dan menghindari akses yang tidak diinginkan.

#### 5. **Proses Kerja dalam Jaringan dengan Tagged dan Untagged VLAN**

1. **Access Port (Untagged VLAN)**:
   * Misalnya, perangkat komputer pada access port di VLAN 10 mengirimkan paket.
   * Paket yang dikirim melalui access port ini **tidak diberi tag** VLAN, tetapi switch tahu bahwa semua lalu lintas yang datang dari port ini harus diarahkan ke VLAN 10.
2. **Trunk Port (Tagged VLAN)**:
   * Paket dari VLAN 10 melewati trunk port, dan sebelum dikirim, **tag VLAN 10** ditambahkan ke paket tersebut.
   * Switch lain di sisi trunk port akan menerima paket ini, membaca tag VLAN-nya, dan meneruskan paket ke VLAN 10 di jaringan penerima.
3. **Native VLAN (Untagged Traffic di Trunk Port)**:
   * Jika paket yang lewat pada trunk port tidak memiliki tag, maka trunk port akan menganggap paket tersebut sebagai bagian dari Native VLAN.

#### Kesimpulan

* **Tagged VLAN**: Digunakan untuk mengidentifikasi VLAN mana yang terkait dengan setiap paket yang melewati jaringan, terutama pada trunk port. Paket yang dikirim melalui trunk port akan diberi tag VLAN menggunakan standar IEEE 802.1Q.
* **Untagged VLAN**: Biasanya digunakan pada access port, di mana paket yang dikirim dari perangkat end-user tidak diberi tag VLAN. Switch akan mengenali VLAN berdasarkan port yang digunakan.
* **Native VLAN**: VLAN yang digunakan untuk menangani untagged traffic di trunk port. Paket yang melewati trunk port tanpa tag VLAN dianggap sebagai bagian dari Native VLAN.

Memahami perbedaan antara **tagged** dan **untagged VLAN** memungkinkan Anda untuk merancang dan mengelola jaringan VLAN yang aman dan efisien.



## Vlan Filtering

**VLAN Filtering** di Mikrotik adalah fitur yang memungkinkan Anda mengontrol bagaimana perangkat Mikrotik menangani lalu lintas VLAN. Dengan VLAN Filtering, Anda dapat menentukan VLAN mana yang diizinkan atau ditolak pada port tertentu serta bagaimana paket diproses saat melalui switch. VLAN Filtering membantu mengoptimalkan kinerja jaringan dengan memastikan hanya VLAN yang diizinkan yang dapat melewati port tertentu, sekaligus meningkatkan keamanan jaringan.

Di Mikrotik, VLAN Filtering biasanya digunakan di dalam **Bridge Interface**, karena Mikrotik menggunakan bridge untuk menghubungkan beberapa port secara virtual dan menerapkan aturan filtering VLAN di dalamnya.

#### 1. **Konsep Dasar VLAN Filtering**

VLAN Filtering pada Mikrotik menggunakan **IEEE 802.1Q** untuk menandai paket dengan ID VLAN. Proses filtering ini akan menentukan apakah paket dari VLAN tertentu diizinkan untuk melewati bridge atau port tertentu. Fitur ini sangat bermanfaat untuk mengelola lalu lintas di antara beberapa VLAN yang berjalan di infrastruktur fisik yang sama.

**Komponen Utama dalam VLAN Filtering:**

* **Bridge**: Komponen yang menghubungkan beberapa interface dan memungkinkan lalu lintas antar-VLAN.
* **VLAN**: Sebuah segmen jaringan logis yang diidentifikasi oleh ID VLAN.
* **VLAN ID**: Nomor yang mengidentifikasi setiap VLAN secara unik.
* **VLAN Tagging/Untagging**: Proses menambahkan atau menghapus tag VLAN di paket jaringan.
* **pvid (Port VLAN ID)**: VLAN default yang diasosiasikan dengan port tertentu. Paket yang datang ke port ini akan otomatis dianggap berasal dari VLAN yang ditentukan oleh pvid.
* **VLAN Table**: Tabel yang berisi konfigurasi VLAN, termasuk ID dan port mana yang diizinkan untuk VLAN tersebut.

#### 2. **Bagaimana VLAN Filtering Bekerja di Mikrotik**

Ketika VLAN Filtering diaktifkan, Mikrotik akan melakukan hal-hal berikut:

* **Memfilter** lalu lintas berdasarkan ID VLAN.
* **Menandai** atau **menghapus tag** VLAN dari paket tergantung pada konfigurasi.
* **Memastikan** hanya paket dari VLAN yang sah dapat melewati port yang terhubung ke VLAN tersebut.

**Alur Kerja:**

1. Paket masuk ke bridge melalui port.
2. Mikrotik memeriksa tag VLAN pada paket (jika ada).
3. Paket dengan tag VLAN yang sesuai akan diteruskan melalui bridge atau port yang diizinkan berdasarkan aturan VLAN filtering.
4. Jika paket datang tanpa tag VLAN, Mikrotik akan memeriksa **pvid** dari port tersebut untuk menentukan VLAN asal paket.

#### 3. **Mengaktifkan VLAN Filtering pada Mikrotik**

Untuk mengaktifkan VLAN Filtering pada Mikrotik, Anda perlu membuat konfigurasi bridge dan mengatur **VLAN IDs** serta **pvid** di port-port tertentu. Berikut langkah-langkah untuk konfigurasi dasar VLAN Filtering:

**Langkah 1: Buat Bridge**

```bash
/interface bridge
add name=bridge1 vlan-filtering=yes
```

Perintah ini membuat sebuah bridge dengan **vlan-filtering** yang diaktifkan.

**Langkah 2: Tambahkan Port ke Bridge**

```bash
/interface bridge port
add bridge=bridge1 interface=ether1 pvid=10
add bridge=bridge1 interface=ether2 pvid=20
```

* `ether1` dikaitkan dengan VLAN 10 (pvid=10), dan semua paket yang datang ke `ether1` tanpa tag VLAN akan dianggap sebagai bagian dari VLAN 10.
* `ether2` dikaitkan dengan VLAN 20 (pvid=20).

**Langkah 3: Konfigurasi VLAN**

```bash
/interface bridge vlan
add bridge=bridge1 tagged=ether3 untagged=ether1 vlan-ids=10
add bridge=bridge1 tagged=ether3 untagged=ether2 vlan-ids=20
```

Pada konfigurasi ini:

* VLAN 10 akan berjalan melalui port `ether1` (untagged) dan `ether3` (tagged).
* VLAN 20 akan berjalan melalui port `ether2` (untagged) dan `ether3` (tagged).

**Langkah 4: Mengaktifkan VLAN Filtering**

```bash
/interface bridge set bridge1 vlan-filtering=yes
```

Langkah ini memastikan bahwa hanya lalu lintas dari VLAN yang diizinkan dapat melewati bridge.

#### 4. **VLAN Filtering dan pvid**

* **pvid** atau Port VLAN ID adalah VLAN default untuk port tersebut. Paket yang masuk ke port dengan pvid tertentu tanpa tag VLAN akan secara otomatis dianggap sebagai bagian dari VLAN yang sama dengan pvid.
* Ini penting karena perangkat end-user (seperti PC atau printer) biasanya tidak menandai paket dengan ID VLAN (tidak support VLAN tagging), sehingga Anda dapat menggunakan **pvid** untuk mengelompokkan lalu lintas dari port tertentu ke dalam VLAN tertentu.

Misalnya, jika `ether1` memiliki **pvid=10**, maka semua paket yang datang tanpa tag VLAN akan secara otomatis dianggap sebagai lalu lintas dari VLAN 10.

#### 5. **Keuntungan VLAN Filtering**

* **Keamanan**: Dengan mengaktifkan VLAN Filtering, Anda dapat membatasi lalu lintas dari VLAN yang tidak sah, sehingga mengurangi potensi risiko akses yang tidak diizinkan.
* **Efisiensi**: Hanya paket dari VLAN yang diizinkan yang akan diteruskan, mengurangi kemungkinan terjadinya broadcast storm atau pemborosan bandwidth karena paket tidak relevan.
* **Kontrol yang Lebih Baik**: Anda memiliki kontrol penuh atas bagaimana lalu lintas diproses di antara VLAN yang berbeda, memungkinkan Anda mengelola lalu lintas lebih efektif.

#### 6. **Best Practices untuk VLAN Filtering**

* **Pemisahan VLAN dengan Tepat**: Pastikan bahwa VLAN yang berbeda digunakan untuk memisahkan kelompok pengguna atau perangkat yang memerlukan segmentasi keamanan atau lalu lintas yang berbeda.
* **Penggunaan Trunk dan Access Port**: Konfigurasikan port yang membawa banyak VLAN (trunk port) dengan benar, dan pastikan port yang hanya menghubungkan perangkat end-user (access port) memiliki konfigurasi VLAN yang tepat.
* **VLAN ID yang Konsisten**: Gunakan ID VLAN yang konsisten di seluruh jaringan Anda untuk memudahkan manajemen dan troubleshooting.

#### 7. **Contoh Konfigurasi Lengkap VLAN Filtering**

Berikut adalah contoh konfigurasi lengkap VLAN Filtering pada Mikrotik dengan dua VLAN (10 dan 20) dan trunk port:

```bash
# Membuat bridge dengan vlan-filtering diaktifkan
/interface bridge
add name=bridge1 vlan-filtering=yes

# Menambahkan port ke bridge dengan pvid tertentu
/interface bridge port
add bridge=bridge1 interface=ether1 pvid=10  # Port ini untuk VLAN 10
add bridge=bridge1 interface=ether2 pvid=20  # Port ini untuk VLAN 20
add bridge=bridge1 interface=ether3          # Port trunk yang membawa VLAN 10 dan VLAN 20

# Konfigurasi VLAN IDs untuk VLAN 10 dan VLAN 20
/interface bridge vlan
add bridge=bridge1 tagged=ether3 untagged=ether1 vlan-ids=10
add bridge=bridge1 tagged=ether3 untagged=ether2 vlan-ids=20

# Mengaktifkan VLAN Filtering pada bridge1
/interface bridge set bridge1 vlan-filtering=yes
```

Dalam contoh ini:

* `ether1` dan `ether2` adalah access port untuk VLAN 10 dan VLAN 20.
* `ether3` adalah trunk port yang membawa lalu lintas dari kedua VLAN 10 dan VLAN 20.
* VLAN Filtering aktif, sehingga hanya paket dari VLAN yang diizinkan yang akan diteruskan di masing-masing port.

#### Kesimpulan

**VLAN Filtering** di Mikrotik memungkinkan Anda untuk mengelola bagaimana paket VLAN diproses di seluruh jaringan Anda. Dengan menggunakan VLAN Filtering, Anda dapat membatasi VLAN mana yang dapat melewati port tertentu, menjaga keamanan, dan meningkatkan efisiensi jaringan Anda.



**Ingress Filtering** pada Mikrotik adalah fitur yang digunakan untuk memfilter atau menyaring paket yang masuk ke interface tertentu berdasarkan ID VLAN yang telah dikonfigurasi. Fungsi ini terutama digunakan untuk memastikan bahwa hanya paket dengan **VLAN ID** yang sesuai (seperti yang ditentukan dalam pengaturan VLAN pada bridge) yang dapat diterima oleh port/interface tersebut. Paket dengan VLAN ID yang tidak sesuai akan ditolak atau dibuang.

#### Fungsi dan Cara Kerja Ingress Filtering:

1. **Pemeriksaan VLAN ID**: Ketika **Ingress Filtering** diaktifkan pada port tertentu, setiap frame yang masuk akan diperiksa VLAN ID-nya. Jika frame tersebut bertanda (tagged) dan VLAN ID-nya tidak cocok dengan VLAN yang diizinkan pada port tersebut, maka frame tersebut akan dibuang.
2. **Mencegah Lalu Lintas Tidak Sah**: Fitur ini berfungsi untuk mencegah lalu lintas dari VLAN yang tidak diizinkan memasuki jaringan melalui port yang salah. Ini meningkatkan keamanan jaringan karena memastikan hanya VLAN yang ditentukan saja yang dapat mengakses port tertentu.
3. **Penanganan Frame Untagged**: Ingress Filtering juga berperan dalam menangani frame untagged (tanpa VLAN ID). Jika port memiliki **pvid** (Port VLAN ID) yang dikonfigurasi, frame untagged akan secara otomatis dimasukkan ke VLAN default (berdasarkan pvid). Namun, frame untagged bisa saja dibuang jika pengaturan pvid atau Ingress Filtering tidak sesuai.
4. **Kompatibilitas dengan Bridge VLAN Filtering**: Ingress Filtering bekerja bersamaan dengan **VLAN Filtering** di bridge. Jika VLAN Filtering diaktifkan pada bridge, Ingress Filtering akan menambah lapisan kontrol tambahan terhadap bagaimana traffic VLAN diteruskan di dalam bridge.

#### Opsi Konfigurasi Ingress Filtering pada Mikrotik:

* **Enable/Disable**: Ingress Filtering biasanya dapat diaktifkan atau dinonaktifkan per port. Ketika diaktifkan, filtering berdasarkan VLAN ID akan dilakukan secara otomatis.

#### Contoh Penerapan Ingress Filtering:

Misalkan Anda memiliki dua VLAN: **VLAN 10** dan **VLAN 20**. Jika port `ether1` hanya diizinkan untuk menerima lalu lintas dari VLAN 10, Anda dapat mengaktifkan **Ingress Filtering** di port ini sehingga hanya paket dengan tag VLAN 10 yang diterima.

**Langkah-langkah konfigurasi:**

1.  Mengatur VLAN Filtering pada Bridge:

    ```bash
    /interface bridge
    add name=bridge1 vlan-filtering=yes
    ```
2.  Menambahkan port ke bridge dan mengaktifkan Ingress Filtering:

    ```bash
    /interface bridge port
    add bridge=bridge1 interface=ether1 pvid=10 ingress-filtering=yes
    ```

Dengan konfigurasi di atas:

* Port `ether1` hanya akan menerima lalu lintas dari VLAN 10.
* Jika ada paket dari VLAN lain atau paket untagged yang tidak sesuai dengan pengaturan pvid, paket tersebut akan dibuang.

#### Keuntungan Menggunakan Ingress Filtering:

1. **Keamanan**: Mencegah lalu lintas dari VLAN yang tidak sah memasuki jaringan Anda melalui port yang salah.
2. **Pengendalian Lalu Lintas**: Memastikan bahwa setiap port hanya menerima lalu lintas VLAN yang relevan, sehingga meningkatkan efisiensi dan performa jaringan.
3. **Pencegahan Broadcast Storm**: Dengan hanya mengizinkan VLAN yang ditentukan, Anda bisa mengurangi risiko broadcast storm yang dapat terjadi jika banyak VLAN berbeda mengakses port yang sama.

#### Kesimpulan:

**Ingress Filtering** adalah fitur penting untuk mengontrol bagaimana paket VLAN diterima oleh port dalam jaringan Mikrotik. Dengan mengaktifkan fitur ini, Anda dapat memperketat kontrol akses VLAN, meningkatkan keamanan, dan mengelola lalu lintas jaringan dengan lebih baik.



Gambar tersebut menampilkan pilihan **EtherType** pada pengaturan VLAN di perangkat Mikrotik. **EtherType** merupakan sebuah field dalam header frame Ethernet yang menunjukkan tipe protokol yang dikandung oleh payload dari frame tersebut. Dalam konteks VLAN, **EtherType** menunjukkan tipe frame Ethernet yang mendukung tagging VLAN.

Pada gambar tersebut, terdapat tiga pilihan EtherType yang bisa dipilih:

#### 1. **0x8100** (Standar VLAN Tagging - IEEE 802.1Q)

* **0x8100** adalah **EtherType** standar yang digunakan untuk frame VLAN tagging menurut standar **IEEE 802.1Q**.
* Ketika Anda memilih **0x8100**, frame Ethernet yang keluar dari interface akan ditandai dengan tag VLAN standar yang berisi informasi seperti **VLAN ID** dan **priority**.
* **0x8100** adalah pilihan yang paling umum digunakan dalam jaringan VLAN. Frame dengan EtherType ini akan menandai frame dengan VLAN ID yang sudah diatur pada perangkat jaringan yang kompatibel dengan VLAN 802.1Q.

#### 2. **0x88a8** (Q-in-Q Tunneling / IEEE 802.1ad)

* **0x88a8** adalah **EtherType** yang digunakan untuk **Q-in-Q VLAN tagging** (juga dikenal sebagai **stacked VLAN** atau **provider bridging**).
* **Q-in-Q** digunakan untuk menambahkan tag VLAN ganda pada frame, sehingga frame yang sudah memiliki tag VLAN dapat diberi tag tambahan oleh penyedia layanan. Ini berguna untuk meneruskan lalu lintas VLAN di antara jaringan pelanggan tanpa konflik dengan VLAN internal provider.
* Dengan **Q-in-Q**, frame yang sudah ditag VLAN oleh pelanggan (VLAN customer) diberi tag VLAN tambahan oleh penyedia (VLAN service provider), sehingga dapat dikelola lebih baik dalam jaringan besar yang dikelola oleh ISP atau penyedia layanan.

#### 3. **0x9100** (Proprietary VLAN Tagging)

* **0x9100** adalah EtherType yang digunakan untuk **tag VLAN khusus**. Standar ini sering digunakan oleh beberapa vendor jaringan untuk menerapkan VLAN yang berbeda dari standar **802.1Q**.
* Ini biasanya digunakan untuk aplikasi khusus atau untuk keperluan proprietary yang memerlukan VLAN tagging yang lebih fleksibel atau berbeda dari yang umum.
* Sering ditemukan di lingkungan jaringan yang kompleks atau dalam jaringan perusahaan yang menggunakan peralatan yang mendukung EtherType ini.

#### Fungsi EtherType dalam VLAN:

EtherType adalah penanda dalam header Ethernet yang memberitahukan perangkat jaringan bagaimana memproses frame yang diterima. Dalam konteks VLAN:

* EtherType menentukan apakah frame tersebut mengandung informasi VLAN dan bagaimana informasi VLAN tersebut diproses.
* Dengan memilih salah satu EtherType yang sesuai, Anda bisa mengatur apakah frame yang ditag menggunakan **VLAN standar** (0x8100), **Q-in-Q** (0x88a8), atau **proprietary tagging** (0x9100).

#### Kesimpulan:

* **0x8100**: Umum digunakan untuk tagging VLAN standar 802.1Q.
* **0x88a8**: Digunakan untuk Q-in-Q atau VLAN bertumpuk (stacked VLAN), umumnya dalam skenario ISP atau jaringan penyedia layanan.
* **0x9100**: Digunakan untuk VLAN proprietary atau khusus, mungkin untuk aplikasi vendor tertentu atau keperluan jaringan yang lebih spesifik.

Pemilihan EtherType ini memungkinkan pengaturan VLAN yang lebih fleksibel tergantung pada kebutuhan jaringan dan tipe VLAN yang sedang diimplementasikan.

## Service Tag

Pada perangkat Mikrotik, **Service Tag** adalah opsi yang sering ditemukan di pengaturan VLAN, khususnya ketika menggunakan fitur **Q-in-Q** atau **stacked VLAN**. Fungsi utama dari **Service Tag** adalah untuk menambahkan tag VLAN kedua di atas tag VLAN yang sudah ada pada suatu frame Ethernet. Ini biasanya digunakan dalam jaringan penyedia layanan (ISP) untuk memisahkan lalu lintas pelanggan dengan cara yang lebih efisien.

Berikut penjelasan detail tentang **Service Tag**:

#### 1. **Fungsi Service Tag dalam VLAN (Q-in-Q)**

* **Service Tag** digunakan untuk menandai frame Ethernet yang sudah memiliki **tag VLAN** dengan tag tambahan, yang dikenal sebagai **Outer VLAN Tag**.
* Fitur ini memungkinkan penyedia layanan untuk memasukkan lalu lintas VLAN dari beberapa pelanggan ke dalam satu VLAN besar dengan menambahkan tag VLAN tambahan. Ini sering disebut sebagai **Q-in-Q** atau **double-tagging**.
* Dalam skenario **Q-in-Q**, tag VLAN pelanggan disebut sebagai **C-TAG (Customer Tag)** dan tag tambahan yang diterapkan oleh penyedia layanan disebut sebagai **S-TAG (Service Tag)**.

#### 2. **Penggunaan Service Tag (S-TAG) di Jaringan ISP**

* Dalam jaringan penyedia layanan seperti ISP, banyak pelanggan menggunakan VLAN untuk memisahkan jaringan mereka. Namun, agar penyedia layanan dapat mengelola banyak VLAN pelanggan ini tanpa tumpang tindih, mereka menggunakan **Service Tag (S-TAG)** untuk memberi label pada lalu lintas pelanggan.
* Dengan mengaktifkan **Service Tag**, penyedia layanan dapat melacak dan mengarahkan lalu lintas pelanggan mereka dengan lebih efisien melalui jaringan mereka. Penyedia layanan menambahkan tag VLAN kedua, sehingga memungkinkan beberapa pelanggan dengan VLAN yang sama (misalnya, VLAN 10) untuk ditransmisikan melalui jaringan tanpa konflik.
* **S-TAG** juga memungkinkan isolasi antar pelanggan, sehingga setiap pelanggan memiliki jaringan yang terpisah dan tidak saling berinteraksi meskipun mereka menggunakan VLAN yang sama.

#### 3. **Cara Kerja Q-in-Q dengan Service Tag**

* **Q-in-Q** (atau **802.1ad**) memungkinkan frame Ethernet yang sudah memiliki tag VLAN (802.1Q) dari jaringan pelanggan ditambahkan dengan tag VLAN tambahan (S-TAG) oleh penyedia layanan.
* Contoh skenario:
  * Sebuah pelanggan mengirim frame Ethernet dengan tag VLAN 100 (C-TAG).
  * Penyedia layanan menambahkan **Service Tag** dengan VLAN 200 (S-TAG) ke frame tersebut.
  * Ketika frame ini berjalan melalui jaringan penyedia layanan, frame tersebut sekarang memiliki dua tag VLAN: VLAN 100 (C-TAG) dan VLAN 200 (S-TAG).
  * Ketika frame mencapai titik akhir di jaringan pelanggan, tag VLAN yang ditambahkan oleh penyedia layanan (S-TAG) dapat dihapus, meninggalkan tag asli pelanggan (C-TAG).

#### 4. **Penerapan Service Tag pada Mikrotik**

* Pada router atau switch Mikrotik, ketika **Service Tag** dicentang, sistem akan menambahkan tag VLAN kedua (S-TAG) di atas tag VLAN yang ada pada frame (C-TAG).
* Opsi ini biasanya hanya digunakan jika Anda bekerja di lingkungan yang memerlukan pengelolaan VLAN untuk beberapa pelanggan yang berbeda, seperti pada jaringan penyedia layanan (ISP).
* Service Tag juga memungkinkan jaringan penyedia layanan untuk menjaga VLAN pelanggan tetap independen, sehingga mereka bisa mengelola dan mengisolasi pelanggan secara lebih efektif.

#### 5. **Pentingnya EtherType dalam Service Tag**

* Ketika Anda mengaktifkan **Service Tag**, tipe frame yang digunakan biasanya **0x88a8**, yang merupakan **EtherType** untuk **Q-in-Q VLAN tagging**. Ini memungkinkan frame bertumpuk (stacked) di jaringan penyedia layanan.

#### 6. **Contoh Penggunaan Service Tag di Mikrotik**

Misalkan sebuah ISP menggunakan VLAN 1000 sebagai **Service Tag** untuk menandai semua lalu lintas pelanggan, sementara pelanggan mereka menggunakan VLAN 10, VLAN 20, dan VLAN 30. Dengan mengaktifkan **Service Tag**:

* Frame yang berasal dari pelanggan dengan VLAN 10 akan diberi tag kedua (S-TAG) dengan VLAN 1000.
* Frame dari pelanggan lain dengan VLAN 20 juga akan diberi tag kedua dengan VLAN 1000.
* Sehingga semua lalu lintas akan membawa tag VLAN tambahan (VLAN 1000) selama transit melalui jaringan penyedia layanan.

#### Kesimpulan:

* **Service Tag** memungkinkan penyedia layanan atau administrator jaringan untuk menambahkan tag VLAN tambahan (S-TAG) di atas VLAN pelanggan (C-TAG), yang memungkinkan pengelolaan dan pemisahan lalu lintas yang lebih efisien di jaringan besar.
* Opsi ini sering digunakan dalam **Q-in-Q (802.1ad)** untuk memungkinkan beberapa pelanggan dengan VLAN yang sama tetapi independen dapat menggunakan jaringan yang sama tanpa konflik.
* **Service Tag** memastikan bahwa lalu lintas antar pelanggan tetap terisolasi di jaringan penyedia layanan, sehingga meningkatkan keamanan dan manajemen VLAN di skala besar.

Fitur ini sangat berguna di jaringan penyedia layanan (ISP) dan lingkungan jaringan yang lebih besar di mana pemisahan dan isolasi lalu lintas menjadi krusial.



## Bridge

**Bridge** pada Mikrotik adalah fitur yang memungkinkan beberapa interface jaringan (seperti ethernet, wireless, atau VLAN) untuk dihubungkan menjadi satu domain atau segmen jaringan yang sama, seolah-olah mereka berada di satu switch fisik. **Bridge** berperan penting dalam menyederhanakan dan memperluas jaringan lokal (LAN) dengan menggabungkan beberapa interface dan memperlakukan mereka sebagai satu jaringan logis.

Berikut adalah penjelasan detail tentang **Bridge** pada Mikrotik:

#### 1. **Fungsi Utama Bridge**

Bridge berfungsi untuk **menyambungkan** beberapa interface fisik (misalnya, ethernet port, wireless) atau virtual (seperti VLAN) ke dalam satu jaringan Layer 2. Pada dasarnya, **Bridge** akan membuat perangkat bertindak seperti **switch** yang memungkinkan perangkat-perangkat lain di jaringan yang sama untuk berkomunikasi.

* **Layer 2 switching**: Bridge bekerja di Layer 2 dari model OSI, yang artinya mereka meneruskan frame Ethernet berdasarkan alamat MAC (bukan berdasarkan IP seperti Layer 3 routing).
* **Menggabungkan beberapa interface**: Dengan bridge, Anda bisa menggabungkan beberapa interface fisik (seperti Ethernet dan Wireless) atau VLAN ke dalam satu domain broadcast yang sama.
* **Mengelola VLAN**: Bridge juga sering digunakan dalam topologi jaringan untuk mengelola VLAN di Mikrotik dengan menambahkan **filtering** dan **tagging** pada frame Ethernet.

#### 2. **Konsep Bridge dalam Jaringan**

Ketika beberapa interface berada dalam **Bridge**, mereka semua berfungsi sebagai bagian dari **satu domain broadcast**. Semua perangkat yang terhubung ke interface-interface yang menjadi anggota Bridge bisa berkomunikasi satu sama lain secara langsung. Sebagai contoh, jika Anda memiliki tiga port ethernet (eth1, eth2, dan eth3) dan menambahkannya ke dalam Bridge, perangkat yang terhubung ke port-port ini akan dianggap berada dalam satu LAN, seperti dalam jaringan switch biasa.

#### 3. **Bridge vs Switch di Mikrotik**

Mikrotik memungkinkan Anda menggunakan **Bridge** sebagai cara untuk mensimulasikan fungsi switch. Namun, jika perangkat Mikrotik memiliki **chip switching hardware**, penggunaan **switch chip** (dengan fitur **master-slave** pada interface) lebih efisien daripada menggunakan Bridge, karena Bridge biasanya diproses di CPU.

Namun, dalam beberapa kasus, terutama dengan perangkat yang tidak memiliki hardware switch, **Bridge** sering digunakan untuk mencapai efek yang sama.

#### 4. **Fitur-Fitur Utama pada Bridge Mikrotik**

Berikut beberapa fitur penting yang tersedia dalam konfigurasi **Bridge** di Mikrotik:

**a. Spanning Tree Protocol (STP/RSTP)**

* **STP** dan **RSTP** adalah protokol yang digunakan untuk mencegah **looping** dalam jaringan Layer 2, yang bisa terjadi ketika ada lebih dari satu jalur antara switch atau bridge di jaringan.
* **STP** memblokir jalur redundan yang bisa menyebabkan loop, memastikan jalur tunggal yang aman untuk frame Ethernet. Jika jalur utama gagal, jalur cadangan akan diaktifkan.
* Mikrotik mendukung **RSTP** (Rapid Spanning Tree Protocol), yang merupakan versi lebih cepat dari STP. Anda bisa mengaktifkan **RSTP** pada Bridge untuk mencegah loop di jaringan Anda.

**b. VLAN Filtering**

* **VLAN filtering** memungkinkan Bridge untuk menerapkan aturan VLAN pada frame Ethernet yang melewati interface Bridge. Ini berarti Bridge dapat memisahkan lalu lintas VLAN yang berbeda meskipun mereka menggunakan satu trunk port fisik.
* **Tagged** dan **Untagged** frame VLAN bisa dikelola secara efektif di Mikrotik menggunakan Bridge dengan **VLAN filtering** aktif.

**c. Hardware Offload**

* Jika perangkat mendukung **hardware offload**, proses bridging dapat dilakukan pada **switch chip** di perangkat, yang membuatnya lebih efisien. Hardware offload mengurangi beban pada CPU karena pemrosesan dilakukan di level hardware.
* Untuk mengaktifkan hardware offload, interface yang ada dalam bridge harus kompatibel dengan switch chip pada perangkat Mikrotik.

**d. Bridge Ports**

* Setiap **interface** (ethernet, wireless, VLAN) yang dimasukkan ke dalam Bridge disebut sebagai **port**.
* **Bridge Port** adalah pengaturan di mana setiap interface individu yang terhubung ke Bridge bisa diatur lebih lanjut. Anda bisa menentukan VLAN, prioritas, atau mengatur pembatasan pada port-port tertentu.

**e. IP Addressing**

* Anda tidak bisa memberikan **alamat IP** langsung pada interface yang ada dalam Bridge. Alamat IP harus ditetapkan pada **Bridge interface** itu sendiri. Semua interface yang tergabung ke dalam Bridge akan berbagi alamat IP yang sama.

#### 5. **Contoh Penggunaan Bridge di Mikrotik**

Berikut adalah beberapa contoh penggunaan **Bridge** dalam skenario jaringan nyata:

**a. Menggabungkan Interface**

Jika Anda memiliki beberapa port ethernet di router dan ingin menjadikannya bagian dari satu jaringan, Anda bisa membuat **Bridge** dan menambahkan port-port tersebut sebagai anggota Bridge.

```bash
/interface bridge add name=bridge1
/interface bridge port add bridge=bridge1 interface=ether1
/interface bridge port add bridge=bridge1 interface=ether2
/interface bridge port add bridge=bridge1 interface=ether3
```

Dengan konfigurasi ini, **ether1**, **ether2**, dan **ether3** menjadi bagian dari satu jaringan LAN dan dapat berkomunikasi satu sama lain seperti dalam switch.

**b. Bridge VLAN dengan VLAN Filtering**

Anda bisa mengonfigurasi **VLAN filtering** di **Bridge** untuk mengelola lalu lintas VLAN.

```bash
/interface bridge add name=bridge1 vlan-filtering=yes
/interface bridge port add bridge=bridge1 interface=ether1
/interface bridge port add bridge=bridge1 interface=ether2
/interface bridge vlan add bridge=bridge1 tagged=ether1,ether2 vlan-ids=10
/interface bridge vlan add bridge=bridge1 tagged=ether1 untagged=ether2 vlan-ids=20
```

Dalam konfigurasi ini, VLAN 10 akan ditandai sebagai **tagged** pada **ether1** dan **ether2**, sedangkan VLAN 20 akan ditandai sebagai **untagged** pada **ether2**.

#### 6. **Keuntungan Bridge**

* **Konsolidasi**: Menghubungkan beberapa interface jaringan ke dalam satu segmen jaringan, sehingga memudahkan manajemen dan pengaturan jaringan.
* **Pemrosesan VLAN**: Mendukung VLAN filtering, yang memungkinkan pemrosesan frame VLAN dengan cara yang efisien.
* **Multicast dan Broadcast**: Memungkinkan pengaturan broadcast domain dan mengelola multicast yang lebih baik.
* **Fleksibilitas**: Bisa digunakan dalam berbagai topologi jaringan, baik jaringan rumah, bisnis, maupun jaringan penyedia layanan.

#### 7. **Keterbatasan Bridge**

* **Beban CPU**: Pada perangkat yang tidak mendukung hardware switching, Bridge dapat membebani CPU, terutama jika ada banyak traffic yang melewati interface Bridge.
* **Latency**: Jika digunakan tanpa hardware offload, Bridge bisa menambah latency dalam jaringan dibandingkan dengan penggunaan switch fisik.

#### Kesimpulan

**Bridge** pada Mikrotik sangat berguna untuk menyatukan beberapa interface menjadi satu domain Layer 2. Dengan fitur-fitur seperti **VLAN filtering**, **STP/RSTP**, dan **hardware offload**, Bridge memungkinkan pengaturan jaringan yang fleksibel dan efisien. Namun, dalam skenario dengan banyak traffic, penggunaan **hardware switch** lebih disarankan untuk meminimalkan beban pada CPU.
