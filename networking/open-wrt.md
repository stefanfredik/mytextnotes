# Open WRT

## Install  Open WRT di Raspi

#### 1. **Instalasi OpenWRT di Raspberry Pi**

Pastikan OpenWRT sudah diinstal pada Raspberry Pi kamu. Jika belum, kamu bisa mengunduh image OpenWRT yang sesuai dengan model Raspberry Pi dari [situs resmi OpenWRT](https://openwrt.org/toh/raspberry\_pi\_foundation/raspberry\_pi).

#### 2. **Login ke OpenWRT**

Setelah OpenWRT terpasang, sambungkan Raspberry Pi ke jaringan dan login menggunakan SSH atau akses langsung melalui terminal:

```bash
ssh root@<IP_RaspberryPi>
```

Ganti `<IP_RaspberryPi>` dengan alamat IP perangkatmu.

#### 3. **Instalasi Web Server (uHTTPd atau Nginx)**

Secara default, OpenWRT menggunakan **uHTTPd** sebagai web server untuk antarmuka LuCI. Jika kamu ingin menggunakan **Nginx** atau **Lighttpd**, ikuti langkah-langkah di bawah ini.

**a. Menggunakan uHTTPd (Web Server Default)**

Jika kamu hanya ingin menggunakan uHTTPd, pastikan service ini sudah terinstal dan berjalan:

```bash
opkg update
opkg install uhttpd luci
/etc/init.d/uhttpd start
/etc/init.d/uhttpd enable
```

Perintah di atas akan menginstal uHTTPd dan antarmuka LuCI, lalu memulai service uHTTPd.

**b. Menggunakan Nginx**

Jika kamu lebih suka menggunakan **Nginx**, ikuti langkah-langkah berikut untuk menginstalnya:

```bash
opkg updaterestart
opkg install nginx luci-ssl
```

Setelah itu, mulai dan aktifkan Nginx:

```bash
/etc/init.d/nginx start
/etc/init.d/nginx enable
```

**c. Menggunakan Lighttpd**

Jika kamu ingin menggunakan **Lighttpd** sebagai alternatif, ikuti langkah-langkah berikut:

```bash
opkg update
opkg install lighttpd lighttpd-mod-cgi
```

Setelah diinstal, aktifkan dan mulai Lighttpd:

```bash
/etc/init.d/lighttpd start
/etc/init.d/lighttpd enable
```

#### 4. **Konfigurasi Firewall**

Pastikan port 80 dan 443 (untuk HTTPS jika menggunakan SSL) dibuka di firewall OpenWRT, agar web server bisa diakses dari jaringan luar:

```bash
uci add firewall rule
uci set firewall.@rule[-1].src='wan'
uci set firewall.@rule[-1].proto='tcp'
uci set firewall.@rule[-1].dest_port='80 443'
uci set firewall.@rule[-1].target='ACCEPT'
uci commit firewall
/etc/init.d/firewall restart
```

#### 5. **Akses Web Interface**

Setelah web server berjalan, kamu bisa mengakses web interface OpenWRT melalui browser dengan alamat:

```
http://<IP_RaspberryPi>
```

Jika menggunakan HTTPS:

```
https://<IP_RaspberryPi>
```

Dengan langkah-langkah di atas, web server di OpenWRT pada Raspberry Pi seharusnya sudah aktif dan bisa diakses.

Untuk menambahkan WiFi pada mode bridge di OpenWRT, kamu perlu mengkonfigurasi antarmuka wireless sebagai _bridge_ antara jaringan lokal (LAN) dan jaringan WiFi. Berikut adalah langkah-langkah untuk menambahkan WiFi dalam mode bridge menggunakan command line (CLI).

## Wireless Config

#### Langkah-langkah untuk Menambahkan WiFi dalam Mode Bridge

**1. Login ke OpenWRT**

Masuk ke perangkat OpenWRT menggunakan SSH:

```bash
ssh root@<IP_OpenWRT>
```

Ganti `<IP_OpenWRT>` dengan alamat IP perangkat OpenWRT kamu.

**2. Konfigurasi Wireless untuk Mode Bridge**

1.  **Tambahkan Konfigurasi WiFi** Jalankan perintah berikut untuk menambahkan konfigurasi WiFi dan mengatur mode bridge:

    ```bash
    uci set wireless.@wifi-iface[0]=wifi-iface                      # Tambah wifi-iface baru
    uci set wireless.@wifi-iface[-1].device='radio0'               # Menggunakan radio yang sesuai (ganti dengan radio yang ada)
    uci set wireless.@wifi-iface[-1].mode='ap'                      # Mode AP
    uci set wireless.@wifi-iface[-1].ssid='<SSID_Bridge>'          # Ganti dengan SSID yang diinginkan
    uci set wireless.@wifi-iface[-1].network='lan'                  # Menghubungkan ke jaringan LAN
    uci set wireless.@wifi-iface[-1].encryption='psk2'             # Atur enkripsi
    uci set wireless.@wifi-iface[-1].key='<Kata_Sandi_WiFi>'       # Ganti dengan kata sandi WiFi
    uci commit wireless                                              # Simpan perubahan
    ```

    Ganti `<SSID_Bridge>` dengan nama jaringan WiFi yang diinginkan dan `<Kata_Sandi_WiFi>` dengan kata sandi yang sesuai.
2.  **Restart Jaringan** Setelah melakukan pengaturan, restart jaringan agar perubahan diterapkan:

    ```bash
    /etc/init.d/network restart
    ```

**3. Verifikasi Konfigurasi**

Setelah konfigurasi, kamu bisa memeriksa apakah WiFi berfungsi dengan baik. Gunakan perintah berikut untuk melihat status wireless:

```bash
iw dev
```

atau

```bash
uci show wireless
```

#### Melalui LuCI (GUI)

Jika kamu lebih suka menggunakan antarmuka grafis, berikut adalah langkah-langkahnya:

1. **Buka LuCI** Akses LuCI dengan memasukkan alamat IP OpenWRT di browser (`http://<IP_OpenWRT>`).
2. **Navigasi ke Network > Wireless** Pilih **Network**, kemudian klik **Wireless**.
3. **Tambahkan Interface Wireless**
   * Klik **Add** di radio yang ingin kamu gunakan untuk mode bridge.
   * Pilih **Mode: Access Point**.
   * Isi **SSID** dengan nama jaringan yang diinginkan.
   * Pilih **Network** sebagai **lan** untuk menghubungkan ke jaringan lokal.
   * Atur **Encryption** sesuai kebutuhan dan masukkan kata sandi.
   * Klik **Save & Apply**.
4. **Restart Router (Opsional)** Meskipun LuCI biasanya merestart secara otomatis, kamu bisa pergi ke **System > Reboot** untuk merestart router jika diperlukan.

#### Uji Koneksi

Setelah semua pengaturan selesai, coba sambungkan perangkat ke SSID yang telah dibuat dan pastikan perangkat bisa mengakses internet melalui jaringan yang sama.

Jika ada pertanyaan lebih lanjut atau jika kamu memerlukan informasi tambahan, silakan tanyakan!

## Basic Config

### Hapus port eth0 dari bridge br-lan

```bash
brct delif br-lan eth0
```

### Tambahkan Eth0 pada Interface WAN

```bash
uci set network.wan.ifname='eth0'
uci commit
/etc/init.d/network restart
```



## Blokir  Via Firewall

### Blokir using Mac

Untuk membuat blokir terjadwal pada interface wireless di OpenWRT, kamu bisa menggunakan perintah berikut untuk mematikan Wi-Fi pada waktu tertentu:

```bash
uci add firewall rule
uci set firewall.@rule[-1].name='Blokir_Wireless_Terjadwal'
uci set firewall.@rule[-1].src='lan'
uci set firewall.@rule[-1].dest='wan'
uci set firewall.@rule[-1].src_mac='<MAC_Tertentu>'       # Ganti dengan MAC address perangkat yang ingin diblokir
uci set firewall.@rule[-1].target='REJECT'
uci set firewall.@rule[-1].start_time='22:00'             # Waktu mulai blokir (format HH:MM)
uci set firewall.@rule[-1].stop_time='06:00'              # Waktu berhenti blokir
uci set firewall.@rule[-1].weekdays='Mon Tue Wed Thu Fri' # Hari-hari blokir
uci commit firewall
/etc/init.d/firewall restart
```

#### Penjelasan Singkat:

* **Blokir akses perangkat tertentu di Wi-Fi** berdasarkan MAC address pada jadwal tertentu.

## Blokir terjadwal using Cronjob

### Blokir using Firewall

Untuk memblokir MAC address tertentu pada interface wireless secara terjadwal di OpenWRT, gunakan kombinasi **cron** dan **UCI** untuk mengaktifkan atau menonaktifkan akses perangkat berdasarkan MAC address pada waktu yang ditentukan.

#### Langkah 1: Buat Rule Blokir MAC Address

Tambahkan rule firewall untuk memblokir MAC address, tapi kita aktifkan dan nonaktifkan rule ini dengan cron nanti.

```bash
uci add firewall rule
uci set firewall.@rule[-1].name='Blokir_MAC_Wireless'
uci set firewall.@rule[-1].src='lan'
uci set firewall.@rule[-1].src_mac='<MAC_Tertentu>'    # Ganti dengan MAC address yang ingin diblokir
uci set firewall.@rule[-1].target='REJECT'
uci set firewall.@rule[-1].enabled='0'                 # Nonaktifkan rule ini secara default
uci commit firewall
/etc/init.d/firewall restart
```

#### Langkah 2: Atur Cron Job untuk Mengaktifkan dan Menonaktifkan Blokir

Edit cron job dengan perintah berikut:

```bash
crontab -e
```

Tambahkan baris-baris berikut ke dalam file cron:

```bash
# Aktifkan blokir MAC address pada pukul 08:00
0 8 * * * uci set firewall.@rule[-1].enabled='1'; uci commit firewall; /etc/init.d/firewall restart

# Nonaktifkan blokir MAC address pada pukul 17:00
0 17 * * * uci set firewall.@rule[-1].enabled='0'; uci commit firewall; /etc/init.d/firewall restart
```

#### Penjelasan Singkat:

* **Blokir MAC address tertentu** di Wi-Fi dari jam 8 pagi hingga jam 5 sore.

Tentu, berikut adalah penjelasan rinci tentang kode **cron job** di atas untuk mengaktifkan dan menonaktifkan blokir MAC address tertentu di OpenWRT:

#### 1. **Baris Cron untuk Mengaktifkan Blokir MAC Address**

```bash
0 8 * * * uci set firewall.@rule[-1].enabled='1'; uci commit firewall; /etc/init.d/firewall restart
```

**Penjelasan Detail**

* **`0 8 * * *`**: Ini adalah waktu ketika cron job dijalankan.
  * `0` : Menunjukkan menit ke-0, artinya tepat pada awal jam.
  * `8` : Menunjukkan jam 8 pagi.
  * `* * *`: Tiga tanda bintang terakhir adalah pengaturan untuk **hari, bulan, dan hari dalam minggu**. Karena menggunakan tanda `*`, berarti aturan ini akan berjalan **setiap hari**.
* **`uci set firewall.@rule[-1].enabled='1'`**:
  * `uci` adalah singkatan dari **Unified Configuration Interface**, yaitu sistem konfigurasi di OpenWRT.
  * `firewall.@rule[-1]` menunjukkan **rule terakhir** di konfigurasi firewall (karena kita baru saja membuatnya pada langkah sebelumnya).
  * `.enabled='1'` mengaktifkan rule tersebut. `1` berarti "aktif," dan `0` berarti "nonaktif."
* **`;`**: Tanda titik koma ini memisahkan perintah, sehingga beberapa perintah dapat dieksekusi dalam satu baris.
* **`uci commit firewall`**:
  * Menyimpan atau "commit" perubahan konfigurasi pada firewall. Ini memastikan bahwa konfigurasi terbaru disimpan dan siap diterapkan.
* **`/etc/init.d/firewall restart`**:
  * Merestart layanan firewall agar perubahan segera diterapkan, yaitu mengaktifkan rule blokir untuk MAC address yang ditargetkan.

#### 2. **Baris Cron untuk Menonaktifkan Blokir MAC Address**

```bash
0 17 * * * uci set firewall.@rule[-1].enabled='0'; uci commit firewall; /etc/init.d/firewall restart
```

**Penjelasan Detail**

* **`0 17 * * *`**: Waktu cron job untuk menonaktifkan blokir.
  * `0` : Menit ke-0, pada awal jam.
  * `17` : Jam 17, yaitu pukul 5 sore.
  * `* * *` : Menunjukkan bahwa job ini berjalan setiap hari.
* **`uci set firewall.@rule[-1].enabled='0'`**:
  * Mengubah nilai `.enabled` menjadi `0` pada rule terakhir di firewall, yang berarti aturan ini dinonaktifkan.
* **`uci commit firewall`**:
  * Menyimpan perubahan konfigurasi firewall sehingga blokir ini bisa dihentikan.
* **`/etc/init.d/firewall restart`**:
  * Merestart layanan firewall agar perubahan ini diterapkan, dan MAC address yang diblokir sebelumnya sekarang bisa kembali terhubung.

#### Kesimpulan

Kode di atas akan:

* **Mengaktifkan blokir** pada perangkat dengan MAC address tertentu setiap pukul 08:00 pagi, memutus koneksi perangkat tersebut ke jaringan.
* **Menonaktifkan blokir** pada perangkat tersebut setiap pukul 17:00 sore, mengembalikan akses perangkat ke jaringan.

Ini memungkinkan kamu untuk memblokir akses perangkat secara terjadwal menggunakan konfigurasi OpenWRT.

### Blokir Using Mac Wireless

Untuk melakukan pemfilteran MAC secara terjadwal pada OpenWRT, kamu bisa menggunakan **cron job** untuk menambahkan atau menghapus MAC address dari konfigurasi wireless. Berikut adalah langkah-langkah untuk melakukannya:

#### 1. **Menambahkan MAC Filter**

Misalnya, kamu ingin memblokir MAC address tertentu dari akses wireless. Gunakan perintah berikut untuk menambahkannya ke dalam konfigurasi wireless:

```bash
uci add wireless.macfilter
uci set wireless.macfilter[-1].mac='XX:XX:XX:XX:XX:XX'  # Ganti dengan MAC address yang ingin diblokir
uci set wireless.macfilter[-1].action='deny'  # 'deny' untuk memblokir, 'allow' untuk mengizinkan
uci commit wireless
/etc/init.d/network restart
```

#### 2. **Membuat Cron Job untuk Mengatur MAC Filter Secara Terjadwal**

Buat cron job yang akan mengeksekusi perintah untuk menambahkan atau menghapus MAC filter pada waktu tertentu.

**2.1 Edit Cron Job**

Gunakan perintah berikut untuk mengedit cron job:

```bash
crontab -e
```

**2.2 Contoh Cron Job**

Misalnya, kamu ingin memblokir MAC address pada jam 8 pagi dan mengizinkannya kembali pada jam 5 sore:

```bash
# Memblokir MAC address pada jam 8 pagi
0 8 * * * uci add wireless.macfilter && uci set wireless.macfilter[-1].mac='XX:XX:XX:XX:XX:XX' && uci set wireless.macfilter[-1].action='deny' && uci commit wireless && /etc/init.d/network restart

# Mengizinkan kembali MAC address pada jam 5 sore
0 17 * * * uci add wireless.macfilter && uci set wireless.macfilter[-1].mac='XX:XX:XX:XX:XX:XX' && uci set wireless.macfilter[-1].action='allow' && uci commit wireless && /etc/init.d/network restart
```

#### 3. **Menyimpan dan Keluar**

Setelah menambahkan baris-baris di atas ke dalam file cron, simpan dan keluar dari editor.

#### 4. **Mengecek Cron Job**

Untuk memastikan bahwa cron job telah ditambahkan, kamu dapat menggunakan perintah berikut:

```bash
crontab -l
```

#### Catatan

* Pastikan untuk mengganti `XX:XX:XX:XX:XX:XX` dengan MAC address yang ingin kamu blokir atau izinkan.
* Pastikan juga bahwa pemfilteran MAC diaktifkan di pengaturan wireless OpenWRT.
* Jika kamu ingin menonaktifkan pemfilteran MAC secara keseluruhan, kamu bisa menghapus semua entri MAC filter yang ada dengan perintah `uci delete` atau `uci delete wireless.macfilter[-1]`.

Dengan langkah-langkah di atas, kamu dapat melakukan pemfilteran MAC secara terjadwal pada OpenWRT.



## Format Perintah OpenWrt

Di OpenWRT, perintah biasanya terdiri dari beberapa format dasar dan dapat digunakan untuk konfigurasi sistem atau antarmuka jaringan. Berikut ini adalah penjelasan tentang format dasar perintah di OpenWRT, termasuk beberapa yang paling umum digunakan seperti `uci` dan `opkg`.

#### 1. **Perintah `uci` (Unified Configuration Interface)**

`uci` adalah antarmuka untuk mengelola dan memodifikasi konfigurasi OpenWRT. Format umum perintah `uci` adalah:

```bash
uci <action> <config>.<section>.<option>=<value>
```

**Komponen Format:**

* **`action`**: Operasi yang akan dilakukan, seperti `get`, `set`, `add`, `commit`, dan `delete`.
* **`config`**: Nama file konfigurasi, seperti `network`, `wireless`, atau `firewall`.
* **`section`**: Bagian spesifik dalam konfigurasi yang ingin dimodifikasi atau diambil nilainya.
* **`option`**: Pengaturan tertentu dalam `section` yang ingin diakses atau diubah.
* **`value`**: Nilai yang akan diterapkan pada `option`.

**Contoh:**

*   **Menampilkan Nilai Pengaturan**

    ```bash
    uci get network.lan.ipaddr
    ```

    * `get` mengambil nilai dari konfigurasi.
    * `network` adalah file konfigurasi.
    * `lan` adalah nama section.
    * `ipaddr` adalah option (misalnya, IP address pada LAN).
*   **Mengubah Nilai Pengaturan**

    ```bash
    uci set network.lan.ipaddr='192.168.1.100'
    ```

    * `set` mengubah nilai `ipaddr` pada section `lan` di file `network` menjadi `192.168.1.100`.
*   **Simpan Perubahan**

    ```bash
    uci commit network
    ```

    * Menyimpan perubahan yang sudah dibuat pada file konfigurasi `network`.
*   **Menghapus Section atau Option**

    ```bash
    uci delete firewall.@rule[0]
    ```

    * Menghapus rule firewall pertama di dalam konfigurasi firewall.

#### 2. **Perintah `opkg` (OpenWRT Package Manager)**

`opkg` adalah manajer paket di OpenWRT untuk menginstal, menghapus, atau memperbarui paket.

```bash
opkg <action> <package_name>
```

**Komponen Format:**

* **`action`**: Operasi yang ingin dilakukan seperti `install`, `remove`, atau `update`.
* **`package_name`**: Nama paket yang ingin dimodifikasi.

**Contoh:**

*   **Memperbarui Daftar Paket**

    ```bash
    opkg update
    ```

    * Mengambil daftar terbaru dari paket yang tersedia di repositori OpenWRT.
*   **Menginstal Paket**

    ```bash
    opkg install luci
    ```

    * Menginstal paket `luci`, antarmuka web OpenWRT.
*   **Menghapus Paket**

    ```bash
    opkg remove luci
    ```

    * Menghapus paket `luci` dari sistem.

#### 3. **Perintah `ifconfig` dan `ip` (Konfigurasi Jaringan)**

Untuk melihat atau mengonfigurasi antarmuka jaringan.

**Menggunakan `ifconfig`**

```bash
ifconfig <interface> <option>
```

**Menggunakan `ip`**

```bash
ip <action> <object> <parameters>
```

**Contoh `ifconfig`:**

*   **Melihat Status Semua Interface**

    ```bash
    ifconfig
    ```

    * Menampilkan informasi semua antarmuka jaringan.
*   **Menonaktifkan Interface**

    ```bash
    ifconfig eth0 down
    ```

    * Menonaktifkan antarmuka `eth0`.

**Contoh `ip`:**

*   **Melihat Interface yang Tersedia**

    ```bash
    ip link show
    ```

    * Menampilkan semua antarmuka jaringan.
*   **Mengubah IP Address pada Interface**

    ```bash
    ip addr add 192.168.1.10/24 dev eth0
    ```

    * Mengatur IP address `192.168.1.10` pada antarmuka `eth0`.

#### 4. **Perintah `brctl` (Bridge Control)**

`brctl` digunakan untuk mengelola bridge di jaringan OpenWRT.

```bash
brctl <action> <bridge_name> <interface>
```

**Komponen Format:**

* **`action`**: Operasi seperti `addbr`, `delbr`, `addif`, atau `delif`.
* **`bridge_name`**: Nama bridge yang akan dimodifikasi.
* **`interface`**: Nama interface yang ingin ditambahkan atau dihapus dari bridge.

**Contoh:**

*   **Membuat Bridge Baru**

    ```bash
    brctl addbr br-lan
    ```

    * Membuat bridge dengan nama `br-lan`.
*   **Menambahkan Interface ke Bridge**

    ```bash
    brctl addif br-lan eth0
    ```

    * Menambahkan interface `eth0` ke bridge `br-lan`.

#### 5. **Perintah `reboot` dan `shutdown`**

Perintah ini digunakan untuk merestart atau mematikan OpenWRT.

*   **Reboot Sistem**

    ```bash
    reboot
    ```

    * Merestart sistem OpenWRT.
*   **Mematikan Sistem**

    ```bash
    poweroff
    ```

    * Mematikan sistem OpenWRT.

#### 6. **Perintah `logread` dan `dmesg` (Log Sistem)**

Untuk melihat log sistem di OpenWRT:

*   **Melihat Log Sistem**

    ```bash
    logread
    ```

    * Menampilkan log sistem dari proses `syslogd`.
*   **Melihat Log Kernel**

    ```bash
    dmesg
    ```

    * Menampilkan log kernel yang menunjukkan informasi perangkat keras atau kernel.

#### 7. **Perintah `cron` (Penjadwalan Tugas)**

Mengatur tugas terjadwal di OpenWRT menggunakan `cron`.

```bash
crontab -e
```

Menambahkan tugas terjadwal dengan format berikut:

```
* * * * * <command>
```

Contoh:

*   **Merestart jaringan setiap hari pukul 03:00**

    ```bash
    0 3 * * * /etc/init.d/network restart
    ```

#### Kesimpulan

Perintah di OpenWRT disusun dalam format `action` dan `parameter` sesuai kebutuhan, seperti konfigurasi interface, pengaturan firewall, dan instalasi paket. Dengan kombinasi ini, kamu bisa mengatur dan mengelola berbagai aspek sistem secara fleksibel.



## Basic Config OpenWrt (Raspi5)

Berikut adalah langkah-langkah untuk melakukan pengaturan sesuai kriteria yang diberikan pada OpenWRT di Raspberry Pi dengan interface **eth0** untuk WAN dan **phy0-ap0** untuk akses point:

#### 1. **Konfigurasi Interface eth0 sebagai WAN dengan DHCP Client**

Gunakan perintah berikut untuk mengatur interface **eth0** sebagai WAN yang mendapatkan IP melalui DHCP.

```bash
uci set network.wan=interface
uci set network.wan.ifname='eth0'
uci set network.wan.proto='dhcp'
uci commit network
/etc/init.d/network restart
```

#### 2. **Mengatur Interface phy0-ap0 sebagai Akses Point dengan DHCP Server**

Konfigurasi ini mengatur interface **phy0-ap0** sebagai akses point dengan IP statis dan mengaktifkan DHCP server di jaringan **192.168.100.1/24**.

```bash
uci set network.lan=interface
uci set network.lan.ifname='phy0-ap0'
uci set network.lan.proto='static'
uci set network.lan.ipaddr='192.168.100.1'
uci set network.lan.netmask='255.255.255.0'
uci commit network

uci set dhcp.lan=dhcp
uci set dhcp.lan.interface='lan'
uci set dhcp.lan.start='100'
uci set dhcp.lan.limit='150'
uci set dhcp.lan.leasetime='12h'
uci commit dhcp
/etc/init.d/dnsmasq restart
```

#### 3. **Mengatur Akses SSH dari Jaringan Berbeda**

Untuk mengakses SSH dari jaringan eksternal, pastikan firewall diatur agar menerima koneksi dari WAN.

```bash
uci set firewall.@rule[-1]=rule
uci set firewall.@rule[-1].name='Allow-SSH-From-Any'
uci set firewall.@rule[-1].src='wan'
uci set firewall.@rule[-1].target='ACCEPT'
uci set firewall.@rule[-1].proto='tcp'
uci set firewall.@rule[-1].dest_port='22'
uci commit firewall
/etc/init.d/firewall restart
```

#### 4. **Mengaktifkan Masquerading pada WAN**

Masquerading memungkinkan perangkat di jaringan LAN untuk mengakses internet melalui WAN.

```bash
uci set firewall.@zone[1].masq='1'  # Pastikan ini diterapkan pada zona WAN
uci commit firewall
/etc/init.d/firewall restart
```

#### 5. **Menginstal dan Mengaktifkan LuCI (Web Configuration Interface)**

Instal LuCI untuk antarmuka web.

```bash
opkg update
opkg install luci
/etc/init.d/uhttpd start
/etc/init.d/uhttpd enable
```

#### 6. **Mengatur LuCI agar Bisa Diakses dari Jaringan Berbeda**

Untuk memungkinkan akses LuCI dari jaringan selain LAN, atur firewall agar menerima koneksi ke port web interface (biasanya 80 atau 443 untuk HTTPS).

```bash
uci set firewall.@rule[-1]=rule
uci set firewall.@rule[-1].name='Allow-HTTP-LuCI-From-Any'
uci set firewall.@rule[-1].src='wan'
uci set firewall.@rule[-1].target='ACCEPT'
uci set firewall.@rule[-1].proto='tcp'
uci set firewall.@rule[-1].dest_port='80'
uci commit firewall
/etc/init.d/firewall restart
```

#### Ringkasan

Dengan konfigurasi ini:

* **eth0** terhubung ke WAN dengan DHCP Client.
* **phy0-ap0** bertindak sebagai akses point dengan DHCP server di jaringan **192.168.100.1/24**.
* SSH dan LuCI bisa diakses dari jaringan WAN.
* **Masquerading** diaktifkan, memungkinkan akses internet dari perangkat yang terhubung ke **phy0-ap0**.

## Reset Config

Untuk mereset konfigurasi OpenWRT ke pengaturan pabrik, gunakan perintah berikut:

```bash
firstboot && reboot now
```

#### Penjelasan Singkat

* **firstboot**: Menghapus semua konfigurasi pengguna dan mengembalikan sistem ke pengaturan awal atau pabrik.
* **reboot now**: Merestart perangkat segera setelah reset selesai.

Setelah perintah ini dijalankan, OpenWRT akan kembali ke pengaturan default, dan semua konfigurasi yang telah dibuat sebelumnya akan dihapus.



## DNS Redirect

Untuk mengalihkan semua permintaan DNS melalui firewall di OpenWRT, Anda bisa menggunakan aturan firewall berbasis NAT untuk menangkap dan mengarahkan ulang permintaan DNS (biasanya pada port 53) ke server DNS lokal atau server DNS khusus yang Anda tentukan.

Berikut adalah langkah-langkah untuk mengalihkan semua permintaan DNS dari klien di jaringan ke server DNS tertentu (misalnya, `192.168.1.1`):

#### 1. **Menambahkan Aturan Redirect DNS pada Firewall**

Jalankan perintah `uci` untuk membuat aturan redirect DNS:

```bash
uci add firewall redirect
uci set firewall.@redirect[-1].name='Redirect-DNS'
uci set firewall.@redirect[-1].src='lan'  # Zona sumber
uci set firewall.@redirect[-1].src_dport='53'  # Port DNS (53)
uci set firewall.@redirect[-1].proto='tcp udp'  # Protokol TCP dan UDP
uci set firewall.@redirect[-1].target='DNAT'  # Target NAT
uci set firewall.@redirect[-1].dest_ip='192.168.1.1'  # IP DNS tujuan
uci set firewall.@redirect[-1].dest_port='53'  # Port tujuan (53)
```

Dengan aturan ini, semua permintaan DNS dari perangkat di zona `lan` akan diarahkan ke server DNS `192.168.1.1`.

#### 2. **Menerapkan dan Restart Firewall**

Setelah mengatur redirect DNS, terapkan perubahan dan restart firewall:

```bash
uci commit firewall
/etc/init.d/firewall restart
```

#### Contoh Lengkap

Sebagai contoh kita akan membuat LOW DNS.

DNS Jinom - Homenet

* Type : Low
* IP : 103.122.65.37
* Port : 531

Jika Anda ingin mengarahkan semua permintaan DNS klien jaringan ke server DNS `8.8.8.8` (Google DNS), gunakan konfigurasi berikut:

```bash
uci add firewall redirect
uci set firewall.@redirect[-1].name='Low DNS'
uci set firewall.@redirect[-1].src='lan'
uci set firewall.@redirect[-1].src_dport='53'
uci set firewall.@redirect[-1].proto='tcp udp'
uci set firewall.@redirect[-1].target='DNAT'
uci set firewall.@redirect[-1].dest_ip='103.122.65.37'
uci set firewall.@redirect[-1].dest_port='531'
uci commit firewall
/etc/init.d/firewall restart
```

#### Catatan

* Pastikan perangkat klien menggunakan DNS di jaringan LAN Anda agar aturan ini berfungsi.
* Pengalihan DNS ini membantu memastikan semua perangkat tetap menggunakan server DNS yang Anda tentukan, mengabaikan konfigurasi DNS manual pada perangkat.

### Advanced Setting

Untuk memblokir situs terlarang di OpenWRT menggunakan pengalihan DNS (`DNS Redirect`) dan mencegah end-user mengganti server DNS, Anda dapat melakukan beberapa konfigurasi berikut:

1.  **Mengatur DNS Redirect ke DNS yang Dipilih**

    Kita dapat menggunakan DNS server tertentu (misalnya, OpenDNS) yang memiliki fitur pemblokiran situs berdasarkan kategori, atau menggunakan server DNS internal untuk memblokir situs tertentu. Langkah-langkahnya adalah sebagai berikut:

    ```bash
    uci set dhcp.@dnsmasq[0].server='208.67.222.123'  # Masukkan IP DNS server
    uci set dhcp.@dnsmasq[0].noresolv='1'  # Abaikan resolusi DNS lainnya
    uci commit dhcp
    /etc/init.d/dnsmasq restart
    ```

    **Contoh:** `208.67.222.123` adalah server OpenDNS FamilyShield yang memblokir situs dewasa.
2.  **Mengalihkan Semua Permintaan DNS ke DNS Internal melalui Firewall**

    Agar pengguna tidak bisa menggunakan DNS selain yang telah diatur, buat aturan firewall yang mengarahkan semua lalu lintas DNS ke DNS server yang dipilih.

    ```bash
    uci add firewall redirect
    uci set firewall.@redirect[-1].name='Force-DNS'
    uci set firewall.@redirect[-1].src='lan'
    uci set firewall.@redirect[-1].src_dport='53'
    uci set firewall.@redirect[-1].proto='tcp udp'
    uci set firewall.@redirect[-1].target='DNAT'
    uci set firewall.@redirect[-1].dest_ip='208.67.222.123'  # Server DNS pilihan
    uci set firewall.@redirect[-1].dest_port='53'
    uci commit firewall
    /etc/init.d/firewall restart
    ```

    Dengan aturan ini, semua lalu lintas DNS dari perangkat yang terhubung di jaringan akan diarahkan ke server DNS `208.67.222.123` (contoh ini menggunakan OpenDNS FamilyShield).
3.  **Memblokir Langsung Situs Tertentu dengan `dnsmasq`**

    Jika Anda ingin memblokir situs tertentu tanpa mengandalkan server DNS eksternal, Anda bisa memblokirnya langsung dengan `dnsmasq` di OpenWRT:

    *   Tambahkan entri blokir di `/etc/hosts` untuk situs yang ingin diblokir.

        ```bash
        echo "127.0.0.1 situs-terlarang.com" >> /etc/hosts
        ```
    *   Atau, konfigurasi di `/etc/dnsmasq.conf` atau melalui UCI untuk domain tertentu:

        ```bash
        uci add_list dhcp.@dnsmasq[0].address='/situs-terlarang.com/127.0.0.1'
        uci commit dhcp
        /etc/init.d/dnsmasq restart
        ```

        Setiap permintaan ke `situs-terlarang.com` akan diarahkan ke `127.0.0.1` sehingga situs tidak dapat diakses.
4.  **Cegah Akses DNS ke Internet Langsung (Non-DNS Server Pilihan)**

    Untuk memastikan perangkat tidak bisa menggunakan DNS langsung ke internet, buat aturan firewall untuk memblokir semua lalu lintas DNS selain yang telah dialihkan:

    ```bash
    uci add firewall rule
    uci set firewall.@rule[-1].name='Block-External-DNS'
    uci set firewall.@rule[-1].src='lan'
    uci set firewall.@rule[-1].dest='wan'
    uci set firewall.@rule[-1].dest_port='53'
    uci set firewall.@rule[-1].proto='tcp udp'
    uci set firewall.@rule[-1].target='REJECT'
    uci commit firewall
    /etc/init.d/firewall restart
    ```

    Aturan ini akan mencegah lalu lintas DNS langsung ke internet, memaksa semua perangkat untuk menggunakan pengalihan DNS yang telah diset sebelumnya.

Dengan konfigurasi ini, OpenWRT akan mengarahkan semua permintaan DNS melalui server DNS yang Anda tetapkan, serta mencegah pengguna akhir untuk mengganti DNS mereka secara manual untuk melewati filter.

## Vlan

Untuk melakukan konfigurasi VLAN di OpenWRT, Anda dapat mengatur VLAN melalui antarmuka `Switch` di file konfigurasi `/etc/config/network`. Pada router OpenWRT yang mendukung fitur VLAN, biasanya ada bagian `switch` yang memungkinkan Anda mengatur port dan ID VLAN.

Berikut adalah langkah-langkah dasar untuk mengatur VLAN di OpenWRT:

#### 1. **Edit Konfigurasi Network**

Buka file konfigurasi `/etc/config/network` untuk mengedit pengaturan VLAN.

```bash
vi /etc/config/network
```

#### 2. **Menambahkan Switch VLAN**

Tambahkan atau sesuaikan bagian `switch_vlan` di file konfigurasi. Misalnya, konfigurasi berikut membuat dua VLAN:

* **VLAN 10** untuk jaringan LAN.
* **VLAN 20** untuk jaringan Guest.

```plaintext
config switch 'switch0'
    option name 'switch0'
    option reset '1'
    option enable_vlan '1'

config switch_vlan
    option device 'switch0'
    option vlan '10'
    option ports '1 2 3 4 6t'  # Port 1-4 untuk VLAN 10, port 6 sebagai tagged

config switch_vlan
    option device 'switch0'
    option vlan '20'
    option ports '0 6t'  # Port 0 untuk VLAN 20, port 6 sebagai tagged
```

Pada contoh ini:

* **`switch0`** adalah nama switch pada router.
* **`ports`** menentukan port yang terhubung ke VLAN tertentu.
* **`6t`** berarti port 6 diatur sebagai tagged. Biasanya ini adalah port yang terhubung ke CPU atau perangkat yang mendukung VLAN.

#### 3. **Menambahkan Interface untuk VLAN**

Sekarang, tambahkan interface baru untuk setiap VLAN agar bisa digunakan oleh jaringan.

```plaintext
config interface 'lan'
    option type 'bridge'
    option proto 'static'
    option ipaddr '192.168.10.1'
    option netmask '255.255.255.0'
    option ifname 'eth0.10'  # VLAN 10 pada interface eth0

config interface 'guest'
    option type 'bridge'
    option proto 'static'
    option ipaddr '192.168.20.1'
    option netmask '255.255.255.0'
    option ifname 'eth0.20'  # VLAN 20 pada interface eth0
```

* **`eth0.10`** merujuk pada interface `eth0` dengan VLAN ID 10.
* **`eth0.20`** merujuk pada interface `eth0` dengan VLAN ID 20.

#### 4. **Simpan Konfigurasi dan Restart Network**

Setelah selesai mengedit file konfigurasi, simpan perubahan dan restart layanan jaringan untuk menerapkan pengaturan.

```bash
/etc/init.d/network restart
```

#### Contoh Lengkap Konfigurasi

```plaintext
config switch 'switch0'
    option name 'switch0'
    option reset '1'
    option enable_vlan '1'

config switch_vlan
    option device 'switch0'
    option vlan '10'
    option ports '1 2 3 4 6t'

config switch_vlan
    option device 'switch0'
    option vlan '20'
    option ports '0 6t'

config interface 'lan'
    option type 'bridge'
    option proto 'static'
    option ipaddr '192.168.10.1'
    option netmask '255.255.255.0'
    option ifname 'eth0.10'

config interface 'guest'
    option type 'bridge'
    option proto 'static'
    option ipaddr '192.168.20.1'
    option netmask '255.255.255.0'
    option ifname 'eth0.20'
```

Dengan pengaturan di atas, Anda akan memiliki dua VLAN di OpenWRT:

* VLAN 10 sebagai jaringan `LAN` dengan subnet `192.168.10.0/24`
* VLAN 20 sebagai jaringan `Guest` dengan subnet `192.168.20.0/24`



## Wireless Config

### Enable dual band mode

Untuk mengatur OpenWRT agar bekerja dalam dua mode sekaligus, yaitu 5GHz dan 2.4GHz, pastikan bahwa perangkat Anda mendukung dual-band WiFi. Langkah-langkah berikut dapat digunakan untuk mengonfigurasi kedua band secara bersamaan:

1.  **Buka Konfigurasi Wireless di OpenWRT**

    Akses file konfigurasi WiFi di OpenWRT:

    ```bash
    vi /etc/config/wireless
    ```
2.  **Konfigurasi untuk Band 2.4GHz**

    Tambahkan atau sesuaikan bagian `wifi-device` dan `wifi-iface` untuk jaringan 2.4GHz seperti berikut:

    ```plaintext
    config wifi-device 'radio0'
        option type 'mac80211'
        option hwmode '11g'  # Mode untuk 2.4GHz
        option path 'platform/xxxxxx'  # Ganti dengan path perangkat Anda
        option channel '6'  # Pilih saluran untuk 2.4GHz
        option htmode 'HT20'
        option disabled '0'

    config wifi-iface 'default_radio0'
        option device 'radio0'
        option network 'lan'
        option mode 'ap'
        option ssid 'OpenWrt-2.4GHz'
        option encryption 'psk2'
        option key 'password24ghz'
    ```

    **Penjelasan:**

    * **`radio0`** adalah interface untuk jaringan 2.4GHz.
    * **`htmode 'HT20'`** digunakan untuk koneksi 2.4GHz.
    * **`ssid`** adalah nama jaringan untuk band 2.4GHz (ubah sesuai kebutuhan).
    * **`encryption`** mengatur enkripsi dan **`key`** untuk password WiFi.
3.  **Konfigurasi untuk Band 5GHz**

    Sekarang tambahkan bagian lain untuk jaringan 5GHz:

    ```plaintext
    config wifi-device 'radio1'
        option type 'mac80211'
        option hwmode '11a'  # Mode untuk 5GHz
        option path 'platform/yyyyyy'  # Ganti dengan path perangkat Anda
        option channel '36'  # Pilih saluran untuk 5GHz
        option htmode 'VHT80'
        option disabled '0'

    config wifi-iface 'default_radio1'
        option device 'radio1'
        option network 'lan'
        option mode 'ap'
        option ssid 'OpenWrt-5GHz'
        option encryption 'psk2'
        option key 'password5ghz'
    ```

    **Penjelasan:**

    * **`radio1`** adalah interface untuk jaringan 5GHz.
    * **`htmode 'VHT80'`** digunakan untuk kecepatan lebih tinggi di jaringan 5GHz.
    * **`ssid`** adalah nama jaringan untuk band 5GHz.
    * **`encryption`** dan **`key`** untuk mengatur keamanan.
4.  **Simpan dan Restart Wireless**

    Simpan konfigurasi dan restart layanan jaringan untuk menerapkan perubahan:

    ```bash
    /etc/init.d/network restart
    ```
5.  **Verifikasi Jaringan**

    Pastikan bahwa kedua jaringan, 2.4GHz dan 5GHz, muncul pada perangkat Anda dengan SSID yang telah ditetapkan, dan bahwa keduanya dapat diakses dengan baik.

Dengan konfigurasi ini, OpenWRT akan menyediakan jaringan WiFi di kedua band, memungkinkan perangkat terhubung ke salah satu sesuai dukungan masing-masing.
