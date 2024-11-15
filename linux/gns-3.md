---
icon: diagram-project
description: >-
  GNS 3 merupakan sebuah aplikasi Open Source yang digunakan untuk melakukan
  simulasi jaringan secara virtual. GNS 3 dapat menjalankan berbagai macam
  perangkat jaringan dan os jaringan secara virtual.
---

# GNS 3

## Instalasi

### Add Repo

Add and update repo,

```bash
sudo add-apt-repository ppa:gns3/ppa
sudo apt update                                
sudo apt install gns3-gui gns3-server
```

### Install Docker

Remove Old Docker

```bash
sudo apt remove docker docker-engine docker.io
```

### Install New Docker

```bash
sudo apt-get install apt-transport-https ca-certificates curl \ software-properties-common
```

### Import Docker Key

Gpg Key

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

```bash
sudo add-apt-repository \
"deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) stable"
```

### Install Docker CE

```bash
sudo apt update
sudo apt install docker-ce
```

Change Usermod and Group of following Application

```bash
ubridge libvirt kvm wireshark docker
```

Change User mode

```bash
sudo usermod -aG [groupname] [username] 

#example : 
# sudo usermod -aG ubridge fredikstefan
# sudo usermod -aG kvm fredikstefan

```

### Install  Virtual Bridge Network

```bash
sudo apt-get install bridge-utils
```

### Install Libvirt

Libvirt merupakan library yang  dibutuhkan untuk menjalankan virtual network pada gns3 sepert NAT atau Bridge network pada GNS 3.

```bash
sudo apt update
sudo apt install qemu-kvm libvirt-daemon-system
```

Modifikasi User Group Untuk Libvirt

```bash
sudo adduser $USER libvirt
```

### Config Virsh

Ini berguna untuk mengaktifkan virtual netowork adapter dan dapat berjalan otomatis

Cek Status Network Virtual Adapter

```bash
 virsh net-list --all
```

Jika masih disable atau nonaktif. Silahkan aktifkan dengan perintah berikut :&#x20;

```bash
virsh net-start default
virsh net-autostart default
```

## Uninstall

To cleanly uninstall GNS3 on Linux (Xubuntu), follow these steps:

1.  **Remove GNS3 using `apt`**: Open a terminal and run the following command to uninstall GNS3:

    ```bash
    sudo apt-get remove --purge gns3 gns3-server gns3-gui
    ```
2.  **Remove Dependencies**: If you want to remove any unused dependencies that were installed with GNS3, run:

    ```bash
    sudo apt-get autoremove
    ```
3.  **Delete Configuration Files and Directories**: GNS3 may leave behind configuration files or directories. To remove them, use the following commands:

    ```bash
    rm -rf ~/.gns3
    rm -rf ~/.config/gns3
    rm -rf /etc/gns3
    ```
4.  **Optional: Remove Docker (if installed with GNS3)**: If you installed Docker through GNS3 and wish to remove it as well, run:

    ```bash
    sudo apt-get purge docker-ce docker-ce-cli containerd.io
    sudo apt-get autoremove --purge
    ```
5.  **Clear Any Remaining Dependencies**: Run the following command to clean up any residual package files:

    ```bash
    sudo apt-get clean
    ```

After completing these steps, GNS3 should be completely uninstalled from your system.

## Mengeluarkan koneksi dari GNS3 ke interface fisik

#### Langkah 1: Konfigurasi Interface Laptop

1. **Pastikan interface `eth0` di laptop terhubung ke jaringan LAN** yang digunakan oleh akses point.
2.  **Set IP address pada interface `eth0`** jika belum:

    ```shell
    sudo ip addr add 192.168.1.1/24 dev eth0
    sudo ip link set dev eth0 up
    ```

#### Langkah 2: Konfigurasi Tap Interface untuk GNS3

1.  **Buat interface TAP untuk menghubungkan GNS3 ke jaringan fisik**:

    ```shell
    sudo ip tuntap add dev tap0 mode tap user $(whoami)
    sudo ip link set dev tap0 up
    ```
2.  **Bridge interface `eth0` dengan `tap0`**:

    ```shell
    sudo brctl addbr br0
    sudo brctl addif br0 eth0
    sudo brctl addif br0 tap0
    sudo ip link set dev br0 up
    ```

#### Langkah 3: Konfigurasi GNS3

1. **Tambahkan interface cloud di GNS3** dan hubungkan ke `tap0` (atau `br0` jika kamu membuat bridge).
2. **Hubungkan router OpenWrt di GNS3 ke interface cloud**. Misalnya, jika kamu menggunakan router OpenWrt di GNS3:
   * Klik pada router OpenWrt, pilih interface yang terhubung ke jaringan fisik (`eth0` atau lainnya).
   * Hubungkan ke interface cloud yang telah dibuat (menggunakan `tap0` atau `br0`).

#### Langkah 4: Konfigurasi Router OpenWrt di GNS3

1. **Masuk ke antarmuka konfigurasi OpenWrt**:
   * Gunakan terminal atau LuCI (web interface) untuk mengonfigurasi router OpenWrt.
2.  **Konfigurasi interface yang terhubung ke tap0** dengan pengaturan IP yang sesuai dengan jaringan LAN kamu. Misalnya:

    ```shell
    uci set network.lan.ipaddr='192.168.1.2'
    uci set network.lan.netmask='255.255.255.0'
    uci commit network
    /etc/init.d/network restart
    ```
3. **Konfigurasi layanan yang diinginkan** pada router OpenWrt (misalnya, DHCP, firewall, NAT, dll.).

#### Langkah 5: Uji Koneksi

1. **Pastikan semua perangkat yang terhubung ke akses point** bisa mendapatkan IP address dan layanan yang diatur oleh OpenWrt di GNS3.
2.  **Uji koneksi** dengan mencoba ping dari perangkat klien ke OpenWrt dan sebaliknya:

    ```shell
    ping 192.168.1.2
    ```

#### Contoh Topologi

1. **Laptop (eth0)** ↔ **Bridge (br0)** ↔ **Tap Interface (tap0)** ↔ **GNS3 Cloud** ↔ **Router OpenWrt di GNS3**
2. **Router OpenWrt di GNS3** mengelola trafik dari klien yang terhubung ke akses point dan menyalurkan layanan melalui interface fisik `eth0` di laptop.

Dengan langkah-langkah ini, kamu dapat membuat topologi jaringan yang memenuhi kriteria yang kamu inginkan. Jika ada pertanyaan lebih lanjut atau butuh bantuan tambahan, jangan ragu untuk bertanya! 😊
