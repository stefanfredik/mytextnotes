# Firewall

## Fastpath dan Fastrack

**FastPath** dan **FastTrack** adalah dua fitur yang dirancang untuk meningkatkan kinerja routing dan switching pada perangkat Mikrotik. Keduanya berfungsi untuk mempercepat penanganan paket jaringan melalui perangkat Mikrotik dengan mengoptimalkan cara paket diproses. Berikut penjelasan detail mengenai kedua fitur ini beserta contoh penerapannya.

***

#### 1. **FastPath**

**Pengertian:**

**FastPath** adalah metode di mana perangkat Mikrotik dapat melewatkan paket langsung melalui hardware tanpa harus diproses oleh CPU perangkat. Ini memberikan peningkatan throughput yang signifikan, terutama pada perangkat dengan beban lalu lintas yang tinggi.

Pada perangkat yang mendukung **hardware acceleration**, seperti pada switch Mikrotik atau beberapa router, **FastPath** memungkinkan perangkat untuk menghindari pemrosesan penuh di CPU dan menggunakan jalur yang lebih cepat untuk memproses paket yang sederhana.

**Cara Kerja:**

* **FastPath** bekerja dengan menghindari aturan firewall, queue, dan fitur-fitur lain yang memerlukan CPU untuk memproses setiap paket.
* Ketika paket memenuhi kriteria untuk dilewatkan melalui **FastPath**, paket tersebut langsung diteruskan ke interface tujuan tanpa pemeriksaan yang mendalam.
* **FastPath** secara otomatis aktif jika kondisi perangkat dan konfigurasi mendukungnya.

**Fitur yang Mendukung FastPath:**

* **Bridging** (untuk hardware switch)
* **Routing** (untuk paket yang bisa diproses tanpa inspeksi mendalam)
* **VLANs** (untuk perangkat yang mendukung switch chip VLAN offload)

**Penerapan:**

Pada situasi di mana tidak ada banyak aturan firewall yang kompleks atau fitur seperti queueing, **FastPath** bisa memberikan peningkatan kinerja. **FastPath** paling berguna untuk jaringan besar dengan lalu lintas tinggi, di mana throughput adalah prioritas utama.

**Studi Kasus:**

* **Kantor atau Data Center dengan Lalu Lintas Tinggi**: Sebuah kantor besar yang menghubungkan banyak cabang dan memiliki lalu lintas internal yang besar dapat memanfaatkan **FastPath**. Misalnya, router yang menghubungkan kantor pusat dan cabang bisa menggunakan **FastPath** untuk mempercepat lalu lintas antar-kantor, di mana paket-paket tersebut tidak memerlukan filtering firewall yang ketat.

**Cara Mengaktifkan FastPath:**

**FastPath** biasanya diaktifkan secara otomatis, tetapi Anda bisa memeriksa apakah **FastPath** aktif atau tidak melalui perintah di terminal:

```bash
/interface ethernet print detail
```

Jika **FastPath** aktif, Anda akan melihat keterangan **"running-fastpath"** pada interface.

***

#### 2. **FastTrack**

**Pengertian:**

**FastTrack** adalah fitur khusus pada firewall Mikrotik yang memungkinkan mempercepat pemrosesan paket-paket yang cocok dengan aturan firewall tertentu. **FastTrack** mengurangi overhead CPU dengan memproses paket lebih cepat pada aturan firewall yang di-tracked.

**FastTrack** pada dasarnya mempercepat **Connection Tracking** di firewall dengan cara membuat koneksi yang sering digunakan melewati jalur khusus yang lebih efisien.

**Cara Kerja:**

* Ketika koneksi tertentu dicocokkan dengan aturan **FastTrack**, koneksi ini akan diproses lebih cepat di masa depan, dan paket yang melewatinya tidak lagi diproses oleh semua aturan firewall.
* Ini sangat berguna untuk lalu lintas yang memiliki banyak koneksi berkelanjutan, seperti **VPN**, **streaming**, **gaming**, atau **trafik P2P**.
* **FastTrack** akan menyimpan detail koneksi untuk sementara, dan ketika koneksi tersebut digunakan lagi, paket akan diproses lebih cepat tanpa harus melalui inspeksi firewall penuh.

**Kondisi yang Mendukung FastTrack:**

* Tidak semua paket bisa di-**FastTrack**, terutama jika Anda menggunakan fitur-fitur seperti **Mangle**, **Queueing**, atau **Logging** pada firewall. **FastTrack** bekerja paling baik ketika koneksi tidak memerlukan pemrosesan firewall lanjutan.

**Penerapan:**

**FastTrack** biasanya digunakan untuk meningkatkan performa jaringan yang memiliki lalu lintas tinggi dan konstan, seperti streaming video, gaming online, atau VPN, di mana penting untuk menjaga latensi rendah dan throughput tinggi.

**Studi Kasus:**

* **Rumah dengan Koneksi Fiber dan Penggunaan Berat**: Misalnya, di rumah dengan banyak perangkat yang digunakan untuk **streaming video**, **gaming**, dan **koneksi VPN** secara bersamaan, **FastTrack** dapat diaktifkan untuk mempercepat koneksi ini dan mencegah router overload. Ini akan membuat pengalaman **streaming** atau **gaming** lebih lancar dengan latensi rendah.

**Cara Mengaktifkan FastTrack:**

Untuk mengaktifkan **FastTrack**, Anda harus membuat aturan firewall di Mikrotik yang menggunakan target **FastTrack**. Berikut contoh cara melakukannya:

```bash
/ip firewall filter add chain=forward action=fasttrack-connection connection-state=established,related
/ip firewall filter add chain=forward action=accept connection-state=established,related
```

Pada aturan ini:

* **FastTrack** hanya akan diterapkan pada koneksi yang berstatus **established** atau **related**.
* Setelah koneksi di-**FastTrack**, aturan firewall lainnya tidak akan diterapkan pada koneksi tersebut.

Anda bisa memeriksa apakah **FastTrack** bekerja dengan melihat statistik **firewall**:

```bash
/ip firewall filter print stats
```

Jika aturan **FastTrack** memiliki hit count, itu berarti koneksi telah berhasil di-**FastTrack**.

***

#### **Perbedaan antara FastPath dan FastTrack**

* **FastPath** berlaku secara umum untuk paket yang tidak memerlukan pemrosesan oleh fitur-fitur tertentu, seperti firewall atau queueing. **FastPath** langsung memproses paket di jalur cepat melalui hardware acceleration.
* **FastTrack**, di sisi lain, bekerja pada level firewall untuk mempercepat koneksi yang telah di-**tracked** oleh firewall, terutama untuk koneksi yang sedang berlangsung (established atau related).

Kedua fitur ini bertujuan untuk meningkatkan kinerja, tetapi **FastTrack** lebih fokus pada optimisasi aturan firewall, sedangkan **FastPath** adalah metode optimasi umum untuk semua jenis lalu lintas yang tidak membutuhkan pemeriksaan yang mendalam.

***

#### **Kesimpulan**

* **FastPath** adalah metode optimasi untuk seluruh lalu lintas jaringan, yang menghindari CPU dan memproses paket dengan jalur cepat melalui perangkat keras.
* **FastTrack** adalah fitur optimasi khusus untuk koneksi yang melalui firewall Mikrotik, terutama yang berstatus **established** dan **related**, untuk mempercepat pemrosesan aturan firewall.

Keduanya sangat bermanfaat dalam situasi dengan lalu lintas jaringan yang tinggi dan kompleks, dan dapat digunakan bersama-sama atau secara terpisah, tergantung pada kebutuhan spesifik jaringan.



Agar fitur **FastTrack** pada Mikrotik dapat **aktif** dan berfungsi dengan benar, ada beberapa syarat dan kriteria yang perlu dipenuhi. Fitur **FastTrack** dirancang untuk mempercepat pemrosesan koneksi yang telah di-tracked oleh firewall, tetapi tidak semua koneksi dapat di-**FastTrack**. Berikut syarat dan kriteria yang harus diperhatikan:

#### **Syarat dan Kriteria untuk FastTrack Aktif**

1.  **Firewall Connection Tracking**

    * **FastTrack** hanya bekerja pada koneksi yang menggunakan **Connection Tracking**. Connection Tracking adalah fitur yang mencatat status setiap koneksi yang melewati firewall, seperti koneksi yang **established** atau **related**.
    * **FastTrack** dapat diterapkan hanya pada koneksi dengan status **established** (koneksi yang sudah aktif) atau **related** (koneksi yang terkait dengan koneksi lain).

    Koneksi dengan status **new** (koneksi baru) tidak bisa langsung di-**FastTrack** sampai koneksi tersebut menjadi **established**.
2.  **Aturan Firewall FastTrack**

    * Anda harus membuat aturan firewall yang secara eksplisit menggunakan aksi **fasttrack-connection**. Tanpa aturan ini, **FastTrack** tidak akan aktif.

    Contoh aturan FastTrack di firewall:

    ```bash
    /ip firewall filter add chain=forward action=fasttrack-connection connection-state=established,related
    /ip firewall filter add chain=forward action=accept connection-state=established,related
    ```
3. **Tidak Ada Penggunaan Mangle yang Mempengaruhi Paket**
   * **FastTrack** tidak akan aktif jika ada aturan **mangle** yang memodifikasi paket-paket tersebut. Jika ada pemrosesan seperti marking (penandaan) pada **mangle**, maka paket tersebut tidak bisa di-**FastTrack**.
   * Jika Anda memerlukan **mangle** untuk memodifikasi paket, FastTrack tidak bisa digunakan.
4. **Tidak Ada Queue (Antrian) pada Koneksi**
   * Koneksi yang menggunakan **Simple Queue** atau **Queue Tree** tidak bisa di-**FastTrack**. Ini karena queue memerlukan pemrosesan paket secara mendetail, yang bertentangan dengan prinsip FastTrack untuk mempercepat pemrosesan koneksi.
   * Jika ada aturan queue yang aktif pada koneksi tertentu, FastTrack tidak bisa diterapkan pada koneksi tersebut.
5. **Tidak Menggunakan Logging pada Firewall**
   * Koneksi yang di-**log** melalui aturan firewall juga tidak bisa di-**FastTrack**, karena logging memerlukan pemrosesan paket secara penuh oleh CPU untuk mencatat informasi log.
6. **Tidak Menggunakan IPsec**
   * **FastTrack** tidak dapat digunakan pada koneksi yang terenkripsi dengan **IPsec**. Paket yang sudah di-enkripsi oleh IPsec tidak bisa di-**FastTrack** karena proses dekripsi memerlukan inspeksi paket secara penuh.
7. **Hardware dan Firmware yang Mendukung**
   * Pastikan perangkat Mikrotik Anda mendukung **FastTrack**. Beberapa perangkat Mikrotik yang lebih lama mungkin tidak mendukung fitur ini, atau mungkin memerlukan upgrade firmware untuk mengaktifkannya.
8. **Tidak Menggunakan VLAN Filtering atau Bridge Filtering**
   * Jika Anda menggunakan **VLAN filtering** atau **Bridge firewall filtering**, paket tidak bisa di-**FastTrack** karena harus melewati inspeksi VLAN atau bridge filtering.
9. **Interface yang Mendukung FastTrack**
   * Interface yang terlibat dalam koneksi harus mendukung **FastTrack**. Misalnya, interface fisik yang menggunakan bridging atau VLAN filtering mungkin tidak kompatibel dengan FastTrack.

#### **Kriteria Koneksi yang Bisa Di-FastTrack**

Koneksi yang dapat di-**FastTrack** harus memenuhi kriteria berikut:

* **Status Koneksi**: Koneksi harus dalam status **established** atau **related**.
* **Protokol Koneksi**: FastTrack bekerja dengan sebagian besar protokol koneksi umum seperti **TCP** dan **UDP**.
* **Koneksi yang Stabil**: Koneksi dengan lalu lintas yang konsisten dan stabil, seperti streaming video, gaming, atau koneksi VPN, cenderung dapat di-**FastTrack**.

#### **Verifikasi FastTrack Aktif**

Setelah menerapkan aturan **FastTrack**, Anda bisa memverifikasi apakah koneksi benar-benar di-**FastTrack** dengan melihat statistik firewall:

```bash
/ip firewall filter print stats
```

Jika **FastTrack** aktif, aturan **fasttrack-connection** akan memiliki **hit count** yang menunjukkan bahwa koneksi sedang dipercepat oleh **FastTrack**.

#### **Studi Kasus Penggunaan FastTrack**

* **Jaringan Rumah dengan Streaming Video**: Dalam jaringan rumah dengan banyak perangkat yang melakukan streaming video (YouTube, Netflix) atau bermain game online, **FastTrack** dapat mempercepat koneksi tersebut, mengurangi penggunaan CPU, dan meningkatkan throughput. Jika tidak ada aturan mangle, queue, atau logging yang diterapkan, koneksi ini dapat dipercepat dengan **FastTrack**.
* **Koneksi VPN pada Kantor Kecil**: Koneksi VPN yang digunakan untuk menghubungkan kantor dengan kantor cabang dapat menggunakan **FastTrack** untuk mempercepat transfer data selama koneksi sudah **established**. Namun, **IPsec** tidak bisa menggunakan **FastTrack**, jadi hanya koneksi VPN yang tidak terenkripsi oleh **IPsec** yang bisa dipercepat.

***

Dengan memenuhi syarat-syarat ini, **FastTrack** akan aktif dan memberikan peningkatan kinerja dalam pemrosesan paket pada Mikrotik.
