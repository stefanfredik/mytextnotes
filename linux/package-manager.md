---
icon: camera-polaroid
---

# Package Manager

Di Linux, **file `.tar.gz` atau `.zip` sebenarnya bukan installer**, melainkan arsip. Agar terasa seperti _installer_ (tinggal jalankan, otomatis terpasang), ada beberapa pendekatan **paling umum dan profesional**, tergantung target distribusi dan tingkat kematangan aplikasinya.

Di bawah ini saya jelaskan **dari yang paling sederhana sampai yang standar industri**.

***

### 1️⃣ Cara Paling Sederhana: Installer Shell Script (`install.sh`)

**Cocok untuk:** aplikasi internal, tools DevOps, binary custom.

#### Struktur direktori

```
myapp/
├── bin/myapp
├── config/myapp.conf
├── install.sh
└── uninstall.sh
```

#### Contoh `install.sh`

```bash
#!/bin/bash
set -e

APP_NAME="myapp"
INSTALL_DIR="/opt/$APP_NAME"
BIN_DIR="/usr/local/bin"

echo "Installing $APP_NAME..."

mkdir -p $INSTALL_DIR
cp -r ./* $INSTALL_DIR

ln -sf $INSTALL_DIR/bin/$APP_NAME $BIN_DIR/$APP_NAME

echo "Installation completed."
```

#### Jalankan

```bash
chmod +x install.sh
sudo ./install.sh
```

#### Distribusi

```bash
tar -czvf myapp-installer.tar.gz myapp/
```

✔ **Kelebihan**

* Mudah
* Universal
* Cocok untuk server / ISP tools

✖ **Kekurangan**

* Tidak terintegrasi package manager

***

### 2️⃣ Membuat Installer Satu File (`.run`)

**Dipakai oleh:** NVIDIA, VMware, Oracle

#### Konsep

* Arsip + shell script dalam satu file

#### Contoh sederhana

```bash
cat install.sh myapp.tar.gz > myapp.run
chmod +x myapp.run
```

#### Jalankan

```bash
sudo ./myapp.run
```

✔ Profesional\
✔ Mudah distribusi\
✖ Tidak terkelola oleh sistem paket

***

### 3️⃣ Mengubah `.tar.gz` Menjadi Paket `.deb` (Ubuntu/Debian)

**Ini BEST PRACTICE untuk Ubuntu Server / Desktop**

#### Struktur paket

```
myapp_1.0/
├── DEBIAN
│   └── control
├── usr/local/bin/myapp
└── etc/myapp/myapp.conf
```

#### File `DEBIAN/control`

```
Package: myapp
Version: 1.0
Architecture: amd64
Maintainer: Your Name
Description: My Custom Application
```

#### Build

```bash
dpkg-deb --build myapp_1.0
```

#### Install

```bash
sudo dpkg -i myapp_1.0.deb
```

✔ Terintegrasi APT\
✔ Bisa uninstall dengan rapi\
✔ Cocok untuk Proxmox & Ubuntu

***

### 4️⃣ Mengubah `.tar.gz` Menjadi Paket `.rpm`

**Untuk:** CentOS, Rocky, AlmaLinux

Tools:

```bash
sudo dnf install rpm-build
```

Struktur RPM lebih kompleks, tapi hasilnya **enterprise-grade**.

***

### 5️⃣ AppImage (Portable Installer)

**Cocok untuk:** desktop Linux modern

#### Karakteristik

* Satu file
* Tidak perlu install
* Tinggal double-click

#### Tools

```bash
appimagetool
```

✔ Portable\
✖ Kurang cocok untuk server

***

### 6️⃣ Flatpak / Snap (Modern & Sandboxed)

**Cocok untuk aplikasi GUI**

| Format  | Kelebihan             | Kekurangan         |
| ------- | --------------------- | ------------------ |
| Flatpak | Aman, distro-agnostic | Lebih besar        |
| Snap    | Auto-update           | Performance kadang |

***

### 7️⃣ Installer GUI (Advanced)

Tools:

* `makeself`
* `InstallShield`
* `NSIS (Linux)`
* `Qt Installer Framework`

Dipakai untuk software komersial.

***

### Rekomendasi Berdasarkan Kebutuhan Anda

Sebagai **Network Engineer / Linux / DevOps**, saya sarankan:

| Kebutuhan                   | Solusi                 |
| --------------------------- | ---------------------- |
| Tools server / ISP internal | `install.sh + tar.gz`  |
| Aplikasi produksi Ubuntu    | `.deb`                 |
| Binary portable             | AppImage               |
| Software komersial          | `.run` / GUI installer |

***

### Contoh Real-World (ISP / NMS)

Untuk tools seperti:

* Agent monitoring
* TR-069 client
* Script provisioning OLT

➡ **Gunakan `.deb` + systemd service**

Jika Anda mau, saya bisa:

* Buatkan **template installer.sh**
* Buatkan **template paket `.deb`**
* Buatkan **auto-install + systemd service**
* Konversi **aplikasi Anda sekarang → installer profesional**

Tinggal sebutkan:\
👉 _jenis aplikasi (binary, python, node, go)_\
👉 _target distro_\
👉 _jalan di server atau desktop_
