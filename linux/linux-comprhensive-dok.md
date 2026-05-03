---
icon: book-open-lines
---

# Linux Comprhensive Dok

## DOKUMENTASI LENGKAP LINUX

### Panduan Komprehensif Sistem Operasi Linux

***

## DAFTAR ISI

1. [Pendahuluan & Sejarah Linux](https://arena.ai/c/019dec6d-cf8d-7219-80e2-2adefb08a669#1-pendahuluan--sejarah-linux)
2. [Arsitektur Linux](https://arena.ai/c/019dec6d-cf8d-7219-80e2-2adefb08a669#2-arsitektur-linux)
3. [Distribusi Linux](https://arena.ai/c/019dec6d-cf8d-7219-80e2-2adefb08a669#3-distribusi-linux)
4. [Instalasi Linux](https://arena.ai/c/019dec6d-cf8d-7219-80e2-2adefb08a669#4-instalasi-linux)
5. [Struktur Direktori Linux](https://arena.ai/c/019dec6d-cf8d-7219-80e2-2adefb08a669#5-struktur-direktori-linux)
6. [Perintah Dasar Linux](https://arena.ai/c/019dec6d-cf8d-7219-80e2-2adefb08a669#6-perintah-dasar-linux)
7. [Manajemen File & Direktori](https://arena.ai/c/019dec6d-cf8d-7219-80e2-2adefb08a669#7-manajemen-file--direktori)
8. [Manajemen Pengguna & Grup](https://arena.ai/c/019dec6d-cf8d-7219-80e2-2adefb08a669#8-manajemen-pengguna--grup)
9. [Manajemen Hak Akses](https://arena.ai/c/019dec6d-cf8d-7219-80e2-2adefb08a669#9-manajemen-hak-akses)
10. [Manajemen Proses](https://arena.ai/c/019dec6d-cf8d-7219-80e2-2adefb08a669#10-manajemen-proses)
11. [Manajemen Paket](https://arena.ai/c/019dec6d-cf8d-7219-80e2-2adefb08a669#11-manajemen-paket)
12. [Manajemen Disk & Storage](https://arena.ai/c/019dec6d-cf8d-7219-80e2-2adefb08a669#12-manajemen-disk--storage)
13. [Jaringan di Linux](https://arena.ai/c/019dec6d-cf8d-7219-80e2-2adefb08a669#13-jaringan-di-linux)
14. [Shell & Scripting](https://arena.ai/c/019dec6d-cf8d-7219-80e2-2adefb08a669#14-shell--scripting)
15. [Manajemen Layanan (Systemd)](https://arena.ai/c/019dec6d-cf8d-7219-80e2-2adefb08a669#15-manajemen-layanan-systemd)
16. [Keamanan Linux](https://arena.ai/c/019dec6d-cf8d-7219-80e2-2adefb08a669#16-keamanan-linux)
17. [Monitoring & Logging](https://arena.ai/c/019dec6d-cf8d-7219-80e2-2adefb08a669#17-monitoring--logging)
18. [Virtualisasi & Container](https://arena.ai/c/019dec6d-cf8d-7219-80e2-2adefb08a669#18-virtualisasi--container)
19. [Linux untuk Server](https://arena.ai/c/019dec6d-cf8d-7219-80e2-2adefb08a669#19-linux-untuk-server)
20. [Troubleshooting](https://arena.ai/c/019dec6d-cf8d-7219-80e2-2adefb08a669#20-troubleshooting)

***

## 1. PENDAHULUAN & SEJARAH LINUX

### 1.1 Apa Itu Linux?

Linux adalah sistem operasi **open-source** berbasis **Unix** yang pertama kali dikembangkan oleh **Linus Benedict Torvalds** pada tahun 1991. Linux bukan hanya sebuah sistem operasi, melainkan sebuah **kernel** — inti dari sistem operasi yang mengelola komunikasi antara perangkat keras (hardware) dan perangkat lunak (software).

#### Karakteristik Utama Linux:

* **Open Source** — Kode sumber dapat dilihat, dimodifikasi, dan didistribusikan secara bebas
* **Multi-user** — Mendukung banyak pengguna secara bersamaan
* **Multi-tasking** — Dapat menjalankan banyak proses secara bersamaan
* **Portable** — Dapat berjalan di berbagai arsitektur hardware
* **Stable & Secure** — Dikenal sangat stabil dan aman
* **Free** — Gratis untuk digunakan dan didistribusikan

***

### 1.2 Sejarah Linux

#### Timeline Perkembangan Linux

```
┌─────────────────────────────────────────────────────────────────┐
│                    TIMELINE SEJARAH LINUX                       │
├────────┬────────────────────────────────────────────────────────┤
│ TAHUN  │ KEJADIAN                                               │
├────────┼────────────────────────────────────────────────────────┤
│ 1969   │ AT&T Bell Labs menciptakan UNIX                        │
│ 1983   │ Richard Stallman memulai Proyek GNU (GNU's Not Unix)   │
│ 1985   │ GNU General Public License (GPL) diperkenalkan         │
│ 1987   │ MINIX dibuat oleh Andrew Tanenbaum (untuk edukasi)     │
│ 1991   │ Linus Torvalds mengumumkan kernel Linux versi 0.01     │
│ 1992   │ Linux dirilis di bawah lisensi GNU GPL                 │
│ 1993   │ Slackware & Debian Linux dirilis                       │
│ 1994   │ Linux Kernel 1.0 dirilis                               │
│ 1996   │ Linux Kernel 2.0 dirilis, maskot Tux dipilih           │
│ 1998   │ IBM & Oracle mengumumkan dukungan untuk Linux          │
│ 1999   │ Red Hat IPO, Linux mulai masuk enterprise              │
│ 2003   │ Linux Kernel 2.6 dirilis                               │
│ 2004   │ Ubuntu Linux pertama kali dirilis                      │
│ 2007   │ Android (berbasis Linux) diumumkan oleh Google         │
│ 2011   │ Linux Kernel 3.0 dirilis                               │
│ 2015   │ Linux Kernel 4.0 dirilis                               │
│ 2019   │ Linux Kernel 5.0 dirilis                               │
│ 2022   │ Linux Kernel 6.0 dirilis                               │
│ 2024   │ Linux Kernel 6.x terus berkembang                      │
└────────┴────────────────────────────────────────────────────────┘
```

### 1.3 Filosofi Linux

Linux mengikuti **Filosofi UNIX** yang dikembangkan oleh Ken Thompson dan Dennis Ritchie:

1. **"Do one thing and do it well"** — Setiap program melakukan satu tugas dengan sempurna
2. **"Everything is a file"** — Semua hal diperlakukan sebagai file
3. **"Small, simple programs"** — Program kecil yang dapat dikombinasikan
4. **"Modularity"** — Komponen yang dapat diganti dan dipertukarkan
5. **"Transparency"** — Konfigurasi dalam bentuk teks yang dapat dibaca

### 1.4 Perbandingan Linux dengan Sistem Operasi Lain

```
┌──────────────────┬───────────┬───────────┬───────────┬──────────┐
│ FITUR            │  LINUX    │  WINDOWS  │  macOS    │  FreeBSD │
├──────────────────┼───────────┼───────────┼───────────┼──────────┤
│ Biaya            │ Gratis    │ Berbayar  │ Gratis*   │ Gratis   │
│ Open Source      │ Ya        │ Tidak     │ Sebagian  │ Ya       │
│ Keamanan         │ Tinggi    │ Sedang    │ Tinggi    │ Tinggi   │
│ Stabilitas       │ Sangat    │ Sedang    │ Tinggi    │ Sangat   │
│                  │ Tinggi    │           │           │ Tinggi   │
│ Kemudahan Pakai  │ Sedang    │ Mudah     │ Mudah     │ Sulit    │
│ Dukungan HW      │ Luas      │ Sangat    │ Terbatas  │ Terbatas │
│                  │           │ Luas      │           │          │
│ Server Use       │ Dominan   │ Ada       │ Jarang    │ Ada      │
│ Desktop Use      │ ~3%       │ ~73%      │ ~15%      │ <1%      │
│ Customizable     │ Sangat    │ Terbatas  │ Terbatas  │ Sangat   │
└──────────────────┴───────────┴───────────┴───────────┴──────────┘
```

***

## 2. ARSITEKTUR LINUX

### 2.1 Gambaran Umum Arsitektur

```
┌─────────────────────────────────────────────────────────────────┐
│                    ARSITEKTUR LINUX                             │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌─────────────────────────────────────────────────────────┐   ││  
│              USER SPACE (RUANG PENGGUNA)                │   ││  │
┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐  │   ││  │  
│ Aplikasi │ │Aplikasi  │ │ Shell    │ │ Library  │  │   ││  │  
│ Desktop  │ │ Server   │ │ (bash)   │ │ (glibc)  │  │   ││  │  
└──────────┘ └──────────┘ └──────────┘ └──────────┘  │   ││  └─────────────────────────────────────────────────────────┘   ││                           │                                     ││                    System Calls                                 ││                           │                                     ││  ┌─────────────────────────────────────────────────────────┐   ││  │                  KERNEL SPACE                           │   ││  │  ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐  │   ││  │  │ Process │  │ Memory  │  │  File   │  │ Network │  │   ││  │  │ Mgmt    │  │ Mgmt    │  │ System  │  │ Stack   │  │   ││  │  └─────────┘  └─────────┘  └─────────┘  └─────────┘  │   ││  │  ┌─────────────────────────────────────────────────┐  │   ││  │  │           DEVICE DRIVERS                        │  │   ││  │  └─────────────────────────────────────────────────┘  │   ││  └─────────────────────────────────────────────────────────┘   ││                           │                                     ││  ┌─────────────────────────────────────────────────────────┐   ││  │                    HARDWARE                             │   ││  │   ┌────┐  ┌────┐  ┌────────┐  ┌────────┐  ┌───────┐  │   ││  │   │CPU │  │RAM │  │Storage │  │Network │  │ I/O   │  │   ││  │   └────┘  └────┘  └────────┘  └────────┘  └───────┘  │   ││  └─────────────────────────────────────────────────────────┘   │└─────────────────────────────────────────────────────────────────┘
```

### 2.2 Komponen Kernel Linux

#### 2.2.1 Process Management (Manajemen Proses)

* Membuat dan menghapus proses
* Penjadwalan proses (scheduling)
* Komunikasi antar proses (IPC)
* Sinkronisasi proses

#### 2.2.2 Memory Management (Manajemen Memori)

* Virtual memory
* Memory allocation dan deallocation
* Paging dan swapping
* Memory protection

#### 2.2.3 File System

* Mendukung berbagai sistem file (ext4, XFS, Btrfs, NTFS, FAT32)
* Virtual File System (VFS) sebagai abstraksi
* Manajemen buffer dan cache

#### 2.2.4 Network Stack

* Implementasi protokol TCP/IP
* Socket interface
* Filtering paket (iptables/nftables)

#### 2.2.5 Device Drivers

* Antarmuka antara kernel dan hardware
* Character devices (keyboard, mouse)
* Block devices (disk, SSD)
* Network devices (NIC)

### 2.3 Jenis-Jenis Kernel

```
text┌─────────────────────────────────────────────────────────────────┐│                    JENIS-JENIS KERNEL                           │├──────────────────┬──────────────────────────────────────────────┤│ MONOLITHIC       │ Semua layanan berjalan di kernel space        ││ (Linux)          │ + Performa tinggi                            ││                  │ - Lebih rentan jika ada bug                  │├──────────────────┼──────────────────────────────────────────────┤│ MICROKERNEL      │ Hanya layanan inti di kernel space           ││ (MINIX)          │ + Lebih stabil & aman                        ││                  │ - Performa lebih rendah                      │├──────────────────┼──────────────────────────────────────────────┤│ HYBRID           │ Kombinasi monolithic dan microkernel         ││ (Windows NT,     │ + Performa & stabilitas seimbang             ││  macOS XNU)      │                                              │└──────────────────┴──────────────────────────────────────────────┘
```

***

## 3. DISTRIBUSI LINUX

### 3.1 Apa Itu Distribusi (Distro)?

Distribusi Linux adalah paket lengkap yang terdiri dari:

* **Kernel Linux**
* **GNU Tools & Libraries**
* **Package Manager**
* **Desktop Environment** (opsional)
* **Software Tambahan**

### 3.2 Pohon Keluarga Distro Linux

```
text┌─────────────────────────────────────────────────────────────────┐│                  POHON KELUARGA DISTRO LINUX                   ││                                                                 ││                        LINUX KERNEL                            ││                             │                                  ││          ┌──────────────────┼──────────────────┐              ││          │                  │                  │              ││       DEBIAN             RED HAT            SLACKWARE         ││          │                  │                  │              ││     ┌────┴────┐         ┌───┴───┐          ┌───┴───┐         ││   Ubuntu   Kali      RHEL    Fedora        SUSE   Arch        ││     │      Linux        │       │            │      │          ││  ┌──┴──┐          CentOS   openSUSE      openSUSE Manjaro     ││ Mint Lubuntu      Rocky     Stream                            ││      Xubuntu      AlmaLinux                                   ││      Kubuntu                                                  │└─────────────────────────────────────────────────────────────────┘
```

### 3.3 Distro Populer Beserta Karakteristiknya

#### 3.3.1 Ubuntu

```
text┌─────────────────────────────────────────────────────────────────┐│ UBUNTU                                                          │├─────────────────┬───────────────────────────────────────────────┤│ Developer       │ Canonical Ltd.                                ││ Basis           │ Debian                                        ││ Package Manager │ APT (apt/apt-get)                             ││ Desktop         │ GNOME (default)                               ││ Target User     │ Pemula hingga Profesional                     ││ Rilis           │ 6 bulan sekali (LTS: 2 tahun sekali)         ││ LTS Support     │ 5 tahun                                       ││ Cocok untuk     │ Desktop, Server, Cloud                        ││ Website         │ ubuntu.com                                    │└─────────────────┴───────────────────────────────────────────────┘
```

#### 3.3.2 Debian

```
text┌─────────────────────────────────────────────────────────────────┐│ DEBIAN                                                          │├─────────────────┬───────────────────────────────────────────────┤│ Developer       │ Debian Project (komunitas)                    ││ Package Manager │ APT (dpkg)                                    ││ Filosofi        │ 100% Free Software                            ││ Stabilitas      │ Sangat Tinggi                                 ││ Cocok untuk     │ Server, Advanced Users                        ││ Website         │ debian.org                                    │└─────────────────┴───────────────────────────────────────────────┘
```

#### 3.3.3 Red Hat Enterprise Linux (RHEL)

```
text┌─────────────────────────────────────────────────────────────────┐│ RED HAT ENTERPRISE LINUX (RHEL)                                 │├─────────────────┬───────────────────────────────────────────────┤│ Developer       │ Red Hat (IBM)                                 ││ Package Manager │ DNF/YUM (RPM)                                 ││ Target          │ Enterprise                                    ││ Support         │ 10 tahun                                      ││ Biaya           │ Berbayar (subscription)                       ││ Cocok untuk     │ Enterprise Server, Mission Critical           ││ Website         │ redhat.com                                    │└─────────────────┴───────────────────────────────────────────────┘
```

#### 3.3.4 Distribusi Lainnya

| Distro              | Basis         | Keunggulan                           | Target User           |
| ------------------- | ------------- | ------------------------------------ | --------------------- |
| **Linux Mint**      | Ubuntu/Debian | Sangat mudah, mirip Windows          | Pemula                |
| **Fedora**          | Red Hat       | Teknologi terbaru                    | Developer             |
| **Arch Linux**      | Independen    | Rolling release, sangat customizable | Expert                |
| **Manjaro**         | Arch          | Arch yang mudah digunakan            | Intermediate          |
| **openSUSE**        | SUSE          | YaST tool, enterprise grade          | Intermediate          |
| **Kali Linux**      | Debian        | Tools keamanan lengkap               | Security Professional |
| **Rocky Linux**     | RHEL          | Pengganti CentOS gratis              | Enterprise            |
| **AlmaLinux**       | RHEL          | Pengganti CentOS gratis              | Enterprise            |
| **Alpine Linux**    | Independen    | Sangat ringan                        | Container, Embedded   |
| **Raspberry Pi OS** | Debian        | Untuk Raspberry Pi                   | IoT, Education        |

***

## 4. INSTALASI LINUX

### 4.1 Persiapan Sebelum Instalasi

#### Persyaratan Minimum Hardware (Ubuntu 22.04 LTS)

```
text┌─────────────────────────────────────────────────────────────────┐│              PERSYARATAN HARDWARE UBUNTU                        │├──────────────────┬──────────────────┬──────────────────────────┤│ KOMPONEN         │ MINIMUM          │ REKOMENDASI              │├──────────────────┼──────────────────┼──────────────────────────┤│ CPU              │ 1 GHz dual-core  │ 2+ GHz quad-core        ││ RAM              │ 4 GB             │ 8 GB atau lebih          ││ Storage          │ 25 GB            │ 50 GB atau lebih         ││ Display          │ 1024x768         │ 1920x1080                ││ USB/DVD          │ Untuk boot media │ USB 3.0                  ││ Internet         │ Opsional         │ Direkomendasikan         │└──────────────────┴──────────────────┴──────────────────────────┘
```

### 4.2 Cara Membuat Bootable Media

#### Menggunakan Rufus (Windows)

```
text1. Download Rufus dari rufus.ie2. Download ISO Linux yang diinginkan3. Colokkan USB minimal 8 GB4. Buka Rufus5. Pilih USB Device6. Pilih file ISO7. Klik START
```

#### Menggunakan dd (Linux/Mac)

```
Bash# Cari nama device USBlsblk# Buat bootable USB (HATI-HATI: pastikan device benar!)sudo dd if=/path/to/linux.iso of=/dev/sdX bs=4M status=progress sync# Contoh:sudo dd if=ubuntu-22.04-desktop-amd64.iso of=/dev/sdb bs=4M status=progress sync
```

### 4.3 Proses Instalasi Ubuntu (Step by Step)

```
textLANGKAH INSTALASI UBUNTU:Step 1: Boot dari USB        ├── Masuk BIOS/UEFI (F2, F12, DEL)        ├── Ubah boot order ke USB        └── Save & RestartStep 2: Pilih Bahasa        └── Pilih bahasa yang diinginkanStep 3: Persiapan Instalasi        ├── Centang "Download updates while installing"        └── Centang "Install third-party software" (opsional)Step 4: Partisi Disk        ├── Erase disk (otomatis) - untuk instalasi baru        ├── "Something else" - untuk partisi manual        │   ├── /boot    : 512 MB - 1 GB (ext4)        │   ├── /        : 20-50 GB (ext4) - root partition        │   ├── /home    : Sisa ruang (ext4)        │   └── swap     : 2x RAM atau 4-8 GB        └── Dual boot dengan WindowsStep 5: Lokasi & Timezone        └── Pilih kota/zona waktuStep 6: Buat Akun Pengguna        ├── Nama lengkap        ├── Username (huruf kecil, tanpa spasi)        └── Password (minimal 8 karakter)Step 7: Instalasi Berlangsung        └── Tunggu 15-30 menitStep 8: Restart        └── Lepas USB dan restart
```

### 4.4 Skema Partisi yang Direkomendasikan

#### Untuk Desktop (100 GB)

```
text┌─────────────────────────────────────────────────────────────────┐│                    SKEMA PARTISI DESKTOP                        │├─────────────┬──────────┬──────────┬─────────────────────────────┤│ PARTISI     │ UKURAN   │ FORMAT   │ DESKRIPSI                   │├─────────────┼──────────┼──────────┼─────────────────────────────┤│ /boot/efi   │ 512 MB   │ FAT32    │ EFI System Partition        ││ /boot       │ 1 GB     │ ext4     │ Kernel & bootloader files   ││ /           │ 30 GB    │ ext4     │ Sistem utama                ││ /home       │ 60 GB    │ ext4     │ Data pengguna               ││ swap        │ 8 GB     │ swap     │ Virtual memory              │└─────────────┴──────────┴──────────┴─────────────────────────────┘
```

#### Untuk Server (200 GB)

```
text┌─────────────────────────────────────────────────────────────────┐│                    SKEMA PARTISI SERVER                         │├─────────────┬──────────┬──────────┬─────────────────────────────┤│ PARTISI     │ UKURAN   │ FORMAT   │ DESKRIPSI                   │├─────────────┼──────────┼──────────┼─────────────────────────────┤│ /boot/efi   │ 512 MB   │ FAT32    │ EFI System Partition        ││ /boot       │ 1 GB     │ ext4     │ Boot files                  ││ /           │ 20 GB    │ ext4     │ Sistem utama                ││ /var        │ 50 GB    │ ext4     │ Log & variable data         ││ /tmp        │ 10 GB    │ ext4     │ File temporary              ││ /home       │ 20 GB    │ ext4     │ Home pengguna               ││ /opt        │ 50 GB    │ ext4     │ Optional software           ││ /data       │ 40 GB    │ ext4     │ Data aplikasi               ││ swap        │ 8 GB     │ swap     │ Virtual memory              │└─────────────┴──────────┴──────────┴─────────────────────────────┘
```

***

## 5. STRUKTUR DIREKTORI LINUX

### 5.1 Filesystem Hierarchy Standard (FHS)

Linux menggunakan standar hierarki filesystem yang disebut **FHS (Filesystem Hierarchy Standard)**:

```
text/├── bin/          → Perintah dasar untuk semua pengguna├── boot/         → File bootloader & kernel├── dev/          → Device files├── etc/          → File konfigurasi sistem├── home/         → Direktori home pengguna│   ├── user1/│   └── user2/├── lib/          → Library sistem├── lib64/        → Library 64-bit├── media/        → Mount point untuk media removable├── mnt/          → Mount point sementara├── opt/          → Software optional/third-party├── proc/         → Virtual filesystem (informasi proses)├── root/         → Home directory root user├── run/          → Data runtime├── sbin/         → Perintah sistem untuk administrator├── srv/          → Data untuk layanan (web, ftp, dll)├── sys/          → Virtual filesystem (informasi kernel)├── tmp/          → File temporary├── usr/          → Program dan data pengguna│   ├── bin/      → Perintah pengguna│   ├── lib/      → Library│   ├── local/    → Software yang diinstal manual│   └── share/    → Data bersama└── var/          → Variable data (log, mail, spool)    ├── log/      → File log    ├── mail/     → Mail spool    └── www/      → Web data
```

### 5.2 Detail Setiap Direktori

```
text┌─────────────────────────────────────────────────────────────────┐│              DETAIL DIREKTORI LINUX                             │├──────────────┬──────────────────────────────────────────────────┤│ DIREKTORI    │ DESKRIPSI & KONTEN                               │├──────────────┼──────────────────────────────────────────────────┤│ /            │ Root directory - puncak hierarki filesystem      │├──────────────┼──────────────────────────────────────────────────┤│ /bin         │ Binary essentials: ls, cp, mv, cat, bash         │├──────────────┼──────────────────────────────────────────────────┤│ /boot        │ kernel (vmlinuz), initrd, GRUB configuration     │├──────────────┼──────────────────────────────────────────────────┤│ /dev         │ Device files: /dev/sda (disk), /dev/null,        ││              │ /dev/random, /dev/tty (terminal)                 │├──────────────┼──────────────────────────────────────────────────┤│ /etc         │ Config: /etc/passwd, /etc/fstab,                 ││              │ /etc/network/, /etc/ssh/, /etc/apt/              │├──────────────┼──────────────────────────────────────────────────┤│ /home        │ /home/alice/, /home/bob/ - data pengguna         │├──────────────┼──────────────────────────────────────────────────┤│ /lib         │ Shared libraries (.so files), kernel modules     │├──────────────┼──────────────────────────────────────────────────┤│ /media       │ Auto-mount: /media/usb, /media/cdrom             │├──────────────┼──────────────────────────────────────────────────┤│ /mnt         │ Manual mount points yang bersifat sementara      │├──────────────┼──────────────────────────────────────────────────┤│ /opt         │ /opt/google/, /opt/zoom/ - software pihak ketiga │├──────────────┼──────────────────────────────────────────────────┤│ /proc        │ /proc/cpuinfo, /proc/meminfo, /proc/[PID]/       │├──────────────┼──────────────────────────────────────────────────┤│ /root        │ Home directory untuk superuser (root)            │├──────────────┼──────────────────────────────────────────────────┤│ /sbin        │ System binaries: fdisk, iptables, shutdown       │├──────────────┼──────────────────────────────────────────────────┤│ /srv         │ /srv/http/, /srv/ftp/ - data layanan             │├──────────────┼──────────────────────────────────────────────────┤│ /sys         │ Virtual FS untuk info kernel & hardware          │├──────────────┼──────────────────────────────────────────────────┤│ /tmp         │ File sementara, dihapus saat reboot              │├──────────────┼──────────────────────────────────────────────────┤│ /usr         │ Hierarki sekunder: /usr/bin, /usr/lib, /usr/share│├──────────────┼──────────────────────────────────────────────────┤│ /var         │ Log: /var/log/syslog, Mail, Database spool       │└──────────────┴──────────────────────────────────────────────────┘
```

***

## 6. PERINTAH DASAR LINUX

### 6.1 Navigasi Sistem

```
Bash# ─────────────────────────────────────────────────────────────# NAVIGASI DIREKTORI# ─────────────────────────────────────────────────────────────pwd                    # Print Working Directory - tampilkan lokasi sekarangcd /home/user          # Pindah ke direktori /home/usercd ~                   # Pindah ke home directorycd ..                  # Pindah ke direktori induk (parent)cd -                   # Kembali ke direktori sebelumnyacd /                   # Pindah ke root directory# ─────────────────────────────────────────────────────────────# MELIHAT ISI DIREKTORI# ─────────────────────────────────────────────────────────────ls                     # List file dan direktorils -l                  # List dengan detail panjangls -a                  # Tampilkan semua file (termasuk hidden)ls -la                 # Kombinasi detail dan hidden filels -lh                 # List dengan ukuran human-readablels -lt                 # Sort berdasarkan waktu modifikasils -lr                 # List secara reversels -R                  # List secara rekursifls --color             # List dengan warna# Contoh Output ls -la:# drwxr-xr-x  2 user group 4096 Jan  1 10:00 Documents# -rw-r--r--  1 user group 1234 Jan  1 09:00 file.txt# │││││││││└─ nama file/direktori# ││││││││└── tanggal modifikasi# │││││││└─── ukuran (bytes)# ││││││└──── grup pemilik# │││││└───── pemilik# ││││└────── jumlah hard link# │└──────── permission bits# └───────── tipe file (d=dir, -=file, l=link)
```

### 6.2 Perintah Informasi Sistem

```
Bash# ─────────────────────────────────────────────────────────────# INFORMASI SISTEM# ─────────────────────────────────────────────────────────────uname -a               # Informasi kernel lengkapuname -r               # Versi kernel sajauname -m               # Arsitektur (x86_64, arm64)hostname               # Nama hosthostname -I            # Alamat IPwhoami                 # Username saat iniid                     # UID, GID, dan grup penggunawho                    # Siapa yang sedang loginw                      # Pengguna aktif dan aktivitasnyalast                   # Riwayat loginuptime                 # Waktu sistem aktif# INFORMASI HARDWARElscpu                  # Informasi CPUlsmem                  # Informasi Memorylsblk                  # Informasi Block Device (disk)lsusb                  # Informasi USB devicelspci                  # Informasi PCI devicedmidecode              # Informasi hardware lengkap dari BIOS# INFORMASI DISTRIBUSIcat /etc/os-release    # Informasi OSlsb_release -a         # Informasi distribusicat /proc/version      # Versi kernelcat /proc/cpuinfo      # Detail CPUcat /proc/meminfo      # Detail memory# TANGGAL & WAKTUdate                   # Tanggal dan waktu sekarangdate "+%Y-%m-%d"       # Format custom: 2024-01-01timedatectl            # Status waktu sistemcal                    # Kalender bulan ini
```

### 6.3 Perintah Manajemen File

```
Bash# ─────────────────────────────────────────────────────────────# MEMBUAT FILE & DIREKTORI# ─────────────────────────────────────────────────────────────touch file.txt              # Buat file kosong / update timestamptouch file1.txt file2.txt   # Buat beberapa file sekaligusmkdir direktori             # Buat direktorimkdir -p /a/b/c             # Buat direktori beserta parent-nyamkdir -m 755 direktori      # Buat direktori dengan permission# ─────────────────────────────────────────────────────────────# MENYALIN FILE# ─────────────────────────────────────────────────────────────cp file.txt backup.txt          # Salin filecp -r direktori/ backup/        # Salin direktori secara rekursifcp -p file.txt backup.txt       # Salin dengan preserve attributecp -v file.txt /tmp/            # Salin dengan verbose outputcp -i file.txt existing.txt     # Konfirmasi jika file sudah adacp -u file.txt backup.txt       # Salin hanya jika lebih baru# ─────────────────────────────────────────────────────────────# MEMINDAHKAN/MENGGANTI NAMA# ─────────────────────────────────────────────────────────────mv file.txt /tmp/               # Pindahkan file ke /tmp/mv oldname.txt newname.txt      # Ganti nama filemv -i file.txt existing.txt     # Konfirmasi jika file sudah adamv -v file.txt /tmp/            # Verbose output# ─────────────────────────────────────────────────────────────# MENGHAPUS FILE# ─────────────────────────────────────────────────────────────rm file.txt                 # Hapus filerm -f file.txt              # Hapus paksa (force)rm -r direktori/            # Hapus direktori rekursifrm -rf direktori/           # Hapus paksa & rekursif (HATI-HATI!)rm -i file.txt              # Konfirmasi sebelum hapusrmdir direktori/            # Hapus direktori kosong# ─────────────────────────────────────────────────────────────# MEMBACA FILE# ─────────────────────────────────────────────────────────────cat file.txt                # Tampilkan isi filecat -n file.txt             # Tampilkan dengan nomor baristac file.txt                # Tampilkan terbalik (baris terakhir dulu)more file.txt               # Tampilkan per halaman (lama)less file.txt               # Tampilkan per halaman (interaktif)head file.txt               # Tampilkan 10 baris pertamahead -n 20 file.txt         # Tampilkan 20 baris pertamatail file.txt               # Tampilkan 10 baris terakhirtail -n 20 file.txt         # Tampilkan 20 baris terakhirtail -f /var/log/syslog     # Monitor log secara real-time# ─────────────────────────────────────────────────────────────# MENCARI FILE# ─────────────────────────────────────────────────────────────find / -name "file.txt"             # Cari file bernama file.txtfind /home -name "*.pdf"            # Cari semua file PDFfind /tmp -type f -mtime -7         # File dimodifikasi dalam 7 harifind . -size +100M                  # File lebih dari 100 MBfind /etc -name "*.conf" -exec cat {} \;   # Eksekusi perintah pada hasillocate file.txt                     # Cari menggunakan database (cepat)updatedb                            # Update database locatewhich ls                            # Lokasi perintah lswhereis bash                        # Lokasi binary, source, manual bashtype ls                             # Informasi tipe perintah
```

### 6.4 Perintah Teks

```
Bash# ─────────────────────────────────────────────────────────────# GREP - Pencarian Teks# ─────────────────────────────────────────────────────────────grep "kata" file.txt            # Cari "kata" dalam filegrep -i "kata" file.txt         # Case-insensitivegrep -r "kata" /direktori/      # Rekursif dalam direktorigrep -n "kata" file.txt         # Tampilkan nomor barisgrep -v "kata" file.txt         # Tampilkan baris yang TIDAK mengandung katagrep -c "kata" file.txt         # Hitung jumlah baris yang cocokgrep -l "kata" *.txt            # Hanya tampilkan nama filegrep -E "pola|regex" file.txt   # Extended regexgrep -w "kata" file.txt         # Hanya kata utuh (whole word)grep "^awal" file.txt           # Baris yang dimulai dengan "awal"grep "akhir$" file.txt          # Baris yang diakhiri "akhir"# ─────────────────────────────────────────────────────────────# SED - Stream Editor# ─────────────────────────────────────────────────────────────sed 's/lama/baru/' file.txt             # Ganti kemunculan pertamased 's/lama/baru/g' file.txt            # Ganti semua kemunculansed -i 's/lama/baru/g' file.txt         # Ganti langsung di filesed -n '1,5p' file.txt                  # Tampilkan baris 1-5sed '3d' file.txt                       # Hapus baris ke-3sed '/kata/d' file.txt                  # Hapus baris yang mengandung "kata"sed 's/^/# /' file.txt                  # Tambah "# " di awal setiap baris# ─────────────────────────────────────────────────────────────# AWK - Text Processing# ─────────────────────────────────────────────────────────────awk '{print $1}' file.txt               # Print kolom pertamaawk '{print $1,$3}' file.txt            # Print kolom 1 dan 3awk -F: '{print $1}' /etc/passwd        # Separator ":", print field 1awk '/kata/ {print}' file.txt           # Print baris yang mengandung "kata"awk '{sum += $1} END {print sum}' f     # Hitung jumlah kolom 1# ─────────────────────────────────────────────────────────────# SORT & UNIQ# ─────────────────────────────────────────────────────────────sort file.txt                   # Urutkan secara alfabetissort -r file.txt                # Urutkan terbaliksort -n file.txt                # Urutkan secara numeriksort -k2 file.txt               # Urutkan berdasarkan kolom 2sort -u file.txt                # Urutkan dan hapus duplikatuniq file.txt                   # Hapus baris duplikat berturutanuniq -c file.txt                # Hitung kemunculanuniq -d file.txt                # Tampilkan hanya duplikat# ─────────────────────────────────────────────────────────────# PERINTAH TEKS LAINNYA# ─────────────────────────────────────────────────────────────wc file.txt                     # Hitung baris, kata, karakterwc -l file.txt                  # Hitung baris sajawc -w file.txt                  # Hitung kata sajacut -d: -f1 /etc/passwd         # Potong field 1 dengan delimiter ":"cut -c1-10 file.txt             # Potong karakter 1-10tr 'a-z' 'A-Z' < file.txt      # Konversi lowercase ke uppercasetr -d '\r' < file.txt           # Hapus karakter carriage returndiff file1.txt file2.txt        # Bandingkan dua filecomm file1.txt file2.txt        # Perbandingan baris dua filepaste file1.txt file2.txt       # Gabungkan dua file secara kolom
```

***

## 7. MANAJEMEN FILE & DIREKTORI

### 7.1 Tipe File di Linux

```
textTIPE FILE LINUX:- (dash)   → Regular file (teks, binary, gambar)d           → Directoryl           → Symbolic linkb           → Block device (disk)c           → Character device (keyboard, mouse)p           → Named pipe (FIFO)s           → Socket
```

### 7.2 Link di Linux

```
Bash# ─────────────────────────────────────────────────────────────# HARD LINK vs SOFT LINK (SYMBOLIC LINK)# ─────────────────────────────────────────────────────────────# HARD LINK# ─────────# - Menunjuk langsung ke inode yang sama# - Tidak bisa link ke direktori# - Tidak bisa cross filesystem# - Jika file asli dihapus, link masih validln file.txt hardlink.txt        # Buat hard link# SYMBOLIC LINK (SYMLINK)# ──────────────────────# - Menunjuk ke path file asli# - Bisa link ke direktori# - Bisa cross filesystem# - Jika file asli dihapus, link menjadi brokenln -s /path/to/file symlink     # Buat symbolic linkln -s /var/log/nginx nginx-log  # Contoh: link ke direktori logls -la                          # Lihat link: lrwxrwxrwx -> target# Hapus symlinkunlink symlink                  # Hapus symbolic linkrm symlink                      # Cara lain menghapus symlink
```

### 7.3 Kompresi dan Arsip

```
Bash# ─────────────────────────────────────────────────────────────# TAR - Tape Archive# ─────────────────────────────────────────────────────────────# Membuat arsiptar -cf arsip.tar direktori/        # Buat arsip tanpa kompresitar -czf arsip.tar.gz direktori/    # Buat arsip + kompresi gziptar -cjf arsip.tar.bz2 direktori/  # Buat arsip + kompresi bzip2tar -cJf arsip.tar.xz direktori/   # Buat arsip + kompresi xz# Mengekstrak arsiptar -xf arsip.tar                   # Ekstrak arsiptar -xzf arsip.tar.gz               # Ekstrak arsip gziptar -xjf arsip.tar.bz2              # Ekstrak arsip bzip2tar -xf arsip.tar -C /tujuan/       # Ekstrak ke direktori tertentu# Melihat isi arsiptar -tf arsip.tar                   # List isi tanpa mengekstraktar -tvf arsip.tar                  # List dengan detail# Flag tar:# c = create (buat)# x = extract (ekstrak)# t = list (lihat isi)# f = file (nama file)# v = verbose (tampilkan proses)# z = gunakan gzip# j = gunakan bzip2# J = gunakan xz# ─────────────────────────────────────────────────────────────# GZIP, BZIP2, XZ# ─────────────────────────────────────────────────────────────gzip file.txt               # Kompresi file (hasilkan file.txt.gz)gzip -d file.txt.gz         # Dekompresigzip -k file.txt            # Kompresi, pertahankan file asligzip -9 file.txt            # Kompresi maksimal (lebih lama)gunzip file.txt.gz          # Dekompresi (sama dengan gzip -d)bzip2 file.txt              # Kompresi dengan bzip2bunzip2 file.txt.bz2        # Dekompresi bzip2xz file.txt                 # Kompresi dengan xz (terbaik)unxz file.txt.xz            # Dekompresi xz# ─────────────────────────────────────────────────────────────# ZIP# ─────────────────────────────────────────────────────────────zip arsip.zip file1 file2   # Buat zipzip -r arsip.zip direktori/ # Buat zip rekursifunzip arsip.zip             # Ekstrak zipunzip -l arsip.zip          # List isi zipunzip arsip.zip -d /tujuan/ # Ekstrak ke direktori
```

### 7.4 File Attributes

```
Bash# ─────────────────────────────────────────────────────────────# CHATTR - Change File Attributes# ─────────────────────────────────────────────────────────────chattr +i file.txt          # Buat file immutable (tidak bisa diubah/hapus)chattr -i file.txt          # Hapus atribut immutablechattr +a file.txt          # Append only (hanya bisa ditambah)lsattr file.txt             # Lihat atribut file# ─────────────────────────────────────────────────────────────# STAT - Informasi Detail File# ─────────────────────────────────────────────────────────────stat file.txt               # Tampilkan info lengkap file# Output mencakup:# - File name# - File size# - Blocks & IO Block# - File type# - Device & Inode# - Links# - Access, Modify, Change time# - Permissions
```

***

## 8. MANAJEMEN PENGGUNA & GRUP

### 8.1 Konsep Pengguna di Linux

```
textJENIS PENGGUNA LINUX:┌─────────────────────────────────────────────────────────────────┐│ 1. ROOT USER (Superuser)                                        ││    - UID = 0                                                    ││    - Akses penuh ke sistem                                      ││    - Prompt: # (tanda pagar)                                    ││                                                                 ││ 2. SYSTEM USERS                                                 ││    - UID = 1 - 999                                              ││    - Untuk layanan sistem (www-data, mysql, postgres)           ││    - Biasanya tidak bisa login                                  ││                                                                 ││ 3. REGULAR USERS                                                ││    - UID = 1000+                                                ││    - Pengguna biasa                                             ││    - Prompt: $ (tanda dollar)                                   │└─────────────────────────────────────────────────────────────────┘
```

### 8.2 File Konfigurasi Pengguna

#### /etc/passwd

```
Bash# Format: username:password:UID:GID:comment:home:shell# Contoh:alice:x:1001:1001:Alice Smith:/home/alice:/bin/bash# │    │  │    │   │           │           └── login shell# │    │  │    │   │           └── home directory# │    │  │    │   └── komentar/nama lengkap# │    │  │    └── Primary GID# │    │  └── UID# │    └── password (x = ada di /etc/shadow)# └── usernamecat /etc/passwd             # Lihat semua pengguna
```

#### /etc/shadow

```
Bash# File password yang dienkripsi (hanya root yang bisa baca)# Format: username:hashed_password:lastchg:min:max:warn:inactive:expirecat /etc/shadow             # Lihat (butuh root)
```

#### /etc/group

```
Bash# Format: groupname:password:GID:members# Contoh:sudo:x:27:alice,bob# │   │  │  └── anggota grup# │   │  └── GID# │   └── password grup# └── nama grupcat /etc/group              # Lihat semua grup
```

### 8.3 Manajemen Pengguna

```
Bash# ─────────────────────────────────────────────────────────────# MENAMBAH PENGGUNA# ─────────────────────────────────────────────────────────────useradd username                    # Buat pengguna baru (basic)useradd -m username                 # Buat pengguna + home directoryuseradd -m -s /bin/bash username    # Buat dengan shell bashuseradd -m -G sudo username         # Buat + tambahkan ke grup sudouseradd -m -u 1500 username         # Buat dengan UID tertentuuseradd -m -c "Nama Lengkap" user   # Buat dengan komentaradduser username                    # Cara interaktif (lebih mudah)# Akan ditanya:# - Password# - Full name# - Room number# - Work phone, Home phone# - Other# ─────────────────────────────────────────────────────────────# MENGATUR PASSWORD# ─────────────────────────────────────────────────────────────passwd username             # Set password untuk penggunapasswd                      # Ubah password diri sendiripasswd -l username          # Lock account (tidak bisa login)passwd -u username          # Unlock accountpasswd -e username          # Expire password (harus ganti saat login)passwd -d username          # Hapus password (login tanpa password)passwd --mindays 7 user     # Minimal 7 hari sebelum bisa gantipasswd --maxdays 90 user    # Maksimal 90 hari sebelum harus gantipasswd --warndays 14 user   # Peringatan 14 hari sebelum expirechage -l username           # Lihat info expire passwordchage -M 90 username        # Set max age password 90 harichage -E 2025-12-31 user    # Set tanggal expire akun# ─────────────────────────────────────────────────────────────# MENGUBAH PENGGUNA# ─────────────────────────────────────────────────────────────usermod -l newname oldname          # Ganti usernameusermod -d /home/newdir username    # Ubah home directoryusermod -s /bin/zsh username        # Ubah shellusermod -G sudo,docker username     # Tambah ke grup (replace)usermod -aG sudo username           # Tambah ke grup (append)usermod -u 1600 username            # Ubah UIDusermod -L username                 # Lock accountusermod -U username                 # Unlock account# ─────────────────────────────────────────────────────────────# MENGHAPUS PENGGUNA# ─────────────────────────────────────────────────────────────userdel username            # Hapus pengguna (home tetap ada)userdel -r username         # Hapus pengguna + home directorydeluser username            # Cara interaktif# ─────────────────────────────────────────────────────────────# INFORMASI PENGGUNA# ─────────────────────────────────────────────────────────────id username                 # UID, GID, dan grupgroups username             # Grup yang diikutifinger username             # Info pengguna lengkapgetent passwd username      # Info dari database sistem
```

### 8.4 Manajemen Grup

```
Bash# ─────────────────────────────────────────────────────────────# MANAJEMEN GRUP# ─────────────────────────────────────────────────────────────groupadd namagrup           # Buat grup barugroupadd -g 1100 namagrup   # Buat grup dengan GID tertentugroupdel namagrup           # Hapus grupgroupmod -n newname oldname # Ganti nama grupgpasswd -a user grup        # Tambah user ke grupgpasswd -d user grup        # Hapus user dari grupgpasswd -A user grup        # Set user sebagai admin grupgetent group namagrup       # Info grup
```

### 8.5 Sudo (Superuser Do)

```
Bash# ─────────────────────────────────────────────────────────────# SUDO# ─────────────────────────────────────────────────────────────sudo perintah               # Jalankan perintah sebagai rootsudo -i                     # Login shell sebagai rootsudo -s                     # Shell sebagai root (tanpa login)sudo -u user perintah       # Jalankan sebagai user lainsudo !!                     # Jalankan perintah sebelumnya dengan sudosudo -l                     # Lihat perintah apa yang boleh di-sudo# Edit konfigurasi sudovisudo                      # Edit /etc/sudoers (cara aman)# Format sudoers:# user ALL=(ALL:ALL) ALL# │    │    │    │   └── perintah yang boleh# │    │    │    └── grup yang boleh digunakan# │    │    └── user yang boleh digunakan# │    └── host yang berlaku# └── siapa# Contoh konfigurasi sudoers:alice ALL=(ALL:ALL) ALL                 # Alice bisa semuabob ALL=(ALL) /bin/ls, /bin/cat        # Bob hanya ls dan cat%admin ALL=(ALL) NOPASSWD: ALL         # Grup admin tanpa password
```

***

## 9. MANAJEMEN HAK AKSES

### 9.1 Sistem Permission Linux

```
textSTRUKTUR PERMISSION:     User  Group  Other      │      │      │    rwxr   xr-   x--    │││    │││   │││    │││    │││   ││└── Execute (other)    │││    │││   │└─── Write (other)    │││    │││   └──── Read (other)    │││    ││└──── Execute (group)    │││    │└───── Write (group)    │││    └────── Read (group)    ││└──── Execute (user)    │└───── Write (user)    └────── Read (user)NILAI PERMISSION:r (read)    = 4w (write)   = 2x (execute) = 1Contoh:rwx = 4+2+1 = 7rw- = 4+2+0 = 6r-x = 4+0+1 = 5r-- = 4+0+0 = 4--- = 0+0+0 = 0Permission umum:755 = rwxr-xr-x (direktori standar)644 = rw-r--r-- (file standar)600 = rw------- (file pribadi)777 = rwxrwxrwx (semua akses - HINDARI!)
```

### 9.2 Chmod - Mengubah Permission

```
Bash# ─────────────────────────────────────────────────────────────# CHMOD - Change Mode# ─────────────────────────────────────────────────────────────# Metode Numerik (Octal)chmod 755 file.txt          # rwxr-xr-xchmod 644 file.txt          # rw-r--r--chmod 600 file.txt          # rw-------chmod 777 file.txt          # rwxrwxrwx (hindari!)chmod -R 755 direktori/     # Rekursif untuk direktori# Metode Simbolikchmod u+x file.txt          # Tambah execute untuk userchmod g-w file.txt          # Hapus write untuk groupchmod o-r file.txt          # Hapus read untuk otherchmod a+x file.txt          # Tambah execute untuk semuachmod u+x,g-w file.txt      # Kombinasichmod u=rwx,g=rx,o=r file   # Set eksplisit# u = user (pemilik)# g = group# o = other# a = all (semua)# + = tambah permission# - = hapus permission# = = set permission eksplisit# ─────────────────────────────────────────────────────────────# CHMOD SPECIAL PERMISSIONS# ─────────────────────────────────────────────────────────────# SETUID (Set User ID) - 4chmod u+s /path/to/program      # Program berjalan dengan UID pemilikchmod 4755 /path/to/program     # Numerik# Contoh: /usr/bin/passwd memiliki setuid# SETGID (Set Group ID) - 2chmod g+s direktori/            # File baru mewarisi grup direktorichmod 2755 direktori/           # Numerik# STICKY BIT - 1chmod +t /tmp                   # Hanya pemilik yang bisa hapus filenyachmod 1777 /tmp                 # Numerik# Contoh: /tmp selalu memiliki sticky bit
```

### 9.3 Chown & Chgrp

```
Bash# ─────────────────────────────────────────────────────────────# CHOWN - Change Owner# ─────────────────────────────────────────────────────────────chown user file.txt             # Ubah pemilik filechown user:group file.txt       # Ubah pemilik dan grupchown :group file.txt           # Ubah hanya grupchown -R user:group direktori/  # Rekursifchown --reference=ref.txt file  # Salin kepemilikan dari file referensi# ─────────────────────────────────────────────────────────────# CHGRP - Change Group# ─────────────────────────────────────────────────────────────chgrp group file.txt            # Ubah grup filechgrp -R group direktori/       # Rekursif
```

### 9.4 UMASK

```
Bash# UMASK menentukan permission default untuk file/direktori baruumask                   # Lihat umask saat iniumask 022               # Set umask (default: 022)# Cara kerja umask:# Untuk file: 666 - umask = permission default# Untuk dir:  777 - umask = permission default# Dengan umask 022:# File baru:  666 - 022 = 644 (rw-r--r--)# Dir baru:   777 - 022 = 755 (rwxr-xr-x)# Dengan umask 027:# File baru:  666 - 027 = 640 (rw-r-----)# Dir baru:   777 - 027 = 750 (rwxr-x---)# Set umask permanen di ~/.bashrc atau /etc/profile:echo "umask 022" >> ~/.bashrc
```

### 9.5 ACL (Access Control List)

```
Bash# ACL memberikan kontrol akses yang lebih granular# Install ACL toolsapt install acl     # Ubuntu/Debianyum install acl     # CentOS/RHEL# Lihat ACLgetfacl file.txt# Set ACLsetfacl -m u:alice:rwx file.txt     # Beri alice akses rwxsetfacl -m g:developers:rx file.txt  # Beri grup developers akses rxsetfacl -m o::r file.txt            # Set permission othersetfacl -R -m u:alice:rwx dir/      # Rekursifsetfacl -x u:alice file.txt         # Hapus ACL untuk alicesetfacl -b file.txt                 # Hapus semua ACLsetfacl -d -m u:alice:rwx dir/      # Set default ACL untuk direktori
```

***

## 10. MANAJEMEN PROSES

### 10.1 Konsep Proses

```
textKONSEP PROSES LINUX:┌─────────────────────────────────────────────────────────────────┐│ PROSES adalah program yang sedang berjalan                      ││                                                                 ││ Setiap proses memiliki:                                         ││ - PID (Process ID) - angka unik                                 ││ - PPID (Parent Process ID)                                      ││ - UID (User yang menjalankan)                                   ││ - State (Running, Sleeping, Stopped, Zombie)                    ││                                                                 ││ STATE PROSES:                                                   ││ R - Running/Runnable                                            ││ S - Sleeping (interruptible)                                    ││ D - Sleeping (uninterruptible) - I/O wait                       ││ T - Stopped                                                     ││ Z - Zombie (proses selesai tapi parent belum baca exit code)    ││ I - Idle                                                        │└─────────────────────────────────────────────────────────────────┘HIERARKI PROSES:systemd (PID 1)├── NetworkManager├── sshd│   └── bash│       └── vim├── cron└── Xorg    └── gnome-shell        ├── firefox        └── terminal
```

### 10.2 Melihat Proses

```
Bash# ─────────────────────────────────────────────────────────────# PS - Process Status# ─────────────────────────────────────────────────────────────ps                          # Proses milik user saat inips -e                       # Semua prosesps -ef                      # Semua proses, format lengkapps -aux                     # Semua proses, format BSDps -u username              # Proses milik user tertentups --pid 1234               # Proses dengan PID tertentups --ppid 1234              # Child process dari PID tertentups -eo pid,ppid,cmd,%mem,%cpu --sort=-%mem   # Custom format, sort# Output ps -ef:# UID   PID  PPID  C  STIME  TTY  TIME    CMD# root    1     0  0  10:00  ?    0:01    /sbin/init#  │      │     │  │   │      │    │       └── perintah#  │      │     │  │   │      │    └── waktu CPU#  │      │     │  │   │      └── terminal#  │      │     │  │   └── waktu start#  │      │     │  └── CPU usage#  │      │     └── PPID#  │      └── PID#  └── user# ─────────────────────────────────────────────────────────────# TOP & HTOP - Monitoring Interaktif# ─────────────────────────────────────────────────────────────top                         # Monitor proses real-time# Shortcut dalam top:# q = quit# k = kill proses (masukkan PID)# r = renice (ubah prioritas)# M = sort by Memory# P = sort by CPU# 1 = toggle per-CPU view# h = helphtop                        # Versi top yang lebih bagus (install dulu)# apt install htop# ─────────────────────────────────────────────────────────────# PGREP & PIDOF# ─────────────────────────────────────────────────────────────pgrep firefox               # Cari PID berdasarkan namapgrep -l firefox            # Cari PID + namapgrep -u alice              # Proses milik alicepidof nginx                 # PID dari nginx
```

### 10.3 Mengelola Proses

```
Bash# ─────────────────────────────────────────────────────────────# KILL - Mengirim Signal ke Proses# ─────────────────────────────────────────────────────────────# Daftar signal umum:# SIGHUP  (1)  - Hangup, restart konfigurasi# SIGINT  (2)  - Interrupt (Ctrl+C)# SIGKILL (9)  - Force kill (tidak bisa diabaikan)# SIGTERM (15) - Terminate (graceful, default)# SIGSTOP (19) - Stop proses# SIGCONT (18) - Lanjutkan proses yang di-stopkill 1234                   # Kirim SIGTERM ke PID 1234kill -9 1234                # Kirim SIGKILL ke PID 1234 (force)kill -SIGTERM 1234          # Kirim SIGTERM secara eksplisitkill -l                     # List semua signalpkill firefox               # Kill proses bernama firefoxpkill -9 firefox            # Force killpkill -u alice              # Kill semua proses milik alicekillall nginx               # Kill semua proses bernama nginx# ─────────────────────────────────────────────────────────────# NICE & RENICE - Prioritas Proses# ─────────────────────────────────────────────────────────────# Nilai nice: -20 (tertinggi) sampai 19 (terendah)# Default: 0nice -n 10 perintah             # Jalankan dengan nice value 10nice -n -5 perintah             # Jalankan dengan prioritas lebih tinggirenice 10 -p 1234               # Ubah nice value proses 1234renice -n 5 -u alice            # Ubah nice semua proses alice# ─────────────────────────────────────────────────────────────# BACKGROUND & FOREGROUND# ─────────────────────────────────────────────────────────────perintah &                      # Jalankan di backgroundjobs                            # Lihat job yang berjalanjobs -l                         # Lihat job dengan PIDfg                              # Bawa job terakhir ke foregroundfg %1                           # Bawa job #1 ke foregroundbg                              # Kirim job ke backgroundbg %1                           # Kirim job #1 ke backgroundCtrl + Z                        # Stop proses ke background# NOHUP - Proses tetap berjalan setelah logoutnohup perintah &                # Jalankan yang tidak terpengaruh HUPnohup perintah > output.log &  # Redirect output# DISOWN - Pisahkan proses dari shelldisown %1                       # Pisahkan job #1 dari shelldisown -a                       # Pisahkan semua job
```

### 10.4 Informasi Sistem dan Resource

```
Bash# ─────────────────────────────────────────────────────────────# INFORMASI MEMORY & CPU# ─────────────────────────────────────────────────────────────free                        # Informasi memoryfree -h                     # Human-readable formatfree -m                     # Dalam MBfree -g                     # Dalam GBfree -s 2                   # Update setiap 2 detikvmstat                      # Virtual memory statisticsvmstat 1 5                  # 5 laporan, interval 1 detikiostat                      # I/O statisticsiostat -x 1                 # Extended, update 1 detik# ─────────────────────────────────────────────────────────────# INFORMASI CPU & LOAD# ─────────────────────────────────────────────────────────────lscpu                       # Detail CPUnproc                       # Jumlah CPU corecat /proc/loadavg           # Load average (1, 5, 15 menit)uptime                      # Uptime & load averagempstat                      # Per-CPU statisticssar                         # System Activity Report# ─────────────────────────────────────────────────────────────# STRACE & LSOF# ─────────────────────────────────────────────────────────────strace perintah             # Trace system calls sebuah prosesstrace -p 1234              # Trace proses yang berjalanlsof                        # List open fileslsof -p 1234                # File yang dibuka oleh PID 1234lsof -u alice               # File yang dibuka oleh alicelsof -i :80                 # Proses yang menggunakan port 80lsof /var/log/syslog        # Proses yang membuka file ini
```

***

## 11. MANAJEMEN PAKET

### 11.1 Sistem Paket Linux

```
textSISTEM PAKET LINUX:┌─────────────────────────────────────────────────────────────────┐│ DEBIAN/UBUNTU (.deb packages)                                   ││   Low level:  dpkg                                              ││   High level: apt, apt-get, aptitude                            │├─────────────────────────────────────────────────────────────────┤│ RED HAT/CENTOS/FEDORA (.rpm packages)                           ││   Low level:  rpm                                               ││   High level: yum (lama), dnf (baru)                           │├─────────────────────────────────────────────────────────────────┤│ ARCH LINUX (.pkg.tar.zst)                                       ││   pacman                                                        │├─────────────────────────────────────────────────────────────────┤│ UNIVERSAL PACKAGES                                              ││   Snap:   snapd (Canonical)                                     ││   Flatpak: flatpak (freedesktop)                                ││   AppImage: self-contained executable                           │└─────────────────────────────────────────────────────────────────┘
```

### 11.2 APT (Advanced Package Tool) - Debian/Ubuntu

```
Bash# ─────────────────────────────────────────────────────────────# APT - Perintah Modern (Direkomendasikan)# ─────────────────────────────────────────────────────────────# Update repositoryapt update                  # Update daftar paket dari repository# Upgrade paketapt upgrade                 # Upgrade semua paketapt full-upgrade            # Upgrade termasuk menghapus paket lamaapt dist-upgrade            # Upgrade distribusi# Instalasi paketapt install nginx           # Install nginxapt install nginx curl vim  # Install beberapa paketapt install -y nginx        # Install tanpa konfirmasiapt install ./paket.deb     # Install dari file lokal# Menghapus paketapt remove nginx            # Hapus nginx (konfigurasi tetap)apt purge nginx             # Hapus nginx + konfigurasiapt autoremove              # Hapus paket yang tidak terpakaiapt clean                   # Hapus cache paketapt autoclean               # Hapus cache paket yang sudah lama# Informasi paketapt search nginx            # Cari paketapt show nginx              # Info detail paketapt list --installed        # Daftar paket yang terinstalapt list --upgradable       # Daftar paket yang bisa diupgradeapt depends nginx           # Dependensi nginx# ─────────────────────────────────────────────────────────────# DPKG - Low Level Package Manager# ─────────────────────────────────────────────────────────────dpkg -i paket.deb           # Install dari file .debdpkg -r nginx               # Hapus paketdpkg -P nginx               # Purge paket (hapus + konfigurasi)dpkg -l                     # List semua paket yang terinstaldpkg -l nginx               # Info paket nginxdpkg -L nginx               # File yang diinstal oleh nginxdpkg -S /usr/bin/ls         # Paket mana yang memiliki file inidpkg --configure -a         # Konfigurasi ulang semua paket# ─────────────────────────────────────────────────────────────# REPOSITORY# ─────────────────────────────────────────────────────────────# Tambah repositoryadd-apt-repository ppa:nama/ppa         # Tambah PPA (Ubuntu)add-apt-repository universe             # Tambah repo universe# Konfigurasi repositorycat /etc/apt/sources.list               # File repository utamals /etc/apt/sources.list.d/             # Direktori repository tambahan# Format sources.list:# deb http://archive.ubuntu.com/ubuntu jammy main restricted# │   │                                │      └── komponen# │   │                                └── codename# │   └── URL repository# └── tipe (deb=binary, deb-src=source)
```

### 11.3 DNF/YUM - Red Hat/CentOS/Fedora

```
Bash# ─────────────────────────────────────────────────────────────# DNF (Dandified Yum) - Pengganti YUM# ─────────────────────────────────────────────────────────────# Update & upgradednf check-update            # Cek update yang tersediadnf update                  # Update semua paketdnf upgrade                 # Sama dengan update# Instalasidnf install nginx           # Install nginxdnf install -y nginx        # Install tanpa konfirmasidnf localinstall paket.rpm  # Install dari file RPM lokaldnf groupinstall "Development Tools"  # Install grup paket# Menghapusdnf remove nginx            # Hapus paketdnf autoremove              # Hapus dependensi yang tidak terpakaidnf clean all               # Hapus semua cache# Informasidnf search nginx            # Cari paketdnf info nginx              # Info paketdnf list installed          # Daftar terinstaldnf list available          # Paket yang tersediadnf provides /usr/bin/vim   # Paket apa yang menyediakan file ini# Repositorydnf repolist                # Daftar repository aktifdnf config-manager --add-repo URL   # Tambah repositorydnf config-manager --enable repo    # Aktifkan repositorydnf config-manager --disable repo   # Nonaktifkan repository# ─────────────────────────────────────────────────────────────# RPM - Red Hat Package Manager (Low Level)# ─────────────────────────────────────────────────────────────rpm -ivh paket.rpm          # Install paket (-i install, -v verbose, -h progress)rpm -Uvh paket.rpm          # Upgrade paketrpm -e paket                # Erase (hapus) paketrpm -q paket                # Query paketrpm -qa                     # Query semua paketrpm -ql nginx               # List file yang diinstal nginxrpm -qf /usr/bin/vim        # Paket yang memiliki filerpm -qi nginx               # Info paket nginxrpm --verify nginx          # Verifikasi integritas paket
```

### 11.4 Pacman - Arch Linux

```
Bash# ─────────────────────────────────────────────────────────────# PACMAN - Arch Linux Package Manager# ─────────────────────────────────────────────────────────────# Updatepacman -Sy                  # Sync databasepacman -Syu                 # Update sistem penuh# Instalasipacman -S nginx             # Install nginxpacman -S nginx curl vim    # Install beberapa paketpacman -U paket.pkg.tar.zst # Install dari file lokal# Menghapuspacman -R nginx             # Hapus paketpacman -Rs nginx            # Hapus + dependensi tidak terpakaipacman -Rns nginx           # Hapus + deps + konfigurasi# Pencarianpacman -Ss nginx            # Cari di repositorypacman -Qs nginx            # Cari di paket terinstalpacman -Si nginx            # Info paket dari repositorypacman -Qi nginx            # Info paket yang terinstalpacman -Ql nginx            # File yang diinstal# Maintenancepacman -Sc                  # Hapus cache paket lamapacman -Scc                 # Hapus semua cachepacman -Qdt                 # Orphan packages
```

### 11.5 Snap & Flatpak

```
Bash# ─────────────────────────────────────────────────────────────# SNAP# ─────────────────────────────────────────────────────────────snap find vscode            # Cari aplikasisnap install vscode         # Install VS Codesnap install --classic vscode   # Install dengan classic confinementsnap remove vscode          # Hapussnap list                   # Daftar snap terinstalsnap refresh                # Update semua snapsnap refresh vscode         # Update snap tertentusnap info vscode            # Info snap# ─────────────────────────────────────────────────────────────# FLATPAK# ─────────────────────────────────────────────────────────────flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepoflatpak install flathub org.gimp.GIMP   # Install GIMPflatpak run org.gimp.GIMP               # Jalankanflatpak uninstall org.gimp.GIMP         # Hapusflatpak list                            # Daftar terinstalflatpak update                          # Update semuaflatpak search gimp                     # Cari aplikasi
```

***

## 12. MANAJEMEN DISK & STORAGE

### 12.1 Melihat Informasi Disk

```
Bash# ─────────────────────────────────────────────────────────────# INFORMASI DISK# ─────────────────────────────────────────────────────────────df                          # Disk space filesystemdf -h                       # Human-readabledf -H                       # Human-readable (1000 bukan 1024)df -T                       # Tampilkan tipe filesystemdf /home                    # Hanya partisi /homedu                          # Disk usage direktoridu -h direktori/            # Human-readabledu -sh direktori/           # Summary (total saja)du -sh *                    # Ukuran semua item di direktori inidu -h --max-depth=1 /var    # Kedalaman 1 levellsblk                       # List block deviceslsblk -f                    # Dengan info filesystemlsblk -a                    # Termasuk disk kosongfdisk -l                    # Detail partisi (butuh root)blkid                       # UUID dan tipe filesystemparted -l                   # Detail partisi dengan parted# ─────────────────────────────────────────────────────────────# PERANGKAT DISK DI LINUX# ─────────────────────────────────────────────────────────────# /dev/sda    - SATA/SCSI disk pertama# /dev/sdb    - SATA/SCSI disk kedua# /dev/sda1   - Partisi pertama dari disk pertama# /dev/sda2   - Partisi kedua dari disk pertama# /dev/nvme0n1  - NVMe disk pertama# /dev/nvme0n1p1 - Partisi pertama NVMe# /dev/vda    - Virtual disk (KVM/QEMU)# /dev/hda    - IDE disk (lama)# /dev/mmcblk0  - MMC/SD Card
```

### 12.2 Partisi Disk

```
Bash# ─────────────────────────────────────────────────────────────# FDISK - Partisi MBR (hingga 2TB)# ─────────────────────────────────────────────────────────────fdisk /dev/sdb              # Mulai partisi disk sdb# Perintah dalam fdisk:# m = menu help# p = print partition table# n = new partition# d = delete partition# t = change partition type# w = write & exit# q = quit tanpa save# ─────────────────────────────────────────────────────────────# GDISK - Partisi GPT (>2TB, UEFI)# ─────────────────────────────────────────────────────────────gdisk /dev/sdb              # Partisi dengan GPT# ─────────────────────────────────────────────────────────────# PARTED - Universal Partition Tool# ─────────────────────────────────────────────────────────────parted /dev/sdb             # Mulai partedparted /dev/sdb print       # Tampilkan tabel partisiparted /dev/sdb mklabel gpt # Buat tabel partisi GPTparted /dev/sdb mklabel msdos # Buat tabel partisi MBRparted /dev/sdb mkpart primary ext4 1MiB 10GiB  # Buat partisiparted /dev/sdb rm 1        # Hapus partisi 1
```

### 12.3 Format Filesystem

```
Bash# ─────────────────────────────────────────────────────────────# MKFS - Make Filesystem# ─────────────────────────────────────────────────────────────mkfs.ext4 /dev/sdb1         # Format ext4mkfs.ext3 /dev/sdb1         # Format ext3mkfs.xfs /dev/sdb1          # Format XFSmkfs.btrfs /dev/sdb1        # Format Btrfsmkfs.vfat /dev/sdb1         # Format FAT32mkfs.ntfs /dev/sdb1         # Format NTFSmkfs -t ext4 /dev/sdb1      # Format dengan tipe# Opsi untuk ext4mkfs.ext4 -L "NamaLabel" /dev/sdb1     # Dengan labelmkfs.ext4 -m 1 /dev/sdb1              # Reserved blocks 1% (default 5%)mkfs.ext4 -j /dev/sdb1                # Dengan journaling# ─────────────────────────────────────────────────────────────# FSCK - Filesystem Check & Repair# ─────────────────────────────────────────────────────────────fsck /dev/sdb1              # Check filesystem (harus di-unmount dulu)fsck -y /dev/sdb1           # Auto-yes untuk semua perbaikanfsck -n /dev/sdb1           # Dry run (tidak memperbaiki)e2fsck /dev/sdb1            # Check ext2/3/4e2fsck -f /dev/sdb1         # Force checkxfs_repair /dev/sdb1        # Repair XFS
```

### 12.4 Mount & Unmount

```
Bash# ─────────────────────────────────────────────────────────────# MOUNT & UNMOUNT# ─────────────────────────────────────────────────────────────mount /dev/sdb1 /mnt/data           # Mount partisimount -t ext4 /dev/sdb1 /mnt/data  # Mount dengan tipe eksplisitmount -o ro /dev/sdb1 /mnt/data    # Mount read-onlymount -o remount,rw /mnt/data      # Remount sebagai read-writemount -a                            # Mount semua yang ada di /etc/fstabmount                               # Tampilkan semua yang ter-mountumount /mnt/data            # Unmount (cara 1)umount /dev/sdb1            # Unmount (cara 2)umount -f /mnt/data         # Force unmountumount -l /mnt/data         # Lazy unmount (nanti saat tidak dipakai)# Mount ISOmount -o loop image.iso /mnt/iso    # Mount file ISO# ─────────────────────────────────────────────────────────────# /etc/fstab - Auto Mount saat Boot# ─────────────────────────────────────────────────────────────# Format:# <device>  <mount point>  <type>  <options>  <dump>  <pass># Contoh /etc/fstab:UUID=abc123 /              ext4    defaults        0  1UUID=def456 /home          ext4    defaults        0  2UUID=ghi789 none           swap    sw              0  0/dev/sdb1   /mnt/data      ext4    defaults        0  2tmpfs       /tmp           tmpfs   defaults,noatime 0 0# Opsi mount:# defaults  = rw,suid,dev,exec,auto,nouser,async# ro        = read-only# rw        = read-write# noexec    = tidak boleh eksekusi# nosuid    = abaikan setuid bit# noatime   = tidak update atime (lebih cepat)# auto      = mount otomatis saat boot# nofail    = tidak error jika gagal mount
```

### 12.5 LVM (Logical Volume Manager)

```
Bash# ─────────────────────────────────────────────────────────────# LVM - Flexible Disk Management# ─────────────────────────────────────────────────────────────# KONSEP LVM:# Physical Volume (PV) → Volume Group (VG) → Logical Volume (LV)# Disk/Partisi → Pool Storage → Partisi Virtual Fleksibel# PHYSICAL VOLUMEpvcreate /dev/sdb1 /dev/sdc1    # Buat PVpvdisplay                        # Tampilkan info PVpvs                              # Ringkasan PVpvremove /dev/sdb1              # Hapus PV# VOLUME GROUPvgcreate vg_data /dev/sdb1 /dev/sdc1  # Buat VGvgdisplay                              # Info VGvgs                                    # Ringkasan VGvgextend vg_data /dev/sdd1            # Tambah disk ke VGvgreduce vg_data /dev/sdd1            # Hapus disk dari VGvgremove vg_data                       # Hapus VG# LOGICAL VOLUMElvcreate -L 10G -n lv_home vg_data    # Buat LV 10GBlvcreate -l 100%FREE -n lv_data vg_d  # Gunakan semua ruang kosonglvdisplay                              # Info LVlvs                                    # Ringkasan LVlvextend -L +5G /dev/vg_data/lv_home  # Tambah 5GB ke LVlvreduce -L -2G /dev/vg_data/lv_home  # Kurangi 2GB dari LVlvremove /dev/vg_data/lv_home         # Hapus LV# Resize filesystem setelah extend LVresize2fs /dev/vg_data/lv_home        # Untuk ext4xfs_growfs /mount/point               # Untuk XFS# SNAPSHOT LVMlvcreate -s -L 5G -n snap_home /dev/vg_data/lv_home  # Buat snapshotlvconvert --merge /dev/vg_data/snap_home              # Restore snapshot
```

### 12.6 RAID

```
Bash# ─────────────────────────────────────────────────────────────# MDADM - Software RAID# ─────────────────────────────────────────────────────────────# RAID Levels:# RAID 0 - Striping (performa, tanpa redundansi)# RAID 1 - Mirroring (redundansi, butuh 2x ruang)# RAID 5 - Striping + Parity (minimal 3 disk)# RAID 6 - Striping + Double Parity (minimal 4 disk)# RAID 10 - Kombinasi RAID 1+0 (minimal 4 disk)# Install mdadmapt install mdadm# Buat RAID 1mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/sdb /dev/sdc# Buat RAID 5mdadm --create /dev/md0 --level=5 --raid-devices=3 /dev/sdb /dev/sdc /dev/sdd# Cek status RAIDcat /proc/mdstatmdadm --detail /dev/md0# Format dan mount RAIDmkfs.ext4 /dev/md0mount /dev/md0 /mnt/raid# Simpan konfigurasi RAIDmdadm --detail --scan >> /etc/mdadm/mdadm.confupdate-initramfs -u
```

***

## 13. JARINGAN DI LINUX

### 13.1 Konfigurasi Jaringan

```
Bash# ─────────────────────────────────────────────────────────────# INFORMASI JARINGAN# ─────────────────────────────────────────────────────────────ip addr                     # Tampilkan interface & IP (modern)ip addr show eth0           # Info interface eth0ip link                     # Status interfaceip route                    # Tabel routingip route show               # Sama dengan ip routeifconfig                    # Cara lama (deprecated)ifconfig eth0               # Info interface eth0hostname                    # Nama hosthostname -I                 # Semua alamat IPhostname -f                 # Fully Qualified Domain Name (FQDN)cat /etc/hosts              # File hosts lokalcat /etc/resolv.conf        # Konfigurasi DNScat /etc/network/interfaces # Konfigurasi jaringan (Debian)nmcli                       # NetworkManager CLI# ─────────────────────────────────────────────────────────────# KONFIGURASI IP# ─────────────────────────────────────────────────────────────# Menggunakan ip command (sementara - hilang saat reboot)ip addr add 192.168.1.100/24 dev eth0    # Tambah IPip addr del 192.168.1.100/24 dev eth0   # Hapus IPip link set eth0 up                       # Aktifkan interfaceip link set eth0 down                     # Nonaktifkan interfaceip route add default via 192.168.1.1     # Tambah default gatewayip route add 10.0.0.0/8 via 192.168.1.1 # Tambah rute spesifikip route del default                      # Hapus default gateway# Menggunakan NetworkManager (Ubuntu modern)nmcli device status                       # Status devicenmcli connection show                     # Daftar koneksinmcli connection up "Wired Connection 1"  # Aktifkan koneksinmcli connection down "Wired Connection 1" # Nonaktifkan koneksinmcli connection modify eth0 ipv4.addresses "192.168.1.100/24"nmcli connection modify eth0 ipv4.gateway "192.168.1.1"nmcli connection modify eth0 ipv4.dns "8.8.8.8,8.8.4.4"nmcli connection modify eth0 ipv4.method manual  # Static IPnmcli connection reload# Konfigurasi statis Ubuntu (Netplan - /etc/netplan/*.yaml)cat /etc/netplan/00-installer-config.yaml# Contoh konfigurasi Netplan:# network:#   version: 2#   ethernets:#     eth0:#       dhcp4: no#       addresses:#         - 192.168.1.100/24#       gateway4: 192.168.1.1#       nameservers:#         addresses: [8.8.8.8, 8.8.4.4]netplan apply               # Terapkan konfigurasi Netplannetplan try                 # Test konfigurasi (rollback otomatis)
```

### 13.2 Perintah Diagnostik Jaringan

```
Bash# ─────────────────────────────────────────────────────────────# PING, TRACEROUTE, DNS# ─────────────────────────────────────────────────────────────ping google.com             # Test konektivitasping -c 4 google.com        # Ping 4 kali sajaping -i 0.5 google.com      # Interval 0.5 detikping6 google.com            # Ping IPv6ping -s 1400 google.com     # Ukuran paket 1400 bytestraceroute google.com       # Trace jalur ke tujuantraceroute -n google.com    # Tanpa resolusi namamtr google.com              # Kombinasi ping + traceroute (interaktif)nslookup google.com         # Query DNSnslookup -type=MX gmail.com # Query record MXdig google.com              # Query DNS detaildig google.com A            # Query record A (IPv4)dig google.com MX           # Query record MXdig google.com @8.8.8.8    # Query menggunakan DNS tertentudig -x 8.8.8.8              # Reverse DNS lookuphost google.com             # Query DNS sederhana# ─────────────────────────────────────────────────────────────# PORT & KONEKSI# ─────────────────────────────────────────────────────────────netstat -tuln               # Port yang sedang listennetstat -tulpn              # Dengan nama prosesnetstat -an                 # Semua koneksiss -tuln                    # Modern pengganti netstatss -tulpn                   # Dengan prosesss -s                       # Statistik socketlsof -i                     # Semua koneksi jaringanlsof -i :80                 # Koneksi pada port 80lsof -i tcp                 # Koneksi TCP# ─────────────────────────────────────────────────────────────# CURL & WGET# ─────────────────────────────────────────────────────────────curl URL                            # Ambil konten URLcurl -o file.html URL               # Simpan ke filecurl -O https://example.com/file    # Download dengan nama aslicurl -L URL                         # Ikuti redirectcurl -I URL                         # Hanya header HTTPcurl -v URL                         # Verbose (debug)curl -s URL                         # Silent modecurl -X POST -d "data" URL          # POST requestcurl -H "Content-Type: application/json" -d '{"key":"val"}' URLcurl -u user:pass URL               # Basic authenticationcurl --proxy proxy:port URL         # Gunakan proxywget URL                            # Download filewget -O namafile URL                # Download dengan nama tertentuwget -c URL                         # Resume downloadwget -r URL                         # Download rekursifwget -q URL                         # Quiet modewget --limit-rate=1M URL            # Batasi kecepatan 1 MB/swget -b URL                         # Download di background# ─────────────────────────────────────────────────────────────# SSH - Secure Shell# ─────────────────────────────────────────────────────────────ssh user@host                       # Koneksi SSHssh -p 2222 user@host              # Port customssh -i ~/.ssh/private_key user@host # Gunakan private keyssh -L 8080:localhost:80 user@host  # Local port forwardingssh -R 8080:localhost:80 user@host  # Remote port forwardingssh -D 1080 user@host              # Dynamic SOCKS proxyssh -X user@host                    # X11 forwarding# Manajemen SSH Keyssh-keygen                          # Generate key pairssh-keygen -t rsa -b 4096          # RSA 4096-bitssh-keygen -t ed25519              # ED25519 (direkomendasikan)ssh-copy-id user@host              # Copy public key ke servercat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys  # Manual copy key# SCP - Secure Copyscp file.txt user@host:/path/      # Upload filescp user@host:/path/file.txt .     # Download filescp -r direktori/ user@host:/path/ # Upload direktoriscp -P 2222 file user@host:/path/  # Port custom# RSYNC - Remote Syncrsync -av source/ user@host:dest/  # Sync direktori ke remotersync -av user@host:source/ dest/  # Sync dari remotersync -avz source/ user@host:dest/ # Dengan kompresirsync -av --delete source/ dest/   # Mirror (hapus file tidak ada)rsync -av --progress source/ dest/ # Tampilkan progressrsync -av -e "ssh -p 2222" source/ user@host:dest/  # Custom SSH port
```

### 13.3 Firewall

```
Bash# ─────────────────────────────────────────────────────────────# UFW (Uncomplicated Firewall) - Ubuntu# ─────────────────────────────────────────────────────────────ufw status                  # Status firewallufw status verbose          # Status detailufw enable                  # Aktifkan firewallufw disable                 # Nonaktifkan firewallufw reset                   # Reset ke default# Rules dasarufw allow ssh               # Izinkan SSH (port 22)ufw allow 80                # Izinkan port 80 (HTTP)ufw allow 443               # Izinkan port 443 (HTTPS)ufw allow 80/tcp            # Izinkan port 80 TCPufw allow from 192.168.1.0/24   # Izinkan dari subnetufw deny 23                 # Blokir port 23 (telnet)ufw delete allow 80         # Hapus ruleufw default deny incoming   # Default deny masukufw default allow outgoing  # Default allow keluar# Numbered rulesufw status numbered         # Tampilkan dengan nomorufw delete 2               # Hapus rule nomor 2# Limit (rate limiting)ufw limit ssh               # Batasi koneksi SSH# ─────────────────────────────────────────────────────────────# IPTABLES - Advanced Firewall# ─────────────────────────────────────────────────────────────# Melihat rulesiptables -L                         # List rulesiptables -L -n -v                   # Verbose dengan angkaiptables -L INPUT -n -v             # Hanya chain INPUT# Struktur iptables:# Tables: filter, nat, mangle, raw# Chains: INPUT, OUTPUT, FORWARD (filter), PREROUTING, POSTROUTING (nat)# Rules: kondisi + action (ACCEPT, DROP, REJECT, LOG)# Rules dasariptables -A INPUT -p tcp --dport 22 -j ACCEPT      # Allow SSHiptables -A INPUT -p tcp --dport 80 -j ACCEPT      # Allow HTTPiptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPTiptables -A INPUT -j DROP                           # Drop sisanyaiptables -A INPUT -i lo -j ACCEPT                  # Allow loopback# Hapus rulesiptables -D INPUT -p tcp --dport 80 -j ACCEPT      # Hapus ruleiptables -F                                         # Flush semua rulesiptables -F INPUT                                   # Flush chain INPUT# Simpan dan load rulesiptables-save > /etc/iptables/rules.v4             # Simpaniptables-restore < /etc/iptables/rules.v4          # Load# ─────────────────────────────────────────────────────────────# NFTABLES - Modern Firewall (pengganti iptables)# ─────────────────────────────────────────────────────────────nft list tables             # List tabelnft list ruleset            # List semua rulesnft add table inet filter   # Buat tabelnft add chain inet filter input { type filter hook input priority 0 \; }nft add rule inet filter input tcp dport 22 acceptnft list ruleset > /etc/nftables.conf   # Simpan konfigurasi
```

***

## 14. SHELL & SCRIPTING

### 14.1 Jenis Shell

```
textJENIS SHELL DI LINUX:┌──────────┬────────────────────────────────────────────────────┐│ SHELL    │ KETERANGAN                                         │├──────────┼────────────────────────────────────────────────────┤│ sh       │ Bourne Shell - shell original Unix                 ││ bash     │ Bourne Again Shell - paling populer               ││ zsh      │ Z Shell - sangat extensible, populer              ││ fish     │ Friendly Interactive Shell - user-friendly        ││ ksh      │ Korn Shell - unix tradisional                     ││ csh/tcsh │ C Shell - syntax mirip C                          ││ dash     │ Debian Almquist Shell - ringan, cepat             │└──────────┴────────────────────────────────────────────────────┘
```

### 14.2 Fitur Shell (Bash)

```
Bash# ─────────────────────────────────────────────────────────────# VARIABEL# ─────────────────────────────────────────────────────────────# Variabel biasanama="Linux"                # Set variabel (tanpa spasi di sekitar =)echo $nama                  # Gunakan variabelecho ${nama}                # Cara eksplisitecho "Halo $nama"           # Dalam string# Variabel environmentexport PATH="$PATH:/usr/local/bin"  # Export ke environmentprintenv                    # Tampilkan semua variabel envprintenv PATH               # Tampilkan PATHenv                         # Semua environment variable# Variabel khususecho $?                     # Exit code perintah terakhir (0=sukses)echo $$                     # PID shell saat iniecho $!                     # PID proses background terakhirecho $0                     # Nama scriptecho $1 $2 $3               # Argumen scriptecho $@                     # Semua argumenecho $#                     # Jumlah argumenecho $BASH_VERSION          # Versi bashecho $HOSTNAME              # Nama hostecho $USER                  # User saat iniecho $HOME                  # Home directoryecho $PWD                   # Direktori saat iniecho $RANDOM                # Angka random# ─────────────────────────────────────────────────────────────# INPUT/OUTPUT REDIRECTION# ─────────────────────────────────────────────────────────────# Standard Streams:# 0 = stdin  (input)# 1 = stdout (output)# 2 = stderr (error)perintah > file.txt         # Redirect stdout ke file (overwrite)perintah >> file.txt        # Redirect stdout ke file (append)perintah < file.txt         # Ambil stdin dari fileperintah 2> error.txt       # Redirect stderr ke fileperintah 2>&1               # Redirect stderr ke stdoutperintah > output.txt 2>&1  # Redirect keduanya ke fileperintah &> file.txt        # Cara singkat redirect semuaperintah > /dev/null 2>&1   # Buang semua output# Pipeperintah1 | perintah2       # Output perintah1 jadi input perintah2perintah1 | perintah2 | perintah3  # Chain pipe# Contoh pipe:ls -la | grep ".txt"ps aux | grep nginxcat /etc/passwd | cut -d: -f1 | sortcat file.txt | wc -l# HERE Documentcat << EOFIni adalahkonten multilineEOFcat << EOF > file.txtIni ditulis ke fileEOF# ─────────────────────────────────────────────────────────────# WILDCARD & GLOBBING# ─────────────────────────────────────────────────────────────ls *.txt                    # Semua file berekstensi .txtls file?.txt                # ? cocok dengan satu karakterls file[1-3].txt            # [] cocok dengan salah satu karakterls file[!1].txt             # [!] cocok dengan selain karakterls {*.txt,*.pdf}            # Brace expansion# ─────────────────────────────────────────────────────────────# HISTORY# ─────────────────────────────────────────────────────────────history                     # Riwayat perintahhistory 20                  # 20 perintah terakhir!!                          # Ulangi perintah terakhir!n                          # Ulangi perintah ke-n dari history!ls                         # Ulangi perintah terakhir yang dimulai "ls"Ctrl + R                    # Cari dalam history (reverse search)history -c                  # Hapus history
```

### 14.3 Bash Scripting

```
Bash#!/bin/bash# ─────────────────────────────────────────────────────────────# BASH SCRIPTING - PANDUAN LENGKAP# ─────────────────────────────────────────────────────────────# Shebang harus di baris pertama#!/bin/bash# ─────────────────────────────────────────────────────────────# CONDITIONAL (IF-ELSE)# ─────────────────────────────────────────────────────────────# Sintaks dasarif [ kondisi ]; then    perintahelif [ kondisi_lain ]; then    perintahelse    perintahfi# Operator perbandingan STRING:# == atau =  (sama)# !=         (tidak sama)# -z         (string kosong)# -n         (string tidak kosong)# <          (lebih kecil alfabet)# >          (lebih besar alfabet)# Operator perbandingan NUMERIK:# -eq   (equal)# -ne   (not equal)# -lt   (less than)# -le   (less than or equal)# -gt   (greater than)# -ge   (greater than or equal)# Operator FILE:# -f file   (ada dan merupakan file biasa)# -d file   (ada dan merupakan direktori)# -e file   (ada)# -r file   (bisa dibaca)# -w file   (bisa ditulis)# -x file   (bisa dieksekusi)# -s file   (tidak kosong)# Contoh:if [ "$nama" == "Linux" ]; then    echo "Halo Linux!"fiif [ $angka -gt 10 ]; then    echo "Lebih dari 10"elif [ $angka -eq 10 ]; then    echo "Sama dengan 10"else    echo "Kurang dari 10"fiif [ -f /etc/passwd ]; then    echo "File ada"fi# Double bracket (lebih powerful)if [[ $string =~ ^[0-9]+$ ]]; then    echo "String adalah angka"fi# ─────────────────────────────────────────────────────────────# LOOP# ─────────────────────────────────────────────────────────────# FOR loopfor i in 1 2 3 4 5; do    echo "Angka: $i"donefor file in *.txt; do    echo "File: $file"donefor i in {1..10}; do    echo $idonefor ((i=0; i<10; i++)); do    echo $idone# WHILE loopi=1while [ $i -le 10 ]; do    echo $i    ((i++))done# UNTIL loopi=1until [ $i -gt 10 ]; do    echo $i    ((i++))done# Loop dengan break & continuefor i in {1..10}; do    if [ $i -eq 5 ]; then        continue        # Skip angka 5    fi    if [ $i -eq 8 ]; then        break           # Stop di 8    fi    echo $idone# ─────────────────────────────────────────────────────────────# FUNGSI# ─────────────────────────────────────────────────────────────# Definisi fungsifunction sapa() {    local nama="$1"     # Parameter pertama    echo "Halo, $nama!"    return 0            # Return value}# Cara alternatifcek_file() {    if [ -f "$1" ]; then        echo "File $1 ada"        return 0    else        echo "File $1 tidak ada"        return 1    fi}# Panggil fungsisapa "Dunia"cek_file "/etc/passwd"# ─────────────────────────────────────────────────────────────# CASE STATEMENT# ─────────────────────────────────────────────────────────────case "$pilihan" in    "a"|"A")        echo "Pilihan A"        ;;    "b"|"B")        echo "Pilihan B"        ;;    *)        echo "Pilihan tidak valid"        ;;esac# ─────────────────────────────────────────────────────────────# ARRAY# ─────────────────────────────────────────────────────────────# Deklarasi arraybuah=("apel" "mangga" "jeruk" "pisang")nama[0]="Alice"nama[1]="Bob"# Akses arrayecho ${buah[0]}             # Elemen pertamaecho ${buah[@]}             # Semua elemenecho ${#buah[@]}            # Jumlah elemenecho ${buah[@]:1:2}         # Slice (dari index 1, ambil 2)# Loop arrayfor item in "${buah[@]}"; do    echo $itemdone# Associative array (seperti dict/hash)declare -A datadata["nama"]="Alice"data["umur"]="25"echo ${data["nama"]}echo ${!data[@]}            # Semua key# ─────────────────────────────────────────────────────────────# STRING MANIPULATION# ─────────────────────────────────────────────────────────────str="Hello World Linux"echo ${#str}                # Panjang stringecho ${str:6}               # Substring dari index 6echo ${str:6:5}             # Substring 5 karakter dari index 6echo ${str/World/Linux}     # Ganti pertamaecho ${str//l/L}            # Ganti semuaecho ${str,,}               # Lowercaseecho ${str^^}               # Uppercaseecho ${str%Linux}           # Hapus suffixecho ${str#Hello}           # Hapus prefix# Default valueecho ${var:-"default"}      # Gunakan default jika var kosongecho ${var:="default"}      # Set var ke default jika kosongecho ${var:?"Error msg"}    # Error jika var kosong# ─────────────────────────────────────────────────────────────# ARITHMETIC# ─────────────────────────────────────────────────────────────# Metode 1: $(( ))a=5b=3echo $((a + b))             # 8echo $((a - b))             # 2echo $((a * b))             # 15echo $((a / b))             # 1 (integer division)echo $((a % b))             # 2 (modulo)echo $((a ** b))            # 125 (pangkat)((a++))                     # Increment((a--))                     # Decrement((a += 5))                  # a = a + 5# Metode 2: expr (lama)expr 5 + 3# Metode 3: bc (floating point)echo "scale=2; 10/3" | bc   # 3.33# ─────────────────────────────────────────────────────────────# CONTOH SCRIPT LENGKAP# ─────────────────────────────────────────────────────────────#!/bin/bash# Script: backup.sh# Deskripsi: Backup otomatis direktoriset -e          # Exit jika ada errorset -u          # Error jika variabel tidak didefinisikanset -o pipefail # Error jika pipe gagal# KonfigurasiSOURCE_DIR="/home/user/data"BACKUP_DIR="/backup"DATE=$(date +%Y%m%d_%H%M%S)BACKUP_NAME="backup_${DATE}.tar.gz"LOG_FILE="/var/log/backup.log"# Fungsi logginglog() {    echo "[$(date '+%Y-%m-%d %H:%M:%S')] $1" | tee -a "$LOG_FILE"}# Fungsi error handlingerror_exit() {    log "ERROR: $1"    exit 1}# Validasi[ -d "$SOURCE_DIR" ] || error_exit "Direktori source tidak ada: $SOURCE_DIR"[ -d "$BACKUP_DIR" ] || mkdir -p "$BACKUP_DIR" || error_exit "Gagal buat direktori backup"# Proses backuplog "Memulai backup $SOURCE_DIR..."if tar -czf "${BACKUP_DIR}/${BACKUP_NAME}" "$SOURCE_DIR"; then    SIZE=$(du -sh "${BACKUP_DIR}/${BACKUP_NAME}" | cut -f1)    log "Backup berhasil: ${BACKUP_NAME} (${SIZE})"else    error_exit "Backup gagal!"fi# Hapus backup lebih dari 7 harifind "$BACKUP_DIR" -name "backup_*.tar.gz" -mtime +7 -deletelog "Backup lama (>7 hari) telah dihapus"log "Selesai."exit 0
```

### 14.4 Cron Job (Penjadwalan Tugas)

```
Bash# ─────────────────────────────────────────────────────────────# CRON - Penjadwalan Otomatis# ─────────────────────────────────────────────────────────────crontab -e                  # Edit cron job penggunacrontab -l                  # List cron jobcrontab -r                  # Hapus semua cron jobcrontab -u alice -l         # List cron job alice (root only)# FORMAT CRONTAB:# ┌─────────────── Menit (0-59)# │ ┌───────────── Jam (0-23)# │ │ ┌─────────── Hari (1-31)# │ │ │ ┌───────── Bulan (1-12)# │ │ │ │ ┌─────── Hari minggu (0-7, 0&7=Minggu)# │ │ │ │ │# * * * * * perintah# CONTOH CRON JOB:# Setiap menit* * * * * /path/to/script.sh# Setiap jam tepat (menit 0)0 * * * * /path/to/script.sh# Setiap hari jam 2:30 pagi30 2 * * * /path/to/backup.sh# Setiap hari Senin jam 8:000 8 * * 1 /path/to/script.sh# Setiap hari kerja (Senin-Jumat) jam 9:000 9 * * 1-5 /path/to/script.sh# Pertama setiap bulan0 0 1 * * /path/to/monthly.sh# Setiap 5 menit*/5 * * * * /path/to/script.sh# Setiap 2 jam0 */2 * * * /path/to/script.sh# Spesial@reboot /path/to/startup.sh    # Saat reboot@daily /path/to/daily.sh       # Sekali sehari (sama dengan 0 0 * * *)@weekly /path/to/weekly.sh     # Sekali seminggu@monthly /path/to/monthly.sh   # Sekali sebulan@yearly /path/to/yearly.sh     # Sekali setahun# Direktori cron sistemls /etc/cron.daily/            # Script yang berjalan harianls /etc/cron.weekly/           # Script yang berjalan mingguanls /etc/cron.monthly/          # Script yang berjalan bulananls /etc/cron.d/                # Konfigurasi cron tambahan# Systemd timer (modern pengganti cron)systemctl list-timers          # List semua timer
```

***

## 15. MANAJEMEN LAYANAN (SYSTEMD)

### 15.1 Pengantar Systemd

```
textSYSTEMD adalah sistem init modern yang menggantikan SysV Init.Systemd adalah PID 1 (proses pertama yang berjalan saat boot).Komponen Systemd:- systemctl   → Kontrol layanan- journalctl  → Manajemen log- systemd-analyze → Analisis waktu boot- hostnamectl → Manajemen hostname- timedatectl → Manajemen waktu- localectl   → Manajemen locale- loginctl    → Manajemen session login- networkctl  → Manajemen jaringan
```

### 15.2 Systemctl - Manajemen Layanan

```
Bash# ─────────────────────────────────────────────────────────────# STATUS & INFORMASI# ─────────────────────────────────────────────────────────────systemctl status nginx              # Status layanan nginxsystemctl status                    # Status semua unitsystemctl list-units                # List semua unit aktifsystemctl list-units --type=service # Hanya servicesystemctl list-units --failed       # Unit yang gagalsystemctl list-unit-files           # Semua file unitsystemctl is-active nginx           # Cek apakah nginx aktifsystemctl is-enabled nginx          # Cek apakah nginx auto-startsystemctl is-failed nginx           # Cek apakah nginx gagal# ─────────────────────────────────────────────────────────────# KONTROL LAYANAN# ─────────────────────────────────────────────────────────────systemctl start nginx               # Mulai layanansystemctl stop nginx                # Hentikan layanansystemctl restart nginx             # Restart layanansystemctl reload nginx              # Reload konfigurasisystemctl reload-or-restart nginx   # Reload jika bisa, else restart# ─────────────────────────────────────────────────────────────# ENABLE/DISABLE (Auto-start saat boot)# ─────────────────────────────────────────────────────────────systemctl enable nginx              # Enable layanan saat bootsystemctl disable nginx             # Disable layanan saat bootsystemctl enable --now nginx        # Enable + start sekarangsystemctl disable --now nginx       # Disable + stop sekarangsystemctl mask nginx                # Masking (tidak bisa diaktifkan)systemctl unmask nginx              # Hapus masking# ─────────────────────────────────────────────────────────────# POWER MANAGEMENT# ─────────────────────────────────────────────────────────────systemctl poweroff              # Matikan sistemsystemctl reboot                # Restart sistemsystemctl halt                  # Halt sistemsystemctl suspend               # Suspend (sleep)systemctl hibernate             # Hibernate# ─────────────────────────────────────────────────────────────# TARGETS (pengganti runlevel)# ─────────────────────────────────────────────────────────────systemctl get-default           # Target defaultsystemctl set-default graphical.target  # Set target defaultsystemctl isolate multi-user.target     # Pindah target saat ini# Target umum:# poweroff.target    = matikan# rescue.target      = mode rescue (runlevel 1)# multi-user.target  = multi-user tanpa GUI (runlevel 3)# graphical.target   = GUI (runlevel 5)# reboot.target      = reboot
```

### 15.3 Membuat Service Custom

```
ini# Contoh file service: /etc/systemd/system/myapp.service[Unit]Description=My Application ServerDocumentation=https://docs.myapp.comAfter=network.target postgresql.serviceRequires=postgresql.service[Service]Type=simpleUser=myappGroup=myappWorkingDirectory=/opt/myappExecStart=/opt/myapp/bin/server --config /etc/myapp/config.yamlExecReload=/bin/kill -HUP $MAINPIDRestart=on-failureRestartSec=5sStandardOutput=journalStandardError=journalSyslogIdentifier=myappEnvironment="NODE_ENV=production"EnvironmentFile=/etc/myapp/environment# SecurityNoNewPrivileges=truePrivateTmp=trueProtectSystem=strictReadWritePaths=/var/lib/myapp /var/log/myapp[Install]WantedBy=multi-user.target
```

```
Bash# Setelah membuat file service:systemctl daemon-reload         # Reload konfigurasi systemdsystemctl enable myapp          # Enable servicesystemctl start myapp           # Start servicesystemctl status myapp          # Cek status
```

### 15.4 Journalctl - Manajemen Log

```
Bash# ─────────────────────────────────────────────────────────────# JOURNALCTL - Systemd Journal# ─────────────────────────────────────────────────────────────journalctl                          # Semua logjournalctl -f                       # Follow (real-time)journalctl -n 100                   # 100 baris terakhirjournalctl -u nginx                 # Log layanan nginxjournalctl -u nginx -f              # Follow log nginxjournalctl --since "2024-01-01"    # Sejak tanggaljournalctl --since "1 hour ago"    # 1 jam terakhirjournalctl --since "09:00" --until "10:00"  # Range waktujournalctl -p err                   # Hanya errorjournalctl -p warning..err          # Warning sampai errorjournalctl _PID=1234                # Log dari PID tertentujournalctl _UID=1000                # Log dari UID tertentujournalctl -b                       # Log boot saat inijournalctl -b -1                    # Log boot sebelumnyajournalctl --list-boots             # List semua bootjournalctl -k                       # Hanya kernel messagesjournalctl --disk-usage             # Ukuran journal di diskjournalctl --vacuum-size=1G         # Batasi ukuran journaljournalctl --vacuum-time=1month     # Hapus log lebih dari 1 bulanjournalctl -o json                  # Output format JSONjournalctl -o short-iso             # Dengan timestamp ISO
```

***

## 16. KEAMANAN LINUX

### 16.1 Hardening Sistem

```
Bash# ─────────────────────────────────────────────────────────────# HARDENING DASAR# ─────────────────────────────────────────────────────────────# 1. Update sistem secara rutinapt update && apt upgrade -y# 2. Disable root SSH login# Edit /etc/ssh/sshd_config:# PermitRootLogin no# PasswordAuthentication no  (gunakan key saja)# Protocol 2# MaxAuthTries 3# AllowUsers alice bob# 3. Konfigurasi SSH yang amanvim /etc/ssh/sshd_config# Konfigurasi rekomendasi:# Port 2222                      # Ubah port default# PermitRootLogin no             # Disable root login# MaxAuthTries 3                 # Maksimal 3 percobaan# PubkeyAuthentication yes       # Gunakan public key# PasswordAuthentication no      # Disable password auth# X11Forwarding no               # Disable X11# AllowUsers alice bob           # Hanya user tertentusystemctl restart sshd# 4. Konfigurasi firewallufw default deny incomingufw default allow outgoingufw allow 2222/tcp      # SSH (port yang diubah)ufw allow 80/tcp        # HTTPufw allow 443/tcp       # HTTPSufw enable# 5. Fail2ban - Proteksi Brute Forceapt install fail2bancat > /etc/fail2ban/jail.local << 'EOF'[DEFAULT]bantime = 3600findtime = 600maxretry = 3[sshd]enabled = trueport = 2222logpath = /var/log/auth.logEOFsystemctl enable fail2bansystemctl start fail2banfail2ban-client status              # Status fail2banfail2ban-client status sshd        # Status jail SSHfail2ban-client set sshd unbanip IP  # Unban IP# 6. Disable layanan yang tidak perlusystemctl list-units --type=service --state=activesystemctl disable --now telnet      # Matikan telnetsystemctl disable --now rsh         # Matikan rsh# 7. Set password policy# Edit /etc/security/pwquality.conf:# minlen = 12# minclass = 3# maxrepeat = 3# 8. Limit akses sudpkg-statoverride --update --add root sudo 4750 /bin/su# 9. Audit dengan Lynisapt install lynislynis audit system              # Scan keamanan sistem
```

### 16.2 SELinux & AppArmor

```
Bash# ─────────────────────────────────────────────────────────────# APPARMOR (Ubuntu/Debian)# ─────────────────────────────────────────────────────────────apparmor_status                     # Status AppArmoraa-status                           # Sama dengan apparmor_statusaa-enforce /etc/apparmor.d/usr.bin.firefox  # Set ke enforce modeaa-complain /etc/apparmor.d/usr.bin.firefox # Set ke complain modeaa-disable /etc/apparmor.d/usr.bin.firefox  # Disable profil# ─────────────────────────────────────────────────────────────# SELINUX (Red Hat/CentOS)# ─────────────────────────────────────────────────────────────getenforce                          # Cek mode SELinuxsetenforce 0                        # Set ke Permissive (sementara)setenforce 1                        # Set ke Enforcing (sementara)# Edit /etc/selinux/config untuk permanen:# SELINUX=enforcing    # atau permissive atau disabledsestatus                            # Status SELinux detaills -Z file.txt                      # Lihat SELinux context fileps auxZ                             # Lihat SELinux context proseschcon -t httpd_sys_content_t file   # Ubah contextrestorecon -rv /var/www/            # Restore context defaultaudit2why < /var/log/audit/audit.log  # Analisis deny messagesaudit2allow -M mypol < /var/log/audit/audit.log  # Buat policy
```

### 16.3 Enkripsi

```
Bash# ─────────────────────────────────────────────────────────────# GPG - GNU Privacy Guard# ─────────────────────────────────────────────────────────────# Generate key pairgpg --gen-keygpg --full-gen-key              # Opsi lebih lengkap# Manajemen keygpg --list-keys                 # List semua keygpg --list-secret-keys          # List private keysgpg --export -a "nama@email"    # Export public keygpg --import pubkey.asc         # Import public keygpg --delete-key email          # Hapus public key# Enkripsi & Dekripsigpg -e -r "recipient@email" file.txt    # Enkripsi untuk recipientgpg -d file.txt.gpg                     # Dekripsigpg -e -s file.txt                      # Enkripsi + sign# Signinggpg -s file.txt                         # Sign filegpg --verify file.txt.gpg               # Verify signaturegpg --clearsign file.txt                # Clear-text signature# ─────────────────────────────────────────────────────────────# LUKS - Full Disk Encryption# ─────────────────────────────────────────────────────────────# Install cryptsetupapt install cryptsetup# Setup LUKS pada partisicryptsetup luksFormat /dev/sdb1         # Format dengan LUKScryptsetup open /dev/sdb1 namasaya     # Buka (decrypt)mkfs.ext4 /dev/mapper/namasaya          # Format filesystemmount /dev/mapper/namasaya /mnt/secure  # Mountcryptsetup close namasaya              # Tutup (encrypt kembali)# Info LUKScryptsetup luksDump /dev/sdb1          # Info LUKS headercryptsetup luksAddKey /dev/sdb1        # Tambah passphrasecryptsetup luksRemoveKey /dev/sdb1     # Hapus passphrase# ─────────────────────────────────────────────────────────────# OPENSSL# ─────────────────────────────────────────────────────────────# Generate self-signed certificateopenssl req -x509 -nodes -days 365 -newkey rsa:2048 \    -keyout server.key -out server.crt# Generate CSR (Certificate Signing Request)openssl req -new -newkey rsa:2048 -nodes -keyout server.key -out server.csr# Enkripsi fileopenssl enc -aes-256-cbc -salt -in file.txt -out file.enc -k passwordopenssl enc -d -aes-256-cbc -in file.enc -out file.txt -k password# Hash fileopenssl dgst -sha256 file.txt          # SHA256 hashmd5sum file.txt                        # MD5 hashsha256sum file.txt                     # SHA256 hashsha512sum file.txt                     # SHA512 hash
```

### 16.4 Audit & Monitoring Keamanan

```
Bash# ─────────────────────────────────────────────────────────────# AUDITD - Linux Audit System# ─────────────────────────────────────────────────────────────apt install auditdsystemctl enable auditdsystemctl start auditd# Tambah aturan auditauditctl -a always,exit -F arch=b64 -S open -k file_open   # Monitor file openauditctl -w /etc/passwd -p wa -k passwd_changes            # Monitor /etc/passwdauditctl -w /etc/sudoers -p rwa -k sudoers                 # Monitor sudoersauditctl -l                                                 # List rules saat ini# Baca log auditausearch -k file_open               # Cari log berdasarkan keyausearch -ui 1000                   # Log dari UID 1000ausearch -x /bin/su                 # Log dari program suaureport                            # Laporan auditaureport --login                    # Laporan loginaureport --failed                   # Laporan yang gagalaureport --summary                  # Ringkasan# Konfigurasi permanen (/etc/audit/rules.d/audit.rules)cat > /etc/audit/rules.d/audit.rules << 'EOF'# Delete all rules-D# Buffer size-b 8192# Monitor authentication-w /var/log/auth.log -p wa -k auth_log# Monitor important config files-w /etc/passwd -p wa -k passwd-w /etc/shadow -p wa -k shadow-w /etc/sudoers -p wa -k sudoers-w /etc/ssh/sshd_config -p wa -k sshd_config# Monitor privileged commands-a always,exit -F path=/usr/bin/sudo -F perm=x -k sudo_commandsEOF# ─────────────────────────────────────────────────────────────# AIDE - Advanced Intrusion Detection Environment# ─────────────────────────────────────────────────────────────apt install aideaide --init                         # Inisialisasi databasecp /var/lib/aide/aide.db.new /var/lib/aide/aide.db  # Aktifkan DBaide --check                        # Cek perubahan fileaide --update                       # Update database# ─────────────────────────────────────────────────────────────# RKHUNTER & CHKROOTKIT# ─────────────────────────────────────────────────────────────apt install rkhunter chkrootkitrkhunter --update                   # Update databaserkhunter --check                    # Scan rootkitchkrootkit                          # Cek rootkit
```

***

## 17. MONITORING & LOGGING

### 17.1 Monitoring Sistem

```
Bash# ─────────────────────────────────────────────────────────────# MONITORING REAL-TIME# ─────────────────────────────────────────────────────────────top                         # Monitoring proses interaktifhtop                        # Top yang lebih canggihatop                        # Advanced top dengan historyglances                     # Monitoring all-in-onedstat                       # Statistik sistem komprehensifiostat                      # I/O Statisticsiotop                       # Top untuk I/O (disk)iftop                       # Top untuk networknethogs                     # Penggunaan bandwidth per prosesbmon                        # Bandwidth monitornload                       # Network load monitorvnstat                      # Network traffic statisticssar                         # System Activity Reportvmstat                      # Virtual memory statisticsmpstat                      # CPU statistics per-core# ─────────────────────────────────────────────────────────────# INFORMASI HARDWARE REAL-TIME# ─────────────────────────────────────────────────────────────sensors                     # Suhu hardware (install: apt install lm-sensors)sensors-detect              # Deteksi sensorwatch sensors               # Monitor suhu secara real-timenvidia-smi                  # Info GPU NVIDIAgpu-burn                    # Test GPUsmartctl -a /dev/sda        # SMART data disk (install: smartmontools)smartctl -t short /dev/sda  # Test SMART singkat# ─────────────────────────────────────────────────────────────# PERINTAH MONITORING JARINGAN# ─────────────────────────────────────────────────────────────ss -s                       # Statistik socketnetstat -s                  # Statistik jaringanip -s link                  # Statistik interfacecat /proc/net/dev           # Statistik raw networktcpdump -i eth0             # Capture paket jaringantcpdump -i eth0 port 80     # Capture port 80tcpdump -i eth0 -w file.pcap  # Simpan ke filewireshark                   # GUI packet analyzer
```

### 17.2 Manajemen Log

```
Bash# ─────────────────────────────────────────────────────────────# FILE LOG PENTING# ─────────────────────────────────────────────────────────────# Lokasi log umum:/var/log/syslog             # Log sistem umum (Debian/Ubuntu)/var/log/messages           # Log sistem umum (RHEL/CentOS)/var/log/auth.log           # Log autentikasi (Debian/Ubuntu)/var/log/secure             # Log autentikasi (RHEL/CentOS)/var/log/kern.log           # Log kernel/var/log/dmesg              # Pesan boot kernel/var/log/dpkg.log           # Log instalasi paket/var/log/apt/               # Log apt/var/log/nginx/             # Log nginx/var/log/apache2/           # Log Apache/var/log/mysql/             # Log MySQL/var/log/postgresql/        # Log PostgreSQL/var/log/cron               # Log cron jobs/var/log/lastlog            # Log login terakhir/var/log/wtmp               # Log semua login/logout/var/log/btmp               # Log percobaan login gagal# Membaca logtail -f /var/log/syslog             # Monitor log real-timetail -n 100 /var/log/auth.log       # 100 baris terakhirgrep "error" /var/log/syslog        # Cari errorgrep "Failed" /var/log/auth.log     # Login gagaldmesg                               # Kernel messagesdmesg | tail -20                    # 20 pesan kernel terakhirdmesg --level=err                   # Hanya error kerneldmesg -T                            # Dengan timestamp yang readable# Perintah khusus untuk log loginlast                        # Riwayat loginlastb                       # Login gagalwho                         # Siapa yang login sekarangw                           # Detail pengguna yang loginlastlog                     # Login terakhir semua user# ─────────────────────────────────────────────────────────────# RSYSLOG - Sistem Logging# ─────────────────────────────────────────────────────────────# Konfigurasi: /etc/rsyslog.conf dan /etc/rsyslog.d/# Format: facility.level    destination# Facility: auth, cron, daemon, kern, mail, user, local0-7# Level: emerg, alert, crit, err, warning, notice, info, debug# Contoh konfigurasi rsyslog:# auth,authpriv.*     /var/log/auth.log# kern.*              /var/log/kern.log# *.error             /var/log/error.log# local0.*            /var/log/myapp.log# Kirim log ke remote server# *.* @@remote-syslog-server:514    # TCP# *.* @remote-syslog-server:514     # UDPsystemctl restart rsyslog# ─────────────────────────────────────────────────────────────# LOGROTATE - Rotasi Log Otomatis# ─────────────────────────────────────────────────────────────# Konfigurasi: /etc/logrotate.conf dan /etc/logrotate.d/# Contoh konfigurasi logrotate:cat > /etc/logrotate.d/myapp << 'EOF'/var/log/myapp/*.log {    daily               # Rotasi harian    rotate 30           # Simpan 30 file    compress            # Kompres file lama    delaycompress       # Tunda kompresi 1 siklus    notifempty          # Tidak rotasi jika kosong    missingok           # Tidak error jika file tidak ada    create 0640 myapp myapp  # Permission file baru    postrotate          # Jalankan setelah rotasi        systemctl reload myapp    endscript}EOFlogrotate -d /etc/logrotate.d/myapp    # Dry run (test)logrotate -f /etc/logrotate.d/myapp    # Force rotasilogrotate /etc/logrotate.conf          # Rotasi manual
```

### 17.3 Monitoring Tools Lanjutan

```
Bash# ─────────────────────────────────────────────────────────────# PROMETHEUS & GRAFANA (Monitoring Enterprise)# ─────────────────────────────────────────────────────────────# Install Node Exporter (pengumpul metrik)wget https://github.com/prometheus/node_exporter/releases/...tar xvf node_exporter-*.tar.gz./node_exporter &# Metrik tersedia di: http://localhost:9100/metrics# ─────────────────────────────────────────────────────────────# ELK STACK (Elasticsearch, Logstash, Kibana)# ─────────────────────────────────────────────────────────────# Untuk log management skala besar# Elasticsearch: penyimpanan & pencarian# Logstash: pengumpulan & transformasi log# Kibana: visualisasi# Filebeat: lightweight log shipper# ─────────────────────────────────────────────────────────────# NAGIOS / ZABBIX / ICINGA# ─────────────────────────────────────────────────────────────# Untuk monitoring infrastruktur enterprise
```

***

## 18. VIRTUALISASI & CONTAINER

### 18.1 KVM (Kernel-based Virtual Machine)

```
Bash# ─────────────────────────────────────────────────────────────# KVM - Virtualisasi Hardware# ─────────────────────────────────────────────────────────────# Cek dukungan virtualisasiegrep -c '(vmx|svm)' /proc/cpuinfo     # >0 = didukungkvm-ok                                  # Cek KVM support# Install KVMapt install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils virt-manager# Tambah user ke grup libvirtusermod -aG libvirt $USERusermod -aG kvm $USER# Manajemen VM dengan virshvirsh list                      # List VM yang berjalanvirsh list --all                # Semua VMvirsh start vm-name             # Mulai VMvirsh shutdown vm-name          # Shutdown VM gracefulvirsh destroy vm-name           # Force stop VMvirsh reboot vm-name            # Reboot VMvirsh suspend vm-name           # Suspend VMvirsh resume vm-name            # Resume VMvirsh snapshot-create-as vm snap1 "Deskripsi"  # Buat snapshotvirsh snapshot-list vm          # List snapshotvirsh snapshot-revert vm snap1  # Revert ke snapshotvirsh dominfo vm-name           # Info VMvirsh dumpxml vm-name           # Konfigurasi XML VM# Buat VM baruvirt-install \    --name ubuntu-vm \    --ram 2048 \    --vcpus 2 \    --disk path=/var/lib/libvirt/images/ubuntu.qcow2,size=20 \    --cdrom /path/to/ubuntu.iso \    --network bridge=virbr0 \    --graphics vnc \    --os-variant ubuntu22.04
```

### 18.2 Docker

```
Bash# ─────────────────────────────────────────────────────────────# DOCKER - Container Platform# ─────────────────────────────────────────────────────────────# Install Dockercurl -fsSL https://get.docker.com | bashusermod -aG docker $USER        # Tambah user ke grup docker# ─────────────────────────────────────────────────────────────# IMAGE MANAGEMENT# ─────────────────────────────────────────────────────────────docker images                   # List imagesdocker pull nginx               # Download image nginxdocker pull nginx:1.25          # Download versi spesifikdocker push myimage             # Upload image ke registrydocker build -t myapp:1.0 .    # Build image dari Dockerfiledocker rmi nginx                # Hapus imagedocker image prune              # Hapus image yang tidak terpakai# ─────────────────────────────────────────────────────────────# CONTAINER MANAGEMENT# ─────────────────────────────────────────────────────────────docker run nginx                            # Jalankan containerdocker run -d nginx                         # Background modedocker run -d -p 80:80 nginx               # Dengan port mappingdocker run -d -p 80:80 --name webserver nginx  # Dengan namadocker run -it ubuntu bash                  # Interactive modedocker run -d -v /host/path:/container/path nginx  # Volume mountdocker run -e ENV_VAR=nilai nginx          # Environment variabledocker run --rm nginx                       # Auto-remove saat stopdocker run --network mynet nginx           # Custom networkdocker ps                       # Container yang berjalandocker ps -a                    # Semua containerdocker stop webserver           # Stop containerdocker start webserver          # Start containerdocker restart webserver        # Restart containerdocker rm webserver             # Hapus container (harus stop dulu)docker rm -f webserver          # Force hapusdocker exec -it webserver bash  # Masuk ke containerdocker logs webserver           # Log containerdocker logs -f webserver        # Follow logdocker inspect webserver        # Detail containerdocker stats                    # Statistik resource# ─────────────────────────────────────────────────────────────# DOCKER NETWORK# ─────────────────────────────────────────────────────────────docker network ls                           # List networkdocker network create mynet                 # Buat networkdocker network create --driver bridge mynet # Bridge networkdocker network rm mynet                     # Hapus networkdocker network inspect mynet               # Detail networkdocker network connect mynet webserver     # Hubungkan container ke networkdocker network disconnect mynet webserver  # Putuskan koneksi# ─────────────────────────────────────────────────────────────# DOCKER VOLUME# ─────────────────────────────────────────────────────────────docker volume ls                # List volumedocker volume create myvolume   # Buat volumedocker volume rm myvolume       # Hapus volumedocker volume inspect myvolume  # Detail volumedocker volume prune             # Hapus volume yang tidak terpakai# ─────────────────────────────────────────────────────────────# DOCKERFILE# ─────────────────────────────────────────────────────────────# Contoh Dockerfile:cat > Dockerfile << 'EOF'# Base imageFROM ubuntu:22.04# MetadataLABEL maintainer="admin@example.com"LABEL version="1.0"# Environment variablesENV APP_HOME=/appENV NODE_ENV=production# Install dependenciesRUN apt-get update && apt-get install -y \    nodejs \    npm \    && rm -rf /var/lib/apt/lists/*# Set working directoryWORKDIR $APP_HOME# Copy fileCOPY package*.json ./RUN npm installCOPY . .# Expose portEXPOSE 3000# Health checkHEALTHCHECK --interval=30s --timeout=3s \    CMD curl -f http://localhost:3000/health || exit 1# Default commandCMD ["node", "server.js"]EOF# Build dan rundocker build -t myapp:1.0 .docker run -d -p 3000:3000 myapp:1.0# ─────────────────────────────────────────────────────────────# DOCKER COMPOSE# ─────────────────────────────────────────────────────────────# Contoh docker-compose.yml:cat > docker-compose.yml << 'EOF'version: '3.8'services:  web:    image: nginx:alpine    ports:      - "80:80"    volumes:      - ./html:/usr/share/nginx/html    depends_on:      - app    networks:      - frontend  app:    build: .    environment:      - DB_HOST=database      - DB_NAME=mydb    networks:      - frontend      - backend    restart: unless-stopped  database:    image: postgres:15    environment:      POSTGRES_DB: mydb      POSTGRES_USER: user      POSTGRES_PASSWORD: password    volumes:      - pgdata:/var/lib/postgresql/data    networks:      - backendnetworks:  frontend:  backend:volumes:  pgdata:EOF# Perintah Docker Composedocker compose up -d            # Jalankan semua servicedocker compose down             # Hentikan dan hapusdocker compose ps               # Status servicedocker compose logs             # Log semua servicedocker compose logs -f web      # Follow log service webdocker compose restart web      # Restart service webdocker compose build            # Build ulang imagedocker compose exec web bash    # Masuk ke containerdocker compose pull             # Update semua image
```

### 18.3 Kubernetes (Pengenalan)

```
Bash# ─────────────────────────────────────────────────────────────# KUBERNETES - Container Orchestration# ─────────────────────────────────────────────────────────────# Install kubectlcurl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"chmod +x kubectlmv kubectl /usr/local/bin/# Install minikube (untuk development)curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64install minikube-linux-amd64 /usr/local/bin/minikubeminikube start# Perintah kubectl dasarkubectl get nodes               # List nodeskubectl get pods                # List podskubectl get pods -A             # Semua namespacekubectl get services            # List serviceskubectl get deployments         # List deploymentskubectl get namespaces          # List namespacekubectl apply -f deployment.yaml    # Deploy dari filekubectl delete -f deployment.yaml   # Hapus dari filekubectl describe pod nama-pod       # Detail podkubectl logs nama-pod               # Log podkubectl exec -it nama-pod -- bash   # Masuk ke podkubectl scale deployment myapp --replicas=3  # Scalekubectl rollout status deployment myapp      # Status rolloutkubectl rollout undo deployment myapp        # Rollback# Contoh deployment.yamlcat > deployment.yaml << 'EOF'apiVersion: apps/v1kind: Deploymentmetadata:  name: myapp  labels:    app: myappspec:  replicas: 3  selector:    matchLabels:      app: myapp  template:    metadata:      labels:        app: myapp    spec:      containers:      - name: myapp        image: myapp:1.0        ports:        - containerPort: 3000        resources:          requests:            memory: "64Mi"            cpu: "250m"          limits:            memory: "128Mi"            cpu: "500m"---apiVersion: v1kind: Servicemetadata:  name: myapp-servicespec:  selector:    app: myapp  ports:  - port: 80    targetPort: 3000  type: LoadBalancerEOF
```

***

## 19. LINUX UNTUK SERVER

### 19.1 Web Server

```
Bash# ─────────────────────────────────────────────────────────────# NGINX# ─────────────────────────────────────────────────────────────# Installapt install nginx# Struktur konfigurasi/etc/nginx/nginx.conf           # Konfigurasi utama/etc/nginx/sites-available/     # Konfigurasi virtual host/etc/nginx/sites-enabled/       # Virtual host yang aktif/var/www/html/                  # Direktori web default/var/log/nginx/                 # Log# Buat virtual hostcat > /etc/nginx/sites-available/example.com << 'EOF'server {    listen 80;    listen [::]:80;    server_name example.com www.example.com;    root /var/www/example.com;    index index.html index.php;    # Redirect ke HTTPS    return 301 https://$host$request_uri;}server {    listen 443 ssl http2;    listen [::]:443 ssl http2;    server_name example.com www.example.com;    ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;    ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;    root /var/www/example.com;    index index.html index.php;    location / {        try_files $uri $uri/ =404;    }    location ~ \.php$ {        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;        fastcgi_index index.php;        include fastcgi_params;        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;    }    location /api/ {        proxy_pass http://localhost:3000;        proxy_http_version 1.1;        proxy_set_header Upgrade $http_upgrade;        proxy_set_header Connection 'upgrade';        proxy_set_header Host $host;        proxy_cache_bypass $http_upgrade;    }    access_log /var/log/nginx/example.com-access.log;    error_log /var/log/nginx/example.com-error.log;    # Gzip    gzip on;    gzip_types text/plain text/css application/json application/javascript;}EOF# Aktifkan virtual hostln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/nginx -t                        # Test konfigurasisystemctl reload nginx# SSL dengan Let's Encryptapt install certbot python3-certbot-nginxcertbot --nginx -d example.com -d www.example.comcertbot renew --dry-run         # Test renewal# Auto-renewal sudah dikonfigurasi di /etc/cron.d/certbot# ─────────────────────────────────────────────────────────────# APACHE# ─────────────────────────────────────────────────────────────apt install apache2/etc/apache2/apache2.conf       # Konfigurasi utama/etc/apache2/sites-available/   # Virtual host/etc/apache2/mods-available/    # Module tersedia/var/www/html/                  # Direktori weba2ensite example.com            # Enable virtual hosta2dissite example.com           # Disable virtual hosta2enmod rewrite                 # Enable modulea2dismod rewrite                # Disable moduleapache2ctl -t                   # Test konfigurasiapache2ctl configtest           # Test konfigurasisystemctl reload apache2
```

### 19.2 Database Server

```
Bash# ─────────────────────────────────────────────────────────────# MYSQL / MARIADB# ─────────────────────────────────────────────────────────────apt install mysql-server# atauapt install mariadb-servermysql_secure_installation       # Hardening MySQL# Loginmysql -u root -p                # Login sebagai rootmysql -u user -p database       # Login dengan user dan database# Perintah MySQL dasar:SHOW DATABASES;CREATE DATABASE mydb;USE mydb;SHOW TABLES;CREATE USER 'user'@'localhost' IDENTIFIED BY 'password';GRANT ALL PRIVILEGES ON mydb.* TO 'user'@'localhost';FLUSH PRIVILEGES;EXIT;# Backup & Restoremysqldump -u root -p mydb > backup.sql      # Backupmysql -u root -p mydb < backup.sql          # Restoremysqldump -u root -p --all-databases > all.sql  # Backup semua# Konfigurasi: /etc/mysql/mysql.conf.d/mysqld.cnf# ─────────────────────────────────────────────────────────────# POSTGRESQL# ─────────────────────────────────────────────────────────────apt install postgresql# Loginsudo -u postgres psql           # Login sebagai postgres# Perintah PostgreSQL:\l                              # List database\c mydb                         # Connect ke database\dt                             # List tablesCREATE DATABASE mydb;CREATE USER myuser WITH ENCRYPTED PASSWORD 'password';GRANT ALL PRIVILEGES ON DATABASE mydb TO myuser;\q                              # Quit# Backup & Restorepg_dump mydb > backup.sql                   # Backup databasepg_dump -Fc mydb > backup.dump              # Format custompsql mydb < backup.sql                      # Restorepg_restore -d mydb backup.dump              # Restore custom formatpg_dumpall > all_databases.sql             # Backup semua# Konfigurasi:/etc/postgresql/*/main/postgresql.conf      # Konfigurasi utama/etc/postgresql/*/main/pg_hba.conf         # Autentikasi
```

### 19.3 Email Server

```
Bash# ─────────────────────────────────────────────────────────────# POSTFIX - Mail Transfer Agent# ─────────────────────────────────────────────────────────────apt install postfix# Konfigurasi: /etc/postfix/main.cf# Konfigurasi penting:myhostname = mail.example.commydomain = example.commyorigin = $mydomaininet_interfaces = allmydestination = $myhostname, localhost.$mydomain, $mydomainrelayhost =mynetworks = 127.0.0.0/8 [::ffff:127.0.0.0]/104 [::1]/128postfix check               # Cek konfigurasipostfix reload              # Reload konfigurasipostfix status              # Status postfixpostqueue -p                # Lihat queue mailpostqueue -f                # Flush queue (kirim ulang)postsuper -d ALL            # Hapus semua mail di queuemailq                       # Lihat queue mail
```

### 19.4 DNS Server

```
Bash# ─────────────────────────────────────────────────────────────# BIND9 - DNS Server# ─────────────────────────────────────────────────────────────apt install bind9 bind9utils# Konfigurasi: /etc/bind/# named.conf.options    - Opsi umum# named.conf.local      - Zone definitions# named.conf.default-zones - Zone default# Contoh zone file (/etc/bind/zones/example.com.db):# $TTL 86400# @ IN SOA ns1.example.com. admin.example.com. (#     2024010101  ; Serial#     3600        ; Refresh#     1800        ; Retry#     604800      ; Expire#     86400       ; Minimum TTL# )# @ IN NS ns1.example.com.# @ IN NS ns2.example.com.# @ IN MX 10 mail.example.com.# @ IN A 192.168.1.100# www IN A 192.168.1.100# mail IN A 192.168.1.101# ns1 IN A 192.168.1.102# ns2 IN A 192.168.1.103named-checkconf             # Cek konfigurasinamed-checkzone example.com /etc/bind/zones/example.com.db  # Cek zonesystemctl restart bind9
```

***

## 20. TROUBLESHOOTING

### 20.1 Metodologi Troubleshooting

```
textLANGKAH TROUBLESHOOTING SISTEMATIS:1. IDENTIFIKASI MASALAH   ├── Apa yang tidak berfungsi?   ├── Kapan mulai terjadi?   ├── Apakah ada perubahan sebelumnya?   └── Apakah ada pesan error?2. KUMPULKAN INFORMASI   ├── Cek log: /var/log/syslog, journalctl -xe   ├── Cek status layanan: systemctl status   ├── Cek resource: top, free, df -h   └── Cek jaringan: ping, netstat, ss3. ANALISIS   ├── Identifikasi root cause   ├── Buat hipotesis   └── Prioritaskan berdasarkan kemungkinan4. SOLUSI   ├── Lakukan perubahan satu per satu   ├── Catat setiap perubahan   └── Test setelah setiap perubahan5. VERIFIKASI   ├── Konfirmasi masalah telah teratasi   ├── Test skenario lain   └── Monitor setelah perbaikan6. DOKUMENTASI   └── Catat masalah, penyebab, dan solusi
```

### 20.2 Perintah Troubleshooting

```
Bash# ─────────────────────────────────────────────────────────────# TROUBLESHOOTING BOOT# ─────────────────────────────────────────────────────────────# Mode Recovery/Single User# - Saat boot, tekan E di GRUB# - Tambahkan "single" atau "rescue" di akhir baris linux# - Ctrl+X untuk boot# Cek status bootsystemd-analyze                 # Waktu total bootsystemd-analyze blame           # Kontribusi setiap servicesystemd-analyze critical-chain  # Rantai kritis bootsystemd-analyze plot > boot.svg # Grafik boot# Log bootdmesg | grep -i error          # Error bootdmesg | grep -i fail           # Kegagalan bootjournalctl -b                   # Log boot saat inijournalctl -b -1                # Log boot sebelumnya# ─────────────────────────────────────────────────────────────# TROUBLESHOOTING DISK# ─────────────────────────────────────────────────────────────# Disk penuhdf -h                           # Cek penggunaan diskdu -sh /* 2>/dev/null | sort -hr | head -20  # Direktori terbesardu -sh /var/log/*               # Ukuran logfind / -size +100M -type f 2>/dev/null   # File besarfind / -name "*.log" -size +50M          # Log besar# Inodes habisdf -i                           # Cek inode usagefind / -xdev -printf '%h\n' | sort | uniq -c | sort -k 1 -n | tail -20# Perbaikan filesystemumount /dev/sdb1                # Unmount dulufsck -y /dev/sdb1              # Perbaiki otomatis# ─────────────────────────────────────────────────────────────# TROUBLESHOOTING MEMORY# ─────────────────────────────────────────────────────────────free -h                         # Cek memorycat /proc/meminfo               # Detail memoryvmstat -s                       # Statistik memorydmesg | grep -i "out of memory" # OOM eventsps aux --sort=-%mem | head -20  # Proses paling banyak pakai memory# OOM Killerdmesg | grep OOM                # Log OOM Killercat /var/log/syslog | grep -i oom# ─────────────────────────────────────────────────────────────# TROUBLESHOOTING JARINGAN# ─────────────────────────────────────────────────────────────# Tidak ada konektivitasping 127.0.0.1                  # Cek loopbackping 192.168.1.1                # Cek gatewayping 8.8.8.8                    # Cek internet (IP)ping google.com                 # Cek DNS# Diagnosa step by step:ip addr show                    # Cek interface & IPip route show                   # Cek routing tablecat /etc/resolv.conf            # Cek DNS configsystemctl status NetworkManager # Cek NetworkManagersystemctl status networking     # Cek networking service# Port tidak bisa diaksesss -tuln                        # Cek port yang listeniptables -L                     # Cek firewall rulesufw status                      # Cek UFWnetstat -tulpn                  # Port + proses# ─────────────────────────────────────────────────────────────# TROUBLESHOOTING PROSES# ─────────────────────────────────────────────────────────────# CPU tinggitop                             # Identifikasi prosesps aux --sort=-%cpu | head -20  # Proses CPU tertinggistrace -p PID                   # Trace system calls# Proses zombieps aux | awk '$8 == "Z"'        # Cari zombie processkill -SIGCHLD PPID              # Kirim signal ke parent# Proses tidak bisa di-killkill -9 PID                     # Force killkill -SIGTERM PID               # Graceful kill# ─────────────────────────────────────────────────────────────# TROUBLESHOOTING LAYANAN# ─────────────────────────────────────────────────────────────systemctl status nginx          # Status layananjournalctl -xe                  # Log error terbarujournalctl -u nginx -n 50      # Log nginx 50 barisjournalctl -u nginx --since "10 min ago"  # Log 10 menit terakhirsystemctl --failed              # Layanan yang gagal# Debug layanannginx -t                        # Test konfigurasi nginxapache2ctl -t                   # Test konfigurasi apachemysql --check-all-tables        # Check MySQL tables# ─────────────────────────────────────────────────────────────# TROUBLESHOOTING SSH# ─────────────────────────────────────────────────────────────ssh -v user@host                # Verbose (debug)ssh -vvv user@host              # Very verbosetail -f /var/log/auth.log       # Monitor SSH loginsshd -t                         # Test konfigurasi SSH# Masalah keyls -la ~/.ssh/                  # Cek permissionchmod 700 ~/.ssh/chmod 600 ~/.ssh/authorized_keyschmod 600 ~/.ssh/id_rsachmod 644 ~/.ssh/id_rsa.pub# ─────────────────────────────────────────────────────────────# RECOVERY & RESCUE# ─────────────────────────────────────────────────────────────# Boot ke rescue mode# - Tekan 'e' di GRUB# - Tambah 'rd.break' atau 'init=/bin/bash' ke baris linux# - Ctrl+X untuk boot# Mount root dalam rescue modemount -o remount,rw /# Reset password root (dari rescue mode)passwd root# Perbaiki GRUBgrub-install /dev/sda           # Install GRUBupdate-grub                     # Update konfigurasi GRUB
```

### 20.3 Cheat Sheet Troubleshooting Cepat

```
textMASALAH                    | PERINTAH─────────────────────────────────────────────────────────────────Disk penuh                 | df -h && du -sh /* | sort -hMemory habis               | free -h && ps aux --sort=-%memCPU 100%                   | top (tekan P untuk sort CPU)Layanan tidak jalan        | systemctl status LAYANANTidak bisa SSH             | tail -f /var/log/auth.logPort tidak terbuka         | ss -tuln | grep PORTDNS tidak jalan            | dig google.com && cat /etc/resolv.confTidak bisa internet        | ping 8.8.8.8 && ip routeFile system read-only      | mount -o remount,rw /Disk error                 | dmesg | grep -i errorBoot gagal                 | journalctl -b -p errProses zombie              | ps aux | grep ZPermission denied          | ls -la FILE && idLog penuh                  | du -sh /var/log/* | sort -hPackage rusak              | apt --fix-broken installGRUB rusak                 | Boot live USB, chroot, grub-install
```

***

## APPENDIX

### A. Shortcut Keyboard Terminal

```
textSHORTCUT TERMINAL BASH:─────────────────────────────────────────────────────────────────Ctrl + A        → Pindah ke awal barisCtrl + E        → Pindah ke akhir barisCtrl + U        → Hapus dari kursor ke awal barisCtrl + K        → Hapus dari kursor ke akhir barisCtrl + W        → Hapus kata sebelum kursorCtrl + Y        → Paste (yank) teks yang dihapusCtrl + R        → Cari perintah di historyCtrl + L        → Clear screenCtrl + C        → Interupsi prosesCtrl + Z        → Suspend proses ke backgroundCtrl + D        → Logout/EOFCtrl + S        → Stop output terminalCtrl + Q        → Lanjutkan output terminalTab             → Auto-completeTab Tab         → Tampilkan semua kemungkinan!!              → Perintah terakhir!$              → Argumen terakhir dari perintah terakhirAlt + .         → Insert argumen terakhir dari perintah sebelumnya
```

### B. Regular Expression (Regex)

```
textREGEX DASAR:─────────────────────────────────────────────────────────────────.               → Satu karakter apapun*               → Nol atau lebih dari karakter sebelumnya+               → Satu atau lebih?               → Nol atau satu^               → Awal baris$               → Akhir baris[abc]           → Salah satu dari a, b, c[^abc]          → Bukan a, b, atau c[a-z]           → Huruf kecil a sampai z[0-9]           → Digit 0 sampai 9\d              → Digit (sama dengan [0-9])\w              → Word character (huruf, angka, underscore)\s              → Whitespace{n}             → Tepat n kali{n,m}           → n sampai m kali(abc)           → Groupa|b             → a atau b\               → Escape karakter khususCONTOH:─────────────────────────────────────────────────────────────────^[0-9]+$        → String yang hanya berisi angka\b[A-Z]\w*      → Kata yang dimulai huruf kapital\d{3}-\d{4}     → Format nomor telepon[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}  → Email
```

### C. Sinyal Penting Linux

```
textSINYAL LINUX:─────────────────────────────────────────────────────────────────SIGHUP  (1)   → Hangup - reload konfigurasiSIGINT  (2)   → Interrupt (Ctrl+C)SIGQUIT (3)   → Quit dengan core dumpSIGKILL (9)   → Kill paksa (tidak bisa diabaikan)SIGTERM (15)  → Terminate dengan graceSIGSTOP (19)  → Stop prosesSIGCONT (18)  → Lanjutkan proses yang di-stopSIGUSR1 (10)  → User-defined signal 1SIGUSR2 (12)  → User-defined signal 2
```

### D. Kode Exit

```
textEXIT CODE:─────────────────────────────────────────────────────────────────0           → Sukses1           → Error umum2           → Misuse shell command126         → Permission denied (tidak bisa eksekusi)127         → Command not found128+n       → Fatal error dengan signal n130         → Script diakhiri Ctrl+C (128+2)
```

### E. File Konfigurasi Penting&#x20;
