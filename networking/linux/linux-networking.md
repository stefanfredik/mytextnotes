# Linux Networking

## Intall NetWork Tools

```bash
apt install net-tools
```

Check Network Interfaces

```bash
sudo ifconfig -a
```

## Ifconfig

Enable Network Intefaces

```bash
ifconfig [interfaces name] up

or

ifup [interfaces name]
```

Disable Network Intefaces

```bash
ifconfig [interfaces name] down

or

ifdown [interfaces name]
```

Refresh Dhcp Client

```bash
sudo dhcpclient
```

Renew Dhcp Client

```bash
sudo dhclient -r
```

```bash
sudo dhclient -r [interface name]

or

sudo dhclient [interface name]
```

Assign/ Add Ip address to interfaces

```bash
ifconfig eth0 172.18.26.124
ifconfig eth0 netmask 255.255.255.224
ifconfig eth0 broadcast 172.18.26.63
```

```bash
ifconfig eth0172.18.26.124 netmask 255.255.255.224 broadcast 172.18.26.63
```

Config MTU

```bash
ifconfig [interface-name] mtu [mtu-value]
```

Change Mac Address

```bash
ifconfig eth0 hw ether AA:BB:CC:DD:EE:FF
```

Start DHCP Client

```bash
ifconfig [inetrface] dhcp start
```

## Routing

Di Linux kita dapat melihat dan mengelolah Routing menggunakan beberapa tool atau command. Beberpa tools yang bisa digunakan adalah :&#x20;

* IP Route
* Ifmetric
* Netstat

### IP Route

#### Menambah IP Route

Add Routing Table

```bash
ip route add [ip network] via [ip gateway]

#Example ::
# ip route add 10.10.10.0/24 via 192.168.1.1
```

#### Menghapus Rute Tertentu

Untuk menghapus satu rute tertentu, gunakan perintah :

```bash
sudo ip route del <network>/<prefix> via <gateway> dev <interface

# Example :
# sudo ip route del 192.168.1.0/24 via 192.168.1.1 dev eth0
```

#### Menghapus Semua Rute

Jika kamu ingin mereset seluruh rute sekaligus, kamu bisa menggunakan script sederhana atau melakukannya secara manual. Sayangnya, perintah langsung untuk menghapus semua rute tidak tersedia secara eksplisit, tetapi kamu dapat menghapus semua rute dengan cara berikut:

**Menghapus Semua Rute IPv4:**

Gunakan perintah berikut untuk menghapus seluruh rute IPv4:

```bash
sudo ip route flush table main
```

**Menghapus Semua Rute IPv6:**

Jika menggunakan IPv6, gunakan perintah ini untuk mereset rute IPv6:

```bash
sudo ip -6 route flush table main
```

#### Menampilkan Routing Table

Untuk menampilkan routing table dapat menggunakan  beberapa cara :&#x20;

**IP Route**

*   Untuk IPv4:

    ```bash
    ip route show
    ```
*   Untuk IPv6:

    ```bash
    ip -6 route show
    ```



**Netstat**

```basic
netstat -rn
```

### &#x20;Mengubah IP Metric

IP metric adalah nilai table routing yang akan diprioritaskan oleh Linux.



**Mengganti IP Metric dengan `ip route`**

Cara ini memungkinkan kamu mengubah metric secara langsung menggunakan perintah `ip route`.

**Langkah 1: Hapus Rute Lama (Opsional)**

Jika kamu ingin mengganti metric pada rute yang sudah ada, kamu perlu menghapus rute yang lama terlebih dahulu.&#x20;

```bash
sudo ip route del default via 192.168.1.1 dev wlo1
```

**Langkah 2: Tambah Rute Baru dengan Metric**

Setelah rute lama dihapus, tambahkan rute baru dengan metric yang diinginkan. Contoh:

```bash
sudo ip route add default via 192.168.1.1 dev wlo1 metric 100
```

Penjelasan:

* **default**: Menunjukkan bahwa ini adalah rute default.
* **via 192.168.1.1**: Gateway untuk rute ini.
* **dev wlo1**: Interface jaringan yang digunakan (misalnya, Wi-Fi).
* **metric 100**: Menentukan nilai metric baru (nilai lebih rendah berarti prioritas lebih tinggi).

Jika kamu tidak ingin menghapus rute lama, tetapi hanya ingin menambahkan rute baru dengan metric lebih rendah, kamu bisa langsung menggunakan perintah **ip route add**.



#### 2. Menggunakan `ifmetric` untuk Mengatur Metric Interface

Metode lain untuk mengubah metric interface jaringan adalah menggunakan tool **ifmetric**. Tool ini memungkinkan kamu mengatur metric untuk interface secara permanen, sehingga tetap ada meski setelah reboot.

**Langkah 1: Instal `ifmetric`**

Jika `ifmetric` belum terinstal, kamu bisa menginstalnya terlebih dahulu.

**Pada Debian/Ubuntu:**

```bash
sudo apt update
sudo apt install ifmetric
```

**Pada CentOS/RHEL (Instalasi manual mungkin dibutuhkan):**

Jika `ifmetric` tidak tersedia di repositori, kamu mungkin perlu menginstalnya secara manual atau menggunakan alternatif `ip route`.

**Langkah 2: Set Metric dengan `ifmetric`**

Setelah diinstal, kamu bisa mengatur metric untuk interface yang diinginkan. Misalnya, untuk interface `wlo1`:

```bash
sudo ifmetric wlo1 100
```

Penjelasan:

* **wlo1**: Nama interface.
* **100**: Nilai metric yang ingin ditetapkan.

**Verifikasi Perubahan**

Setelah mengatur metric, kamu bisa memverifikasi dengan perintah berikut:

```bash
ip route
```

Ini akan menunjukkan tabel rute, termasuk metric yang sudah diatur dengan `ifmetric`.

#### 3. Menyimpan Perubahan Secara Permanen

Jika kamu ingin metric ini tetap ada setelah reboot, kamu perlu mengedit file konfigurasi network.

**Untuk sistem berbasis Debian/Ubuntu:**

1.  Edit file `/etc/network/interfaces` dan tambahkan opsi `metric` pada interface yang diinginkan.

    Contoh konfigurasi untuk interface `wlo1`:

    ```bash
    auto wlo1
    iface wlo1 inet dhcp
        metric 100
    ```
2.  Restart jaringan agar perubahan diterapkan:

    ```bash
    sudo systemctl restart networking
    ```

**Untuk sistem berbasis CentOS/RHEL:**

1.  Edit file konfigurasi di `/etc/sysconfig/network-scripts/ifcfg-ethX` (sesuaikan `ethX` dengan nama interface yang ingin diatur), tambahkan opsi `METRIC`.

    Contoh:

    ```bash
    DEVICE=wlo1
    BOOTPROTO=dhcp
    ONBOOT=yes
    METRIC=100
    ```
2.  Restart jaringan:

    ```bash
    sudo systemctl restart network
    ```

#### Kesimpulan

1. **Dengan `ip route`**: Kamu bisa secara dinamis mengubah metric rute saat ini.
2. **Dengan `ifmetric`**: Kamu bisa mengatur metric untuk interface jaringan dan membuatnya permanen.
3. **Edit file konfigurasi jaringan** untuk menyimpan pengaturan metric secara permanen pada interface tertentu.

### Firewall

Check Firewall Server Status

```bash
ufw app list
```

Activate Firewall

```bash
ufw enable
```

Check Firewall Status

```bash
ufw status
```

Hostname Check List - hostname -I Firewall Allow Traffic

```bash
ufw allow [network service]
```

Example :

```bash
ufw allow 'Apache2'
```

Firewall Check Status

```bash
ufw status
```

### DNS Server

Add Custom DNS Server

```bash
nano /etc/resolv.conf

nameserver 8.8.8.8
```

## IP Address

### Hapus IP Address

Untuk menghapus IP address pada Linux, kamu bisa menggunakan perintah `ip` dengan opsi `del`. Berikut adalah sintaks dasar untuk menghapus IP address dari suatu interface:

```bash
sudo ip addr del <IP_address>/<prefix_length> dev <interface_name>
```

Contoh: Jika kamu ingin menghapus IP address `192.168.1.10` dengan prefix `/24` dari interface `eth0`, perintahnya adalah:

```bash
sudo ip addr del 192.168.1.10/24 dev eth0
```

Penjelasan:

* `ip addr del` digunakan untuk menghapus IP address.
* `<IP_address>/<prefix_length>` adalah IP address yang ingin dihapus beserta subnet mask.
* `dev <interface_name>` merujuk pada interface jaringan tempat IP address tersebut dihapus (misalnya `eth0`, `ens33`, dll.).
