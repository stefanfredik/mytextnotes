# FTP

## Panduan Lengkap FTP (File Transfer Protocol)

### Apa itu FTP?

File Transfer Protocol (FTP) adalah protokol jaringan standar yang digunakan untuk mentransfer file antara klien dan server melalui jaringan komputer. FTP telah menjadi salah satu metode paling umum untuk mengunggah, mengunduh, dan mengelola file di server sejak tahun 1971.

### Cara Kerja FTP

FTP bekerja dengan model client-server dan menggunakan dua koneksi terpisah:

**1. Control Connection (Port 21)**

* Digunakan untuk mengirim perintah dan menerima respons
* Tetap aktif selama sesi FTP berlangsung
* Menangani autentikasi dan navigasi direktori

**2. Data Connection**

* Digunakan untuk transfer file aktual
* Dibuka saat diperlukan dan ditutup setelah transfer selesai
* Menggunakan port yang berbeda (biasanya port 20 untuk mode aktif)

### Mode Operasi FTP

#### Mode Aktif (Active Mode)

* Server membuat koneksi data ke klien
* Klien mendengarkan pada port tertentu
* Dapat bermasalah dengan firewall dan NAT

#### Mode Pasif (Passive Mode)

* Klien membuat koneksi data ke server
* Server mendengarkan pada port yang ditentukan
* Lebih cocok untuk lingkungan dengan firewall

### Jenis-jenis FTP

#### 1. FTP Standar

* Transfer data tanpa enkripsi
* Username dan password dikirim dalam bentuk plain text
* Tidak aman untuk data sensitif

#### 2. FTPS (FTP Secure)

* FTP dengan enkripsi SSL/TLS
* Menyediakan autentikasi dan enkripsi data
* Lebih aman daripada FTP standar

#### 3. SFTP (SSH File Transfer Protocol)

* Menggunakan protokol SSH untuk keamanan
* Semua data dan perintah dienkripsi
* Tidak sama dengan FTP+SSL/TLS

### Perintah FTP Dasar

#### Perintah Navigasi

```
pwd         - Menampilkan direktori kerja saat ini
ls          - Menampilkan daftar file dan direktori
cd [dir]    - Pindah ke direktori tertentu
cd ..       - Kembali ke direktori parent
mkdir [dir] - Membuat direktori baru
rmdir [dir] - Menghapus direktori kosong
```

#### Perintah Transfer File

```
get [file]     - Mengunduh file dari server
put [file]     - Mengunggah file ke server
mget [files]   - Mengunduh multiple file
mput [files]   - Mengunggah multiple file
delete [file]  - Menghapus file di server
rename [old] [new] - Mengubah nama file
```

#### Perintah Mode Transfer

```
ascii    - Mode transfer untuk file teks
binary   - Mode transfer untuk file biner
passive  - Mengaktifkan mode pasif
active   - Mengaktifkan mode aktif
```

#### Perintah Koneksi

```
open [server]  - Membuka koneksi ke server
close          - Menutup koneksi saat ini
quit/bye       - Keluar dari FTP client
```

### Menggunakan FTP Client

#### Command Line FTP (Windows/Linux/Mac)

**Membuka Koneksi:**

```bash
ftp ftp.example.com
# atau
ftp 192.168.1.100
```

**Login:**

```
Name: username
Password: password
```

**Contoh Sesi FTP:**

```bash
ftp> pwd
257 "/" is current directory
ftp> ls
200 PORT command successful
150 Opening ASCII mode data connection
drwxr-xr-x 2 user user 4096 Jan 01 10:00 documents
-rw-r--r-- 1 user user 1024 Jan 01 10:00 readme.txt
226 Transfer complete
ftp> cd documents
250 CWD command successful
ftp> put myfile.txt
200 PORT command successful
150 Opening BINARY mode data connection
226 Transfer complete
ftp> quit
221 Goodbye
```

#### FTP Client Grafis

**FileZilla (Gratis, Cross-platform)**

1. Download dan install FileZilla
2. Masukkan Host, Username, Password, Port
3. Klik "Quickconnect"
4. Drag and drop file untuk transfer

**WinSCP (Windows)**

1. Buat new session
2. Pilih protocol FTP/SFTP
3. Masukkan hostname dan credentials
4. Login dan mulai transfer file

**Cyberduck (Mac/Windows)**

1. Klik "Open Connection"
2. Pilih FTP dari dropdown
3. Masukkan server details
4. Connect dan kelola file

### Setting FTP Server

#### Windows (IIS FTP Server)

**Mengaktifkan FTP Server:**

1. Control Panel → Programs → Turn Windows features on/off
2. Centang "Internet Information Services"
3. Expand dan centang "FTP Server"
4. Install dan restart

**Konfigurasi FTP Site:**

1. Buka IIS Manager
2. Right-click "Sites" → Add FTP Site
3. Masukkan Site name dan Physical path
4. Set binding (IP address dan port)
5. Konfigurasi authentication dan authorization

#### Linux (vsftpd)

**Instalasi:**

```bash
# Ubuntu/Debian
sudo apt update
sudo apt install vsftpd

# CentOS/RHEL
sudo yum install vsftpd
```

**Konfigurasi (/etc/vsftpd.conf):**

```bash
# Basic settings
listen=YES
anonymous_enable=NO
local_enable=YES
write_enable=YES
local_umask=022

# Security settings
chroot_local_user=YES
allow_writeable_chroot=YES
secure_chroot_dir=/var/run/vsftpd/empty

# Passive mode settings
pasv_enable=YES
pasv_min_port=30000
pasv_max_port=31000
```

**Start Service:**

```bash
sudo systemctl start vsftpd
sudo systemctl enable vsftpd
```

### Keamanan FTP

#### Masalah Keamanan FTP

* Password dikirim dalam plain text
* Data tidak dienkripsi
* Rentan terhadap sniffing dan man-in-the-middle attacks
* Port scanning dapat mengidentifikasi FTP server

#### Best Practices Keamanan

**1. Gunakan FTPS atau SFTP**

* Selalu gunakan koneksi terenkripsi untuk data sensitif
* SFTP lebih direkomendasikan daripada FTPS

**2. Konfigurasi Firewall**

```bash
# Allow FTP control port
sudo ufw allow 21/tcp

# Allow passive mode ports
sudo ufw allow 30000:31000/tcp
```

**3. Buat User FTP Terbatas**

```bash
# Create FTP-only user
sudo useradd -m -d /home/ftpuser -s /usr/sbin/nologin ftpuser
sudo passwd ftpuser

# Set directory permissions
sudo chown ftpuser:ftpuser /home/ftpuser
sudo chmod 755 /home/ftpuser
```

**4. Gunakan SSL Certificate**

```bash
# Generate self-signed certificate
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
    -keyout /etc/ssl/private/vsftpd.pem \
    -out /etc/ssl/private/vsftpd.pem
```

**5. Monitoring dan Logging**

```bash
# Enable logging in vsftpd.conf
log_ftp_protocol=YES
xferlog_enable=YES
xferlog_file=/var/log/vsftpd.log
```

### Troubleshooting FTP

#### Masalah Koneksi Umum

**1. Connection Timeout**

* Periksa firewall settings
* Pastikan FTP server berjalan
* Cek network connectivity

**2. Passive Mode Issues**

* Konfigurasi passive port range
* Update firewall rules
* Periksa NAT settings

**3. Transfer Mode Problems**

* Gunakan binary mode untuk file biner
* Gunakan ASCII mode untuk file teks
* Periksa file permissions

#### Debug Commands

```bash
# Test FTP connection
telnet ftp.example.com 21

# Check FTP server status
sudo systemctl status vsftpd

# View FTP logs
sudo tail -f /var/log/vsftpd.log

# Test passive mode
ftp> passive
```

### Alternatif Modern untuk FTP

#### 1. SFTP (SSH File Transfer Protocol)

* Lebih aman dengan enkripsi SSH
* Single port (22) lebih firewall-friendly
* Integrated dengan SSH authentication

#### 2. SCP (Secure Copy Protocol)

* Simple file copying over SSH
* Good untuk transfer one-time
* Command line: `scp file.txt user@server:/path/`

#### 3. rsync

* Efficient synchronization tool
* Delta transfer (hanya perubahan)
* Command: `rsync -avz local/ user@server:remote/`

#### 4. HTTP/HTTPS File Transfer

* WebDAV untuk web-based file management
* REST APIs untuk programmatic access
* Cloud storage solutions (AWS S3, Google Drive)

### Kesimpulan

FTP tetap menjadi protokol yang berguna untuk transfer file, meskipun memiliki keterbatasan keamanan. Untuk penggunaan modern, pertimbangkan alternatif yang lebih aman seperti SFTP atau HTTPS-based solutions. Selalu implementasikan best practices keamanan saat menggunakan FTP, terutama dalam environment production.

Pemahaman yang baik tentang FTP membantu dalam troubleshooting masalah transfer file dan memilih solusi yang tepat untuk kebutuhan spesifik organisasi Anda.
