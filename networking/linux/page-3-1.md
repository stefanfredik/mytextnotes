# Page 3

## Dokumentasi Lengkap Linux

### Panduan komprehensif sistem operasi Linux

***

### Daftar isi

1. Pendahuluan dan sejarah Linux
2. Arsitektur Linux
3. Distribusi Linux
4. Instalasi Linux
5. Struktur sistem file
6. Perintah dasar Linux
7. Manajemen file dan direktori
8. Manajemen pengguna dan grup
9. Permission dan ownership
10. Manajemen proses
11. Manajemen paket
12. Shell dan scripting
13. Manajemen jaringan
14. Manajemen disk dan storage
15. Sistem file dan mount
16. Manajemen layanan
17. Keamanan Linux
18. Monitoring dan logging
19. Virtualisasi dan container
20. Linux untuk server
21. Troubleshooting

***

### 1. Pendahuluan dan sejarah Linux

#### 1.1 Apa itu Linux?

Linux adalah sistem operasi berbasis **kernel open-source**.\
Kernel ini pertama kali dikembangkan oleh **Linus Torvalds** pada tahun 1991.

Linux digunakan pada:

* server
* desktop
* cloud
* embedded system
* smartphone melalui Android

#### 1.2 Karakteristik utama Linux

* **Open source** — kode sumber tersedia untuk dipelajari dan dimodifikasi
* **Multi-user** — mendukung banyak pengguna
* **Multi-tasking** — menjalankan banyak proses sekaligus
* **Stabil** — umum dipakai untuk server
* **Aman** — model permission sangat kuat
* **Portabel** — mendukung banyak arsitektur hardware

#### 1.3 Timeline singkat Linux

| Tahun | Peristiwa                           |
| ----- | ----------------------------------- |
| 1969  | UNIX dibuat di Bell Labs            |
| 1983  | Proyek GNU dimulai                  |
| 1991  | Linux 0.01 diumumkan                |
| 1992  | Linux dirilis dengan lisensi GPL    |
| 1993  | Debian dan Slackware muncul         |
| 1994  | Linux 1.0 dirilis                   |
| 2004  | Ubuntu pertama dirilis              |
| 2008  | Android menggunakan kernel Linux    |
| 2022+ | Linux 6.x mendukung hardware modern |

#### 1.4 Filosofi Linux

Linux mengikuti filosofi UNIX:

1. Buat program kecil yang fokus.
2. Buat program yang bisa digabung.
3. Gunakan teks sebagai antarmuka universal.

***

### 2. Arsitektur Linux

#### 2.1 Gambaran umum

Linux dibagi menjadi dua lapisan utama:

* **User space**
* **Kernel space**

```
+--------------------------------------------------+
| USER SPACE                                       |
| - aplikasi                                       |
| - shell                                          |
| - library                                        |
+--------------------------------------------------+
| SYSTEM CALL INTERFACE                            |
+--------------------------------------------------+
| KERNEL SPACE                                     |
| - process management                             |
| - memory management                              |
| - file system                                    |
| - network stack                                  |
| - device drivers                                 |
+--------------------------------------------------+
| HARDWARE                                         |
+--------------------------------------------------+
```

#### 2.2 Fungsi kernel

Kernel bertanggung jawab untuk:

* manajemen proses
* manajemen memori
* manajemen perangkat
* sistem file
* komunikasi jaringan

#### 2.3 Jenis kernel

| Jenis       | Deskripsi                               | Contoh            |
| ----------- | --------------------------------------- | ----------------- |
| Monolithic  | Banyak layanan berjalan di kernel space | Linux             |
| Microkernel | Layanan inti sangat minimal             | MINIX             |
| Hybrid      | Gabungan dua pendekatan                 | Windows NT, macOS |

> Linux menggunakan **monolithic kernel** dengan dukungan modul.

#### 2.4 Contoh manajemen modul kernel

```bash
# Melihat modul aktif
lsmod

# Memuat modul
sudo modprobe nama_module

# Menghapus modul
sudo modprobe -r nama_module

# Melihat detail modul
modinfo nama_module
```

***

### 3. Distribusi Linux

#### 3.1 Apa itu distribusi?

Distribusi Linux adalah paket lengkap yang berisi:

* kernel Linux
* utilitas GNU
* package manager
* konfigurasi bawaan
* software tambahan

#### 3.2 Keluarga distribusi populer

* **Debian** → Ubuntu, Kali, Linux Mint
* **Red Hat** → RHEL, Fedora, Rocky Linux, AlmaLinux
* **Arch** → Manjaro, EndeavourOS
* **SUSE** → openSUSE Leap, openSUSE Tumbleweed

#### 3.3 Perbandingan distro populer

| Distro     | Basis      | Package manager | Cocok untuk          |
| ---------- | ---------- | --------------- | -------------------- |
| Ubuntu     | Debian     | APT             | Pemula hingga server |
| Debian     | Independen | APT             | Server stabil        |
| Fedora     | Red Hat    | DNF             | Developer            |
| Arch Linux | Independen | Pacman          | Pengguna mahir       |
| Kali Linux | Debian     | APT             | Security testing     |
| Linux Mint | Ubuntu     | APT             | Desktop pemula       |

***

### 4. Instalasi Linux

#### 4.1 Persiapan

Sebelum instalasi:

1. Download file ISO dari situs resmi.
2. Verifikasi checksum jika perlu.
3. Buat bootable USB.
4. Backup data penting.
5. Siapkan skema partisi.

#### 4.2 Kebutuhan minimum umum

| Komponen | Minimum         |
| -------- | --------------- |
| CPU      | Dual-core 2 GHz |
| RAM      | 4 GB            |
| Storage  | 25 GB           |
| Display  | 1024 x 768      |

#### 4.3 Membuat bootable USB dengan `dd`

```bash
# Cek nama device USB
lsblk

# Tulis ISO ke USB
sudo dd if=/path/to/linux.iso of=/dev/sdX bs=4M status=progress sync
```

#### 4.4 Skema partisi sederhana

**Desktop**

```
/boot/efi   512 MB   FAT32
/           30 GB    ext4
/home       sisa     ext4
swap        4-8 GB   swap
```

**Server**

```
/boot/efi   512 MB   FAT32
/           20 GB    ext4
/var        30 GB    ext4
/home       20 GB    ext4
swap        8 GB     swap
```

#### 4.5 Setelah instalasi

```bash
sudo apt update && sudo apt upgrade -y

sudo apt install -y \
  curl wget git vim htop net-tools \
  openssh-server build-essential

sudo ufw enable
sudo reboot
```

***

### 5. Struktur sistem file

Linux mengikuti **Filesystem Hierarchy Standard**.

```
/
├── bin
├── boot
├── dev
├── etc
├── home
├── lib
├── media
├── mnt
├── opt
├── proc
├── root
├── run
├── sbin
├── srv
├── sys
├── tmp
├── usr
└── var
```

#### 5.1 Direktori penting

* **`/etc`** — file konfigurasi sistem
* **`/home`** — home directory pengguna
* **`/var`** — data yang sering berubah
* **`/proc`** — informasi proses dan kernel
* **`/dev`** — representasi perangkat
* **`/tmp`** — file sementara
* **`/usr`** — program dan library pengguna

#### 5.2 Contoh direktori umum

| Direktori       | Fungsi                |
| --------------- | --------------------- |
| `/etc/passwd`   | data akun pengguna    |
| `/etc/fstab`    | mount point saat boot |
| `/var/log`      | file log              |
| `/home/user`    | data user             |
| `/proc/cpuinfo` | informasi CPU         |
| `/dev/sda`      | perangkat disk        |
