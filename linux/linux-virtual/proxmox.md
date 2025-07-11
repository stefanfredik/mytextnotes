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



