# Proxmox

## Basic Command

Berikut adalah perintah lengkap menggunakan **Proxmox CLI (Command Line Interface)** untuk berbagai operasi umum, seperti membuat, mengelola, dan menghapus VM dan container LXC, serta operasi node dan storage.

***

### 🔧 **1. Dasar CLI Proxmox**

Proxmox VE menyediakan utilitas CLI seperti:

* `qm` – untuk VM (QEMU)
* `pct` – untuk container LXC
* `pvesm` – untuk manajemen storage
* `systemctl` – untuk restart layanan
* `pvecm` – untuk manajemen cluster
* `qm list` – daftar semua VM
* `pct list` – daftar semua container

***

### 🧱 **2. Operasi VM (Menggunakan `qm`)**

#### ✅ Membuat VM

```bash
qm create 100 --name vm100 --memory 2048 --cores 2 --net0 virtio,bridge=vmbr0
```

> Membuat VM dengan ID 100, nama vm100, RAM 2GB, 2 core CPU, dan NIC di bridge vmbr0.

#### 📦 Menambahkan Disk

```bash
qm set 100 --scsi0 local-lvm:vm-100-disk-0
```

> Menambahkan disk SCSI ke VM.

#### 📀 Menambahkan ISO

```bash
qm set 100 --cdrom local:iso/debian-12.0.iso
```

#### ⏯️ Memulai VM

```bash
qm start 100
```

#### ⏹️ Menghentikan VM

```bash
qm stop 100
```

#### 🔁 Restart VM

```bash
qm reboot 100
```

#### ❌ Menghapus VM

```bash
qm destroy 100
```

***

### 📦 **3. Operasi Container LXC (Menggunakan `pct`)**

#### ✅ Membuat Container

```bash
pct create 200 local:vztmpl/debian-12-standard_12.0-1_amd64.tar.zst --hostname container200 --memory 1024 --cores 1 --net0 name=eth0,ip=dhcp,bridge=vmbr0
```

> Membuat container LXC dengan ID 200 dari template Debian.

#### ⏯️ Memulai Container

```bash
pct start 200
```

#### ⏹️ Menghentikan Container

```bash
pct stop 200
```

#### 🔁 Restart Container

```bash
pct reboot 200
```

#### 📄 Melihat Konfigurasi Container

```bash
pct config 200
```

#### ❌ Menghapus Container

```bash
pct destroy 200
```

***

### 💾 **4. Manajemen Storage (`pvesm`)**

#### 📦 Daftar Storage

```bash
pvesm status
```

#### 📁 Membuat Direktori Storage

```bash
mkdir /mnt/pve/my-storage
```

#### 🧩 Menambahkan Storage Lokal

```bash
pvesm add dir my-storage -path /mnt/pve/my-storage
```

#### ❌ Menghapus Storage

```bash
pvesm remove my-storage
```

***

### 🌐 **5. Manajemen Jaringan**

#### 🔄 Restart Service Jaringan

```bash
systemctl restart networking
```

#### 🧾 Melihat Konfigurasi Jaringan

```bash
cat /etc/network/interfaces
```

#### 🧪 Restart Bridge

```bash
ifdown vmbr0 && ifup vmbr0
```

***

### 🧑‍💼 **6. Manajemen Pengguna**

#### ✅ Menambahkan User

```bash
pveum user add john@pve
```

#### 🔐 Menambahkan Password untuk User

```bash
pveum passwd john@pve
```

#### 🚫 Menghapus User

```bash
pveum user delete john@pve
```

***

### 🧱 **7. Cluster Management**

#### ✅ Membuat Cluster

```bash
pvecm create mycluster
```

#### ➕ Menambahkan Node ke Cluster

```bash
pvecm add <ip-node-utama>
```

#### 📋 Melihat Status Cluster

```bash
pvecm status
```

***

### 🧪 **8. Backup & Restore**

#### 💾 Backup VM

```bash
vzdump 100 --storage local
```

#### 🔁 Restore VM dari Backup

```bash
qmrestore /var/lib/vz/dump/vzdump-qemu-100-2025-04-05.tar local
```

***

### 🧹 **9. Utilitas Lainnya**

#### 📋 Melihat Daftar VM

```bash
qm list
```

#### 📋 Melihat Daftar Container

```bash
pct list
```

#### 🧮 Melihat Penggunaan Resource

```bash
top
```

atau

```bash
htop
```

#### 🧾 Melihat Log Sistem

```bash
journalctl -x
```

***

### 📌 Catatan Penting

* **ID VM/Container** harus unik.
* Pastikan storage tersedia sebelum membuat VM/Container.
* Pastikan template tersedia di `/var/lib/vz/template/cache/` untuk container.

***

Jika kamu ingin perintah spesifik sesuai kebutuhan, beri tahu saja (misalnya: migrasi VM, clone VM, snapshot, dll), saya bisa bantu lebih lanjut!



## Command

**Panduan Lengkap Perintah Proxmox VE CLI**

***

#### 🔍 Informasi Sistem

```bash
pveversion               # Menampilkan versi Proxmox saat ini
pveversion --verbose     # Versi lengkap beserta kernel dan paket
```

```bash
pvesubscription get      # Cek status subscription
pvebanner                # Cek status lisensi di CLI
```

```bash
pveperf                  # Cek performa CPU, disk, RAM
```

***

#### 🌐 Jaringan

```bash
ip a                     # Lihat konfigurasi IP
ip r                     # Lihat routing table
bridge-utils             # Perlu untuk manajemen bridge (jika belum terinstall)
brctl show               # Lihat bridge yang aktif
```

***

#### 📁 Manajemen VM (KVM)

**🔄 Daftar & Status VM**

```bash
qm list                  # Menampilkan semua VM
qm status <vmid>         # Cek status VM
```

**▶️ Kontrol VM**

```bash
qm start <vmid>          # Menyalakan VM
qm stop <vmid>           # Mematikan VM paksa
qm shutdown <vmid>       # Shutdown VM normal
qm reboot <vmid>         # Reboot VM
```

**🔁 Migrasi VM**

```bash
qm migrate <vmid> <target-node> --online --with-local-disks
```

**🔖 Backup & Restore VM**

```bash
vzdump <vmid> -storage <storage-name> -mode snapshot
qmrestore /path/to/backup.vma.gz <vmid>
```

**📄 Konfigurasi VM**

```bash
qm config <vmid>         # Tampilkan konfigurasi VM
qm set <vmid> -name NEWNAME    # Ganti nama VM
qm set <vmid> -memory 2048     # Ubah RAM menjadi 2GB
qm resize <vmid> sata0 +10G    # Tambah disk
```

***

#### 🪑 Manajemen LXC Container

```bash
pct list                 # Daftar container
pct start <vmid>
pct stop <vmid>
pct enter <vmid>         # Masuk ke container shell
pct migrate <vmid> <node>
```

***

#### 📅 Storage

```bash
pvesm status             # Status semua storage
pvesm list <storage>     # Lihat isi storage tertentu
```

***

#### 🧬 Cluster Management

```bash
pvecm status             # Status cluster
pvecm nodes              # Daftar node dalam cluster
pvecm add <ip>           # Tambahkan node ke cluster
pvecm delnode <name>     # Hapus node dari cluster
```

***

#### 📙 Repository dan Update

```bash
apt update && apt full-upgrade
```

```bash
pve7to8                  # Cek kompatibilitas upgrade ke Proxmox 8
```

***

#### ⚙️ Utilitas Penting

```bash
journalctl -xe           # Lihat log error
systemctl status pvedaemon.service   # Cek status service Proxmox
systemctl restart networking         # Restart jaringan
```

***

#### 🔢 Perintah Tambahan

```bash
qm unlock <vmid>         # Unlock VM yang stuck
qm destroy <vmid>        # Hapus VM
```

***

#### 🔧 Debug & Maintenance

```bash
lsblk                    # Lihat disk dan partisi
df -h                    # Cek penggunaan disk
```

Untuk panduan lebih lanjut, gunakan dokumentasi resmi Proxmox: [https://pve.proxmox.com/wiki/Main\_Page](https://pve.proxmox.com/wiki/Main_Page)



## Upgrade Versi Proxmox

16 Juli 2025 - Dibuat Oleh Grok AI

## Panduan Lengkap Upgrade Proxmox VE ke Versi Terbaru (8.4)

Panduan ini memberikan langkah-langkah terperinci untuk mengupgrade Proxmox Virtual Environment (VE) ke versi terbaru, yaitu versi 8.4 pada Juli 2025. Panduan ini mencakup dua skenario utama: upgrade dari versi 7 ke 8 dan update dari versi 8.x ke 8.4. Proxmox VE adalah platform virtualisasi open-source berbasis Debian yang mendukung KVM dan LXC, dan upgrade ke versi terbaru memastikan Anda mendapatkan fitur terbaru, perbaikan bug, dan pembaruan keamanan.

### 1. Persiapan

Sebelum memulai proses upgrade, pastikan Anda memenuhi persyaratan berikut untuk menghindari masalah:

| **Persyaratan**           | **Detail**                                                                                                                                                              |
| ------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Cadangan Data**         | Buat cadangan lengkap semua VM, container, dan konfigurasi sistem. Gunakan alat seperti Proxmox Backup Server atau solusi eksternal.                                    |
| **Versi Awal**            | Untuk upgrade dari versi 7, pastikan Anda menggunakan Proxmox VE 7.4 dengan `pve-manager` versi minimal `7.4-18`. Jalankan `pveversion` untuk memeriksa.                |
| **Ceph (Jika Digunakan)** | Jika menggunakan Ceph hyper-converged, upgrade ke Ceph 17.2 Quincy terlebih dahulu. Lihat panduan: [Ceph Upgrade](https://pve.proxmox.com/wiki/Ceph_Pacific_to_Quincy). |
| **Akses Node**            | Pastikan akses andal ke node melalui iKVM/IPMI, akses fisik, atau SSH. Tes SSH pada mesin non-produksi jika memungkinkan.                                               |
| **Kesehatan Cluster**     | Pastikan cluster (jika digunakan) dalam keadaan sehat tanpa masalah kritis.                                                                                             |
| **Ruang Disk**            | Pastikan ada minimal 5 GB ruang kosong pada mount point root (`df -h /`).                                                                                               |
| **Repositori**            | Pastikan repositori dikonfigurasi dengan benar (enterprise atau no-subscription).                                                                                       |

**Catatan**: Selalu periksa isu-isu yang diketahui di [Proxmox Roadmap](https://pve.proxmox.com/wiki/Roadmap#8.0-known-issues) sebelum memulai.

### 2. Upgrade dari Proxmox VE 7 ke 8

Jika Anda menggunakan Proxmox VE versi 7, ikuti langkah-langkah berikut berdasarkan dokumentasi resmi: [Upgrade from 7 to 8](https://pve.proxmox.com/wiki/Upgrade_from_7_to_8).

#### **Langkah-Langkah**

**a. Tes Upgrade**

* Lakukan uji coba upgrade pada server standalone atau VM dengan Proxmox VE 7.4 untuk mereplikasi lingkungan produksi. Instal ISO Proxmox VE 7.4, perbarui ke versi terbaru, dan lakukan upgrade ke versi 8.

**b. Gunakan Script pve7to8**

*   Jalankan perintah berikut untuk memeriksa potensi masalah:

    ```
    # pve7to8
    ```

    atau gunakan opsi lengkap:

    ```
    # pve7to8 --full
    ```
* Perbaiki masalah yang ditemukan dan jalankan ulang script hingga tidak ada isu kritis.

**c. Pindahkan VM dan Container**

* Migrasikan semua VM dan container yang berjalan ke node lain dalam cluster untuk menghindari gangguan. Catatan: Migrasi dari versi lama ke baru selalu berhasil, tetapi sebaliknya mungkin tidak.

**d. Perbarui Repositori APT**

1.  Pastikan sistem sudah menggunakan paket terbaru untuk versi 7.4:

    ```
    # apt update
    # apt dist-upgrade
    # pveversion
    ```

    (Pastikan output menunjukkan `7.4-15` atau lebih baru).
2.  Ganti repositori Debian dari Bullseye ke Bookworm:

    ```
    # sed -i 's/bullseye/bookworm/g' /etc/apt/sources.list
    ```
3. Tambahkan repositori Proxmox VE 8:
   *   Untuk repositori enterprise:

       ```
       # echo "deb https://enterprise.proxmox.com/debian/pve bookworm pve-enterprise" > /etc/apt/sources.list.d/pve-enterprise.list
       ```
   *   Untuk repositori no-subscription (jika tidak memiliki langganan):

       ```
       # echo "deb http://download.proxmox.com/debian/pve bookworm pve-no-subscription" > /etc/apt/sources.list.d/pve-no-subscription.list
       ```
4. Jika menggunakan Ceph hyper-converged, perbarui repositori Ceph:
   *   Untuk enterprise:

       ```
       # echo "deb https://enterprise.proxmox.com/debian/ceph-quincy bookworm enterprise" > /etc/apt/sources.list.d/ceph.list
       ```
   *   Untuk no-subscription:

       ```
       # echo "deb http://download.proxmox.com/debian/ceph-quincy bookworm no-subscription" > /etc/apt/sources.list.d/ceph.list
       ```
5. Hapus baris backports (jika ada) dari `/etc/apt/sources.list`.
6.  Segarkan repositori:

    ```
    # apt update
    ```

**e. Upgrade Sistem**

*   Jalankan perintah upgrade:

    ```
    # apt dist-upgrade
    ```
* Perhatikan perubahan konfigurasi pada file berikut:
  * `/etc/issue`: Pilih "No" untuk mempertahankan konfigurasi saat ini.
  * `/etc/lvm/lvm.conf`: Pilih "Yes" jika tidak ada perubahan khusus.
  * `/etc/ssh/sshd_config`: Pilih "Yes" jika hanya ada perubahan pada `ChallengeResponseAuthentication`.
  * `/etc/default/grub`: Pilih "No" jika tidak yakin.
* Jika diminta, periksa perubahan dengan hati-hati sebelum mengonfirmasi.

**f. Pasca-Upgrade**

*   Reboot sistem untuk menggunakan kernel baru:

    ```
    # reboot
    ```
* Kosongkan cache browser untuk Web UI (tekan `CTRL + SHIFT + R` atau `⌘ + Alt + R` pada MacOS).
* Jika menggunakan cluster, ulangi proses untuk setiap node setelah memastikan node pertama berhasil diupgrade.

### 3. Update dari Proxmox VE 8.x ke Versi Terbaru (8.4)

Jika Anda sudah menggunakan Proxmox VE versi 8.x (misalnya 8.0 atau 8.2), Anda dapat memperbarui ke versi terbaru (8.4) menggunakan perintah standar update, karena Proxmox menggunakan model rilis rolling untuk versi minor.

#### **Langkah-Langkah**

1.  Perbarui sistem:

    ```
    # apt-get update
    # apt-get dist-upgrade
    ```
2.  Reboot sistem setelah update selesai:

    ```
    # reboot
    ```

#### **Catatan Tambahan**

* **Repositori Enterprise**: Jika Anda mendapatkan kesalahan 401 Unauthorized, nonaktifkan repositori enterprise melalui GUI:
  * Buka **Server -> Update -> Repository**.
  * Nonaktifkan "Proxmox enterprise repo".
  *   Tambahkan repositori no-subscription:

      ```
      # echo "deb http://download.proxmox.com/debian/pve bookworm pve-no-subscription" > /etc/apt/sources.list.d/pve-no-subscription.list
      ```
* **VM dan Container**: Matikan atau pindahkan VM/container ke node lain sebelum update untuk menghindari gangguan.
* **Cluster**: Jangan tinggalkan cluster selama proses update. Pastikan semua node diperbarui secara berurutan.

### 4. Catatan Penting

| **Aspek**              | **Detail**                                                                                                                                                  |
| ---------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Backup**             | Selalu lakukan backup sebelum upgrade untuk mencegah kehilangan data.                                                                                       |
| **Repositori**         | Gunakan repositori no-subscription jika tidak memiliki langganan enterprise.                                                                                |
| **Cluster**            | Upgrade node satu per satu dalam cluster, pastikan node tetap dalam cluster selama proses.                                                                  |
| **Isu yang Diketahui** | Periksa [Proxmox Roadmap](https://pve.proxmox.com/wiki/Roadmap#8.0-known-issues) untuk isu kompatibilitas, terutama untuk perangkat keras lama (>10 tahun). |
| **Versi Terbaru**      | Versi terbaru adalah 8.4, tersedia di [Proxmox Downloads](https://www.proxmox.com/en/downloads).                                                            |
| **Perangkat Keras**    | Versi 8.4 mendukung perangkat keras baru (misalnya Intel 12th gen ke atas) dan fitur seperti live migration dengan mediated devices.                        |

### 5. Sumber

* [Proxmox VE Official Documentation - Upgrade from 7 to 8](https://pve.proxmox.com/wiki/Upgrade_from_7_to_8)
* [Proxmox VE Official Documentation - System Software Updates](https://pve.proxmox.com/wiki/System_Software_Updates)
* [Proxmox Support Forum - Update from 8.0 to 8.2](https://forum.proxmox.com/threads/update-from-8-0-to-8-2.148476/)
* [Proxmox Official Website - Downloads](https://www.proxmox.com/en/downloads)
* [Proxmox VE Roadmap](https://pve.proxmox.com/wiki/Roadmap)

Panduan ini dirancang untuk memastikan proses upgrade berjalan lancar dengan risiko minimal. Jika Anda menghadapi masalah, periksa forum komunitas Proxmox atau hubungi dukungan resmi untuk bantuan lebih lanjut.

