---
icon: linux
---

# Arh Linux

## Install Acrh Linux on Virtual Box

## Panduan Lengkap Install Arch Linux di VirtualBox (UEFI) — langkah demi langkah + penjelasan setiap langkah

Berikut panduan terperinci untuk _install Arch Linux_ di **VirtualBox (UEFI aktif)** dengan layout partisi yang kamu gunakan:

* `/dev/sda1` → **swap**
* `/dev/sda2` → **EFI System Partition (ESP)** (FAT32)
* `/dev/sda3` → **root (/)**

Setiap langkah ada perintah + penjelasan kenapa perintah itu diperlukan. Jalankan semua perintah dari ISO Arch (live environment) kecuali bagian pasca-reboot.

***

## Persiapan VirtualBox

1. Buat VM baru, pilih Linux → Other Linux (64-bit) atau Arch jika tersedia.
2. **System → Motherboard**: centang **Enable EFI (special OSes only)** (penting).
3. **System → Processor**: set minimal 2 CPU (opsional tapi nyaman).
4. **Display → Video Memory**: 128 MB (opsional).
5. Pasang ISO Arch di Optical Drive (Storage).
6. Buat HDD (VDI) ukuran mis. 20–40 GB, atau sesuai kebutuhan.

**Kenapa:** UEFI diperlukan karena kamu pilih ESP. Setting CPU/VRAM untuk performa VM lebih baik.

***

## 1 — Boot ke ISO & cek koneksi

Boot VM dari ISO, kamu akan masuk ke shell live Arch.

Cek koneksi (penting untuk `pacstrap`):

```bash
ping -c 3 archlinux.org
```

Jika pakai Wi-Fi:

```bash
iwctl
# di iwctl prompt:
device list
station wlan0 connect NAMAWIFI
# keluar dari iwctl dengan `exit`
```

**Penjelasan:** `pacstrap` membutuhkan internet untuk mengunduh paket dasar.

***

## 2 — Sinkronisasi waktu

Atur NTP supaya waktu sistem akurat (penting untuk paket dan sertifikat):

```bash
timedatectl set-ntp true
```

***

## 3 — Partisi disk (jika belum dibuat)

Jika sudah membuat partisi sesuai sda1/sda2/sda3, lewati. Bila belum, contoh singkat menggunakan `cfdisk`:

```bash
cfdisk /dev/sda
```

Buat tiga partisi: 1 swap (\~2GB+), 2 EFI (512M, type FAT32 / EFI System), 3 root (sisanya). Tulis dan keluar.

**Penjelasan:** `cfdisk` interaktif, lebih mudah daripada `fdisk` untuk pemula.

***

## 4 — Format partisi

Jalankan sesuai layout:

#### Swap

```bash
mkswap /dev/sda1
swapon /dev/sda1
```

**Penjelasan:** `mkswap` membuat area swap; `swapon` mengaktifkannya sekarang (penting saat chroot untuk terhindar OOM).

#### EFI (FAT32)

```bash
mkfs.fat -F32 /dev/sda2
```

**Penjelasan:** UEFI mengharuskan ESP bertipe FAT32.

#### Root

```bash
mkfs.ext4 /dev/sda3
```

(Atau gunakan `btrfs`/`xfs` jika mau; ext4 aman dan sederhana.)

***

## 5 — Mount filesystem

```bash
mount /dev/sda3 /mnt
mkdir -p /mnt/boot
mount /dev/sda2 /mnt/boot
```

Swap sudah aktif dari langkah sebelumnya.

**Penjelasan:** Mount root di `/mnt` agar `pacstrap` menginstall ke lokasi yang tepat; ESP di `/mnt/boot` agar GRUB/EFI dapat menulis file boot.

***

## 6 — Install base system

Install paket minimal:

```bash
pacstrap /mnt base linux linux-firmware nano vim networkmanager
```

**Penjelasan:**

* `base` = paket dasar sistem.
* `linux` = kernel.
* `linux-firmware` = firmware hardware.
* `networkmanager` = memudahkan koneksi jaringan nanti.
* `nano`/`vim` = editor (pilih satu).

***

## 7 — Generate fstab

Buat entri fstab permanen:

```bash
genfstab -U /mnt >> /mnt/etc/fstab
cat /mnt/etc/fstab
```

**Pastikan ada baris swap** seperti:

```
/dev/sda1  none  swap  defaults  0 0
```

**Penjelasan:** `fstab` memastikan partisi dipasang otomatis saat boot.

***

## 8 — Masuk ke sistem (chroot)

```bash
arch-chroot /mnt
```

Sekarang semua perintah berikut dijalankan **di dalam** sistem terinstall.

**Penjelasan:** `arch-chroot` membuat root baru sehingga perintah konfigurasi menyasar sistem yang baru diinstall.

***

## 9 — Set timezone & hardware clock

Ganti sesuai zona kamu (misalnya `Asia/Makassar`):

```bash
ln -sf /usr/share/zoneinfo/Asia/Makassar /etc/localtime
hwclock --systohc
```

**Penjelasan:** Menyetel waktu lokal dan hardware clock.

***

## 10 — Konfigurasi locale (bahasa)

Edit `/etc/locale.gen` dan uncomment locale yang diinginkan, contoh:

```
en_US.UTF-8 UTF-8
id_ID.UTF-8 UTF-8
```

Kemudian jalankan:

```bash
locale-gen
echo "LANG=en_US.UTF-8" > /etc/locale.conf
```

Untuk keyboard (opsional):

```bash
echo "KEYMAP=us" > /etc/vconsole.conf
```

**Penjelasan:** `locale-gen` membuat file locale; `LANG` menentukan bahasa default sistem.

***

## 11 — Set hostname & hosts

Ganti `myarch` dengan nama host yang kamu mau:

```bash
echo "myarch" > /etc/hostname

cat >> /etc/hosts <<EOF
127.0.0.1   localhost
::1         localhost
127.0.1.1   myarch.localdomain myarch
EOF
```

**Penjelasan:** Hostname dan file hosts penting untuk resolusi lokal dan beberapa layanan.

***

## 12 — Set root password (penting)

**Wajib** set password root sekarang:

```bash
passwd
```

Masukkan password yang kuat dan konfirmasi.

**Penjelasan:** Jika root tidak punya password, login root akan ditolak/berisiko. Kita set sekarang untuk memastikan akses. Catat password ini.

***

## 13 — Buat user non-root

Rekomendasi: jangan pakai root untuk pekerjaan sehari-hari. Contoh membuat user `fred`:

```bash
useradd -m -G wheel -s /bin/bash fred
passwd fred
```

Penjelasan parameter:

* `-m`: buat home dir `/home/fred`
* `-G wheel`: tambahkan ke grup `wheel` (untuk sudo)
* `-s /bin/bash`: shell default

***

## 14 — Install & konfigurasi sudo

```bash
pacman -S sudo
EDITOR=nano visudo
```

Di `visudo`, uncomment baris:

```
%wheel ALL=(ALL:ALL) ALL
```

Simpan dan keluar.

**Penjelasan:** Memberi permission sudo ke anggota grup `wheel`. Sekarang user `fred` bisa `sudo` setelah login.

***

## 15 — Install GRUB untuk UEFI & buat entry

Install paket yang diperlukan:

```bash
pacman -S grub efibootmgr dosfstools
```

Install GRUB ke ESP yang sudah di-mount di `/boot`:

```bash
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=Arch
grub-mkconfig -o /boot/grub/grub.cfg
```

**Penjelasan:**

* `--efi-directory=/boot` menunjuk tempat ESP yang kita mount.
* `--bootloader-id=Arch` membuat folder `EFI/Arch` di ESP dan membuat entry NVRAM.
* `grub-mkconfig` membuat `grub.cfg`.

Jika VirtualBox tidak menambahkan entry NVRAM otomatis, bisa boot via EFI shell lalu menjalankan `grubx64.efi` dan memperbaiki NVRAM (lihat troubleshooting di akhir).

***

## 16 — Enable NetworkManager (agar jaringan aktif setelah boot)

```bash
systemctl enable NetworkManager
```

***

## 17 — Optional: Install tools tambahan

Mis. `openssh`, `ntp` (biasanya `systemd-timesyncd` sudah cukup), `sudo` sudah terpasang:

```bash
pacman -S openssh
systemctl enable sshd
```

***

## 18 — Keluar chroot dan reboot

```bash
exit
umount -R /mnt
reboot
```

Eject ISO dari virtual drive di VirtualBox sebelum booting ulang agar masuk ke sistem yang baru terpasang.

***

## Setelah reboot — login

Login sebagai `root` atau user `fred`:

* root → `root` + password yang sudah dibuat pada langkah 12
* user → `fred` + password yang dibuat langkah 13

Gunakan `sudo` untuk perintah admin:

```bash
sudo pacman -Syu
```

(pertama kali `sudo` akan meminta password user `fred`)

***

## Troubleshooting umum & solusi

#### 1) Tidak bisa login root / root ditolak meskipun password kosong

Jangan gunakan password kosong. Selalu set password root dengan `passwd` dari `arch-chroot` saat instalasi. Jika terlanjur dan tidak bisa login, boot kembali ISO, mount, chroot, lalu `passwd` lagi.

#### 2) Setelah install masuk ke EFI Shell (bukan GRUB)

Di EFI shell ketik:

```
fs0:
ls
cd EFI
cd Arch
grubx64.efi
```

Jika berhasil, perbaiki NVRAM entry:

```bash
efibootmgr -c -d /dev/sda -p 2 -L "Arch" -l '\EFI\Arch\grubx64.efi'
```

Penjelasan: `-p 2` menunjuk nomor partisi ESP (sesuaikan), `-l` jalur file boot relatif Windows-style.

#### 3) GRUB tidak muncul / VirtualBox tidak menyimpan NVRAM

Beberapa versi VirtualBox tidak menyimpan NVRAM entry. Kalau GRUB bisa dijalankan dari `grubx64.efi` tapi NVRAM hilang saat VM reboot, solusi praktis:

* Tetap beri perintah `grubx64.efi` di EFI shell otomatis, atau
* Gunakan pengaturan VirtualBox snapshot/state untuk menjaga entry, atau
* Pasang GRUB ke MBR (legacy) jika UEFI bermasalah — tapi kamu sudah memilih UEFI jadi idealnya perbaiki efibootmgr.

#### 4) Resolusi layar di VirtualBox / Guest Additions

Untuk fitur seperti clipboard, folder share, auto-resize, install _Guest Additions_ (untuk Arch di VM):

```bash
pacman -S virtualbox-guest-utils
# Untuk kernel modules:
modprobe -a vboxguest vboxsf vboxvideo
systemctl enable vboxservice
```

Jika menggunakan linux-headers diperlukan untuk kompile module, install `linux-headers` versi kernel yang sama.

***

## Rangkuman cepat perintah penting (untuk copy-paste)

```
# format & swap
mkswap /dev/sda1
swapon /dev/sda1
mkfs.fat -F32 /dev/sda2
mkfs.ext4 /dev/sda3

# mount
mount /dev/sda3 /mnt
mkdir -p /mnt/boot
mount /dev/sda2 /mnt/boot

# install base
pacstrap /mnt base linux linux-firmware nano networkmanager

# fstab
genfstab -U /mnt >> /mnt/etc/fstab

# chroot
arch-chroot /mnt

# timezone/locale/hostname
ln -sf /usr/share/zoneinfo/Asia/Makassar /etc/localtime
hwclock --systohc
# edit /etc/locale.gen -> uncomment en_US.UTF-8 and/or id_ID.UTF-8
locale-gen
echo "LANG=en_US.UTF-8" > /etc/locale.conf
echo "myarch" > /etc/hostname

# root password
passwd

# create user
useradd -m -G wheel -s /bin/bash fred
passwd fred

# sudo
pacman -S sudo
EDITOR=nano visudo   # uncomment %wheel ALL=(ALL:ALL) ALL

# grub UEFI
pacman -S grub efibootmgr dosfstools
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=Arch
grub-mkconfig -o /boot/grub/grub.cfg

# enable network
systemctl enable NetworkManager

# exit & reboot
exit
umount -R /mnt
reboot
```

***
