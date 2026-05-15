---
icon: book-open-lines
---

# Linux Comprehensive Dok

## Dokumentasi Lengkap Linux

Panduan ringkas dan rapi tentang Linux: sejarah, arsitektur, distro, instalasi, perintah dasar, admin sistem, sampai troubleshooting.

## Daftar Isi

1. [Pendahuluan & Sejarah Linux](#1-pendahuluan--sejarah-linux)
2. [Arsitektur Linux](#2-arsitektur-linux)
3. [Distribusi Linux](#3-distribusi-linux)
4. [Instalasi Linux](#4-instalasi-linux)
5. [Struktur Direktori Linux](#5-struktur-direktori-linux)
6. [Perintah Dasar Linux](#6-perintah-dasar-linux)
7. [Manajemen File & Direktori](#7-manajemen-file--direktori)
8. [Manajemen Pengguna & Grup](#8-manajemen-pengguna--grup)
9. [Manajemen Hak Akses](#9-manajemen-hak-akses)
10. [Manajemen Proses](#10-manajemen-proses)
11. [Manajemen Paket](#11-manajemen-paket)
12. [Manajemen Disk & Storage](#12-manajemen-disk--storage)
13. [Jaringan di Linux](#13-jaringan-di-linux)
14. [Shell & Scripting](#14-shell--scripting)
15. [Manajemen Layanan (systemd)](#15-manajemen-layanan-systemd)
16. [Keamanan Linux](#16-keamanan-linux)
17. [Monitoring & Logging](#17-monitoring--logging)
18. [Virtualisasi & Container](#18-virtualisasi--container)
19. [Linux untuk Server](#19-linux-untuk-server)
20. [Troubleshooting](#20-troubleshooting)

## 1. Pendahuluan & Sejarah Linux

### 1.1 Apa Itu Linux?

Linux adalah sistem operasi open source berbasis Unix. Linux adalah kernel, bukan keseluruhan OS.

Karakteristik:
- Open source
- Multi-user
- Multi-tasking
- Portable
- Stabil dan aman
- Gratis

### 1.2 Sejarah Singkat

| Tahun | Peristiwa |
| --- | --- |
| 1969 | UNIX lahir di Bell Labs |
| 1983 | GNU Project dimulai |
| 1991 | Linus Torvalds merilis kernel Linux |
| 1992 | Linux memakai lisensi GPL |
| 1993 | Slackware dan Debian muncul |
| 2004 | Ubuntu dirilis |
| 2007 | Android diumumkan |
| 2024+ | Linux 6.x terus berkembang |

### 1.3 Filosofi Unix

1. Do one thing and do it well
2. Everything is a file
3. Small, simple programs
4. Modularity
5. Transparency

## 2. Arsitektur Linux

### 2.1 Gambaran Umum

Linux terdiri dari:
- User space
- System call interface
- Kernel space
- Device drivers
- Hardware

### 2.2 Komponen Kernel

- Process management
- Memory management
- File system
- Network stack
- Device drivers

### 2.3 Jenis Kernel

| Jenis | Ciri |
| --- | --- |
| Monolithic | Layanan inti jalan di kernel space |
| Microkernel | Kernel kecil, layanan lain di user space |
| Hybrid | Campuran monolithic dan microkernel |

## 3. Distribusi Linux

### 3.1 Apa Itu Distro?

Distro adalah paket lengkap berisi:
- Kernel Linux
- GNU tools dan library
- Package manager
- Desktop environment
- Aplikasi tambahan

### 3.2 Distro Populer

| Distro | Basis | Target |
| --- | --- | --- |
| Ubuntu | Debian | Pemula sampai server |
| Debian | Independen | Server dan stabilitas |
| RHEL | RPM | Enterprise |
| Fedora | RHEL | Developer |
| Arch Linux | Independen | Power user |
| Manjaro | Arch | Intermediate |
| Kali Linux | Debian | Security |
| Alpine Linux | Independen | Container dan embedded |

## 4. Instalasi Linux

### 4.1 Persiapan

Contoh minimum Ubuntu:

| Komponen | Minimum | Rekomendasi |
| --- | --- | --- |
| CPU | 1 GHz dual-core | 2+ GHz quad-core |
| RAM | 4 GB | 8 GB+ |
| Storage | 25 GB | 50 GB+ |
| Display | 1024x768 | 1920x1080 |

### 4.2 Bootable Media

Rufus di Windows:
1. Download ISO
2. Colok USB minimal 8 GB
3. Buka Rufus
4. Pilih ISO
5. Klik Start

`dd` di Linux:

```bash
lsblk
sudo dd if=linux.iso of=/dev/sdX bs=4M status=progress sync
```

### 4.3 Langkah Instalasi

1. Boot dari USB
2. Pilih bahasa
3. Pilih opsi instalasi
4. Atur partisi
5. Pilih zona waktu
6. Buat akun
7. Tunggu instalasi selesai
8. Reboot

### 4.4 Skema Partisi

Desktop:
- `/boot/efi` 512 MB FAT32
- `/boot` 1 GB ext4
- `/` 30 GB ext4
- `/home` sisa ruang
- `swap` 2x RAM atau 4-8 GB

Server:
- `/boot/efi` 512 MB FAT32
- `/boot` 1 GB ext4
- `/` 20 GB ext4
- `/var` 50 GB ext4
- `/tmp` 10 GB ext4
- `/home` 20 GB ext4
- `/opt` 50 GB ext4
- `/data` sisa ruang

## 5. Struktur Direktori Linux

| Direktori | Fungsi |
| --- | --- |
| `/` | Root filesystem |
| `/bin` | Binary penting |
| `/boot` | Kernel dan bootloader |
| `/dev` | Device file |
| `/etc` | Konfigurasi |
| `/home` | Data user |
| `/lib` | Library penting |
| `/opt` | Aplikasi opsional |
| `/proc` | Virtual filesystem proses |
| `/root` | Home root |
| `/sbin` | Binary admin |
| `/tmp` | File sementara |
| `/usr` | Program dan data user |
| `/var` | Data berubah-ubah |

## 6. Perintah Dasar Linux

### 6.1 Navigasi

```bash
pwd
ls
cd /path
tree
```

### 6.2 Info Sistem

```bash
uname -a
hostname
whoami
id
uptime
free -h
lsblk
df -h
```

### 6.3 File Dasar

```bash
touch file.txt
mkdir -p dir/subdir
cp file.txt backup.txt
mv old.txt new.txt
rm file.txt
cat file.txt
less file.txt
head file.txt
tail file.txt
find /home -name "*.log"
grep -R "kata" .
```

## 7. Manajemen File & Direktori

- `cp` untuk salin
- `mv` untuk pindah atau rename
- `rm` untuk hapus
- `rmdir` untuk folder kosong
- `find` untuk cari file
- `du` untuk ukuran folder

Contoh:

```bash
cp -r source/ target/
mv file.txt /tmp/
rm -rf old-dir/
du -sh *
```

## 8. Manajemen Pengguna & Grup

```bash
useradd namauser
passwd namauser
usermod -aG sudo namauser
groupadd namagrup
groups namauser
id namauser
```

## 9. Manajemen Hak Akses

| Mode | Arti |
| --- | --- |
| `r` | read |
| `w` | write |
| `x` | execute |

Contoh:

```bash
chmod 755 script.sh
chmod u+x script.sh
chown user:group file.txt
umask
```

## 10. Manajemen Proses

```bash
ps aux
top
htop
kill 1234
kill -9 1234
pkill nama-proses
jobs
fg
bg
nohup command &
```

## 11. Manajemen Paket

| Ekosistem | Manager |
| --- | --- |
| Debian/Ubuntu | `apt`, `dpkg` |
| RHEL/Fedora | `dnf`, `rpm` |
| Arch | `pacman` |
| Universal | `snap`, `flatpak` |

Contoh:

```bash
sudo apt update
sudo apt install nginx
sudo dnf install nginx
sudo pacman -S nginx
```

## 12. Manajemen Disk & Storage

```bash
df -h
du -sh *
lsblk
fdisk -l
blkid
mount /dev/sdb1 /mnt
umount /mnt
```

## 13. Jaringan di Linux

```bash
ip a
ip r
ping 8.8.8.8
ss -tulpn
curl https://example.com
wget https://example.com/file
```

## 14. Shell & Scripting

Contoh skrip:

```bash
#!/usr/bin/env bash
set -euo pipefail

echo "Hello Linux"
```

Dasar:
- Variabel
- Kondisi
- Perulangan
- Exit code
- Redirect output

## 15. Manajemen Layanan (systemd)

```bash
systemctl status nginx
systemctl start nginx
systemctl stop nginx
systemctl restart nginx
systemctl enable nginx
systemctl disable nginx
journalctl -u nginx
```

## 16. Keamanan Linux

- Gunakan user biasa, bukan root
- Update rutin
- Pakai SSH key
- Tutup port yang tidak perlu
- Aktifkan firewall

Contoh:

```bash
sudo ufw enable
sudo ufw allow ssh
sudo ufw status
```

## 17. Monitoring & Logging

```bash
top
htop
free -h
vmstat 1
iostat
journalctl -xe
tail -f /var/log/syslog
```

## 18. Virtualisasi & Container

| Teknologi | Fungsi |
| --- | --- |
| KVM | Virtual machine |
| Docker | Container |
| Podman | Container tanpa daemon |
| LXC | Container sistem |

## 19. Linux untuk Server

Fokus utama server Linux:
- Stabil
- Aman
- Ringan
- Mudah diotomasi
- Cocok untuk web, database, API, dan service background

## 20. Troubleshooting

Checklist cepat:
1. Cek log
2. Cek resource
3. Cek service
4. Cek jaringan
5. Cek permission
6. Cek storage

Perintah:

```bash
journalctl -xe
systemctl status service
df -h
free -h
ss -tulpn
ps aux
```
