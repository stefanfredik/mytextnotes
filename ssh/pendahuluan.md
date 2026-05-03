---
icon: square-list
---

# Pendahuluan

## Dokumentasi SSH (Secure Shell) - Panduan Lengkap & Komprehensif

***

### Daftar Isi

1. Pendahuluan
2. Sejarah SSH
3. Arsitektur & Cara Kerja
4. Instalasi SSH
5. Konfigurasi SSH Server
6. Konfigurasi SSH Client
7. Autentikasi SSH
8. SSH Key Management
9. Perintah-Perintah SSH
10. SSH Tunneling & Port Forwarding
11. SCP & SFTP
12. SSH Agent
13. Keamanan SSH
14. Troubleshooting
15. Use Cases Lanjutan
16. Best Practices

***

### 1. Pendahuluan <a href="#pendahuluan" id="pendahuluan"></a>

#### Apa itu SSH?

**SSH (Secure Shell)** adalah protokol jaringan kriptografi yang dirancang untuk komunikasi data yang aman antara dua komputer melalui jaringan yang tidak aman. SSH menyediakan:

* **Enkripsi end-to-end** untuk semua data yang ditransmisikan
* **Autentikasi** yang kuat untuk memverifikasi identitas
* **Integritas data** untuk memastikan data tidak dimodifikasi
* **Tunneling** untuk mengamankan protokol lain

#### Mengapa SSH Penting?

```
┌─────────────────────────────────────────────────────────────────┐
│                    Tanpa SSH (Telnet/rsh)                       │
│                                                                 │
│  [Client] ──── Username/Password PLAINTEXT ────► [Server]      │
│              ← Data TIDAK terenkripsi ←                        │
│           ⚠️  Rentan terhadap sniffing & MITM attack            │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│                       Dengan SSH                                │
│                                                                 │
│  [Client] ──── Data TERENKRIPSI & TERAUTENTIKASI ───► [Server]  │
│              ← Komunikasi Aman →                               │
│           ✅  Terlindungi dari sniffing & MITM attack           │
└─────────────────────────────────────────────────────────────────┘
```

#### Perbandingan SSH vs Protokol Lama

| Fitur            | Telnet  | rsh/rlogin | SSH  |
| ---------------- | ------- | ---------- | ---- |
| Enkripsi         | ❌ Tidak | ❌ Tidak    | ✅ Ya |
| Autentikasi Kuat | ❌ Tidak | ❌ Tidak    | ✅ Ya |
| Integritas Data  | ❌ Tidak | ❌ Tidak    | ✅ Ya |
| Port Forwarding  | ❌ Tidak | ❌ Tidak    | ✅ Ya |
| Transfer File    | ❌ Tidak | ❌ Terbatas | ✅ Ya |
| Port Default     | 23      | 514        | 22   |

***

### 2. Sejarah SSH <a href="#sejarah" id="sejarah"></a>

```
Timeline Perkembangan SSH
═════════════════════════

1995 ──► SSH-1 dikembangkan oleh Tatu Ylönen (Finlandia)
│        Dibuat sebagai respons terhadap serangan packet sniffer
│        di jaringan Universitas Helsinki
│
1996 ──► SSH-2 dirancang dengan perbaikan keamanan signifikan
│        Mengatasi kelemahan kriptografi SSH-1
│
1999 ──► OpenSSH dirilis oleh tim OpenBSD
│        Implementasi open-source SSH yang paling populer
│
2006 ──► SSH-2 menjadi standar Internet (RFC 4251-4256)
│
2011 ──► SSH diperkuat dengan dukungan algoritma modern
│
Kini ──► SSH versi 2 adalah standar industri
         OpenSSH digunakan di ~90% server Linux/Unix
```

#### Versi SSH

**SSH-1:**

* Protokol asli (1995)
* Memiliki kerentanan kriptografi
* **TIDAK LAGI DIREKOMENDASIKAN**

**SSH-2:**

* Desain ulang lengkap
* Keamanan yang jauh lebih baik
* Standar saat ini yang digunakan

***

### 3. Arsitektur & Cara Kerja <a href="#arsitektur" id="arsitektur"></a>

#### Lapisan Protokol SSH

```
┌──────────────────────────────────────────────────────────────┐
│                    SSH Protocol Stack                        │
├──────────────────────────────────────────────────────────────┤
│  Layer 4: SSH Connection Protocol                           │
│           (Channels, Port Forwarding, Shell, Exec)          │
├──────────────────────────────────────────────────────────────┤
│  Layer 3: SSH Authentication Protocol                       │
│           (Password, Public Key, Keyboard-Interactive)      │
├──────────────────────────────────────────────────────────────┤
│  Layer 2: SSH Transport Layer Protocol                      │
│           (Encryption, Integrity, Key Exchange)             │
├──────────────────────────────────────────────────────────────┤
│  Layer 1: TCP/IP (Port 22)                                  │
└──────────────────────────────────────────────────────────────┘
```

#### Proses Handshake SSH (Detail)

```
CLIENT                                              SERVER
  │                                                   │
  │──── TCP SYN ─────────────────────────────────────►│
  │◄─── TCP SYN-ACK ──────────────────────────────────│
  │──── TCP ACK ─────────────────────────────────────►│
  │                                                   │
  │         [PHASE 1: Protocol Version Exchange]      │
  │◄─── "SSH-2.0-OpenSSH_8.9" ────────────────────────│
  │──── "SSH-2.0-OpenSSH_8.9" ────────────────────────►│
  │                                                   │
  │         [PHASE 2: Algorithm Negotiation]          │
  │──── SSH_MSG_KEXINIT ─────────────────────────────►│
  │     (Daftar algoritma yang didukung client)       │
  │◄─── SSH_MSG_KEXINIT ──────────────────────────────│
  │     (Daftar algoritma yang didukung server)       │
  │                                                   │
  │         [PHASE 3: Key Exchange (Diffie-Hellman)]  │
  │──── SSH_MSG_KEXDH_INIT ──────────────────────────►│
  │     (Client DH public value)                     │
  │◄─── SSH_MSG_KEXDH_REPLY ──────────────────────────│
  │     (Server public key, DH value, Signature)     │
  │                                                   │
  │         [PHASE 4: Verify Server Identity]         │
  │     Client memverifikasi host key server          │
  │     (cek ~/.ssh/known_hosts)                      │
  │                                                   │
  │         [PHASE 5: Session Key Derivation]         │
  │     Kedua pihak menghitung session key yang sama  │
  │     Semua komunikasi selanjutnya TERENKRIPSI      │
  │                                                   │
  │──── SSH_MSG_NEWKEYS ─────────────────────────────►│
  │◄─── SSH_MSG_NEWKEYS ──────────────────────────────│
  │                                                   │
  │         [PHASE 6: User Authentication]            │
  │──── Auth Request (password/key) ─────────────────►│
  │◄─── Auth Success/Failure ─────────────────────────│
  │                                                   │
  │         [PHASE 7: Session/Channel Open]           │
  │──── SSH_MSG_CHANNEL_OPEN ────────────────────────►│
  │◄─── SSH_MSG_CHANNEL_OPEN_CONFIRMATION ────────────│
  │                                                   │
  │         [SESSION AKTIF - Data Terenkripsi]        │
  │◄══════════════════════════════════════════════════│
```

#### Algoritma Kriptografi dalam SSH

**Key Exchange Algorithms**

```
- Curve25519 (Direkomendasikan - Modern & Cepat)
- ECDH (Elliptic Curve Diffie-Hellman)
- DH Group 14, 16, 18 (Diffie-Hellman)
- DH Group Exchange
```

**Symmetric Encryption Algorithms**

```
- AES-256-GCM (Direkomendasikan)
- AES-128-GCM
- ChaCha20-Poly1305 (Direkomendasikan)
- AES-256-CTR
- AES-128-CTR
```

**Message Authentication Code (MAC)**

```
- HMAC-SHA2-512 (Direkomendasikan)
- HMAC-SHA2-256
- UMAC-128
```

**Host Key / Public Key Algorithms**

```
- Ed25519 (Direkomendasikan - Modern)
- ECDSA (256, 384, 521 bit)
- RSA (minimum 2048 bit, lebih baik 4096 bit)
- DSA (TIDAK LAGI DIREKOMENDASIKAN)
```

***

### 4. Instalasi SSH <a href="#instalasi" id="instalasi"></a>

#### Instalasi di Linux

**Ubuntu/Debian**

```bash
# Install SSH Server
sudo apt update
sudo apt install openssh-server -y

# Install SSH Client (biasanya sudah terinstall)
sudo apt install openssh-client -y

# Cek status
sudo systemctl status ssh

# Start SSH server
sudo systemctl start ssh

# Enable auto-start saat boot
sudo systemctl enable ssh

# Stop SSH server
sudo systemctl stop ssh

# Restart SSH server
sudo systemctl restart ssh
```

**CentOS/RHEL/Fedora**

```bash
# Install SSH Server
sudo dnf install openssh-server -y
# atau untuk versi lama
sudo yum install openssh-server -y

# Install SSH Client
sudo dnf install openssh-clients -y

# Start dan enable SSH
sudo systemctl start sshd
sudo systemctl enable sshd

# Cek status
sudo systemctl status sshd
```

**Arch Linux**

```bash
# Install
sudo pacman -S openssh

# Enable dan start
sudo systemctl enable sshd
sudo systemctl start sshd
```

#### Instalasi di macOS

```bash
# macOS sudah dilengkapi SSH client secara default
# Untuk mengaktifkan SSH server:

# Method 1: System Preferences
# System Preferences → Sharing → Remote Login → Enable

# Method 2: Command Line
sudo systemsetup -setremotelogin on

# Cek status
sudo systemsetup -getremotelogin

# Via launchctl
sudo launchctl load -w /System/Library/LaunchDaemons/ssh.plist
```

#### Instalasi di Windows

**Method 1: OpenSSH (Windows 10/11 Built-in)**

```powershell
# Cek ketersediaan (PowerShell as Administrator)
Get-WindowsCapability -Online | Where-Object Name -like 'OpenSSH*'

# Install OpenSSH Client
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0

# Install OpenSSH Server
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0

# Start SSH Server
Start-Service sshd

# Set auto-start
Set-Service -Name sshd -StartupType 'Automatic'

# Cek status
Get-Service -Name sshd

# Konfigurasi Firewall
New-NetFirewallRule -Name sshd -DisplayName 'OpenSSH Server (sshd)' `
  -Enabled True -Direction Inbound -Protocol TCP `
  -Action Allow -LocalPort 22
```

**Method 2: PuTTY (GUI Client)**

```
1. Download dari: https://putty.org
2. Install PuTTY
3. Gunakan PuTTYgen untuk generate keys
4. Gunakan PuTTY untuk koneksi SSH
```

#### Verifikasi Instalasi

```bash
# Cek versi SSH
ssh -V
# Output: OpenSSH_8.9p1 Ubuntu-3ubuntu0.6, OpenSSL 3.0.2 ...

# Cek port yang digunakan
sudo ss -tlnp | grep sshd
# atau
sudo netstat -tlnp | grep sshd

# Cek proses SSH
ps aux | grep sshd
```

***

### 5. Konfigurasi SSH Server <a href="#konfigurasi-server" id="konfigurasi-server"></a>

#### File Konfigurasi Utama

```bash
# Lokasi file konfigurasi server
/etc/ssh/sshd_config

# Backup sebelum edit
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.backup

# Edit konfigurasi
sudo nano /etc/ssh/sshd_config
# atau
sudo vim /etc/ssh/sshd_config
```

#### Konfigurasi sshd\_config Lengkap

```bash
# ================================================================
# /etc/ssh/sshd_config - Konfigurasi SSH Server
# ================================================================

# ─────────────────────────────────────────
# NETWORK SETTINGS
# ─────────────────────────────────────────

# Port SSH (default: 22)
# Mengubah port bukan solusi keamanan utama, hanya mengurangi noise
Port 22

# Listen address (0.0.0.0 = semua interface)
ListenAddress 0.0.0.0
ListenAddress ::

# Protocol version (hanya SSH-2)
# Protocol 2  # Deprecated directive, SSH-2 adalah default

# Address family (any, inet=IPv4, inet6=IPv6)
AddressFamily any

# ─────────────────────────────────────────
# AUTHENTICATION SETTINGS
# ─────────────────────────────────────────

# Maksimal waktu untuk autentikasi (default: 120s)
LoginGraceTime 30

# Izinkan root login
# yes, no, prohibit-password (hanya key auth untuk root), forced-commands-only
PermitRootLogin prohibit-password

# Jumlah percobaan autentikasi maksimal
MaxAuthTries 3

# Autentikasi dengan Public Key
PubkeyAuthentication yes

# Lokasi file authorized keys
AuthorizedKeysFile .ssh/authorized_keys .ssh/authorized_keys2

# Autentikasi Password
PasswordAuthentication yes
# REKOMENDASI: Set ke 'no' jika sudah setup key-based auth

# Izinkan password kosong
PermitEmptyPasswords no

# Challenge-response authentication
ChallengeResponseAuthentication no

# Kerberos authentication
KerberosAuthentication no
KerberosOrLocalPasswd yes
KerberosTicketCleanup yes

# GSSAPI authentication
GSSAPIAuthentication no
GSSAPICleanupCredentials yes

# ─────────────────────────────────────────
# SESSION SETTINGS
# ─────────────────────────────────────────

# Maksimal sesi per koneksi
MaxSessions 10

# Maksimal koneksi yang belum terautentikasi
# start:rate:full
MaxStartups 10:30:100

# Timeout idle (dalam detik)
# Koneksi akan diputus setelah idle
ClientAliveInterval 300
ClientAliveCountMax 3

# TCPKeepAlive
TCPKeepAlive yes

# ─────────────────────────────────────────
# FEATURES & CAPABILITIES
# ─────────────────────────────────────────

# X11 Forwarding (GUI forwarding)
X11Forwarding yes
X11DisplayOffset 10
X11UseLocalhost yes

# Agent Forwarding
AllowAgentForwarding yes

# TCP Port Forwarding
AllowTcpForwarding yes

# Tunnel (VPN-like)
PermitTunnel no

# Compression (yes, no, delayed)
Compression delayed

# Print Message of the Day
PrintMotd no

# Print last login info
PrintLastLog yes

# ─────────────────────────────────────────
# CRYPTOGRAPHY SETTINGS (Modern & Secure)
# ─────────────────────────────────────────

# Key Exchange Algorithms (Urutan = Preferensi)
KexAlgorithms curve25519-sha256,curve25519-sha256@libssh.org,\
diffie-hellman-group16-sha512,diffie-hellman-group18-sha512,\
diffie-hellman-group14-sha256

# Cipher (Enkripsi)
Ciphers chacha20-poly1305@openssh.com,aes256-gcm@openssh.com,\
aes128-gcm@openssh.com,aes256-ctr,aes192-ctr,aes128-ctr

# Message Authentication Code
MACs hmac-sha2-512-etm@openssh.com,hmac-sha2-256-etm@openssh.com,\
umac-128-etm@openssh.com,hmac-sha2-512,hmac-sha2-256

# Host Key Algorithms
HostKeyAlgorithms ssh-ed25519,ssh-ed25519-cert-v01@openssh.com,\
ecdsa-sha2-nistp256,ecdsa-sha2-nistp384,ecdsa-sha2-nistp521,\
rsa-sha2-512,rsa-sha2-256

# ─────────────────────────────────────────
# HOST KEYS
# ─────────────────────────────────────────

HostKey /etc/ssh/ssh_host_ed25519_key
HostKey /etc/ssh/ssh_host_ecdsa_key
HostKey /etc/ssh/ssh_host_rsa_key

# ─────────────────────────────────────────
# LOGGING
# ─────────────────────────────────────────

# Syslog facility
SyslogFacility AUTH

# Log level (QUIET, FATAL, ERROR, INFO, VERBOSE, DEBUG, DEBUG1-3)
LogLevel VERBOSE

# ─────────────────────────────────────────
# ACCESS CONTROL
# ─────────────────────────────────────────

# Izinkan user tertentu (whitelist)
# AllowUsers alice bob carol@192.168.1.*

# Tolak user tertentu (blacklist)
# DenyUsers mallory eve

# Izinkan group tertentu
# AllowGroups sshusers admins

# Tolak group tertentu
# DenyGroups nologin

# ─────────────────────────────────────────
# SFTP SUBSYSTEM
# ─────────────────────────────────────────

Subsystem sftp /usr/lib/openssh/sftp-server
# atau untuk internal SFTP
# Subsystem sftp internal-sftp

# ─────────────────────────────────────────
# MATCH BLOCKS (Conditional Configuration)
# ─────────────────────────────────────────

# Konfigurasi khusus untuk user tertentu
# Match User ftpuser
#     ChrootDirectory /home/ftpuser
#     ForceCommand internal-sftp
#     AllowTcpForwarding no
#     X11Forwarding no

# Konfigurasi khusus untuk group
# Match Group admins
#     PasswordAuthentication yes
#     AllowAgentForwarding yes

# Konfigurasi khusus untuk IP address
# Match Address 192.168.1.0/24
#     PasswordAuthentication yes

# Konfigurasi khusus untuk user dari IP tertentu
# Match User deploy Address 10.0.0.0/8
#     ForceCommand /opt/deploy/script.sh
```

#### Validasi Konfigurasi

```bash
# Test konfigurasi tanpa restart
sudo sshd -t

# Test dengan output verbose
sudo sshd -T

# Lihat konfigurasi aktif
sudo sshd -T | grep -i port
sudo sshd -T | grep -i auth

# Reload konfigurasi (tanpa disconnect existing sessions)
sudo systemctl reload sshd
# atau
sudo kill -HUP $(cat /var/run/sshd.pid)
```

***

### 6. Konfigurasi SSH Client <a href="#konfigurasi-client" id="konfigurasi-client"></a>

#### File Konfigurasi Client

```bash
# Lokasi file konfigurasi
~/.ssh/config          # User-specific (lebih diutamakan)
/etc/ssh/ssh_config    # System-wide

# Buat/edit konfigurasi user
mkdir -p ~/.ssh
chmod 700 ~/.ssh
nano ~/.ssh/config
```

#### Contoh \~/.ssh/config Lengkap

```bash
# ================================================================
# ~/.ssh/config - Konfigurasi SSH Client
# ================================================================

# ─────────────────────────────────────────
# DEFAULT SETTINGS (berlaku untuk semua host)
# ─────────────────────────────────────────
Host *
    # Protokol SSH
    Protocol 2
    
    # Algoritma modern
    KexAlgorithms curve25519-sha256,curve25519-sha256@libssh.org
    Ciphers chacha20-poly1305@openssh.com,aes256-gcm@openssh.com
    MACs hmac-sha2-512-etm@openssh.com,hmac-sha2-256-etm@openssh.com
    
    # Koneksi keepalive
    ServerAliveInterval 60
    ServerAliveCountMax 3
    
    # Kompresi
    Compression yes
    
    # Simpan koneksi untuk reuse
    ControlMaster auto
    ControlPath ~/.ssh/control/%r@%h:%p
    ControlPersist 10m
    
    # Hash known hosts
    HashKnownHosts yes
    
    # Verifikasi host key
    StrictHostKeyChecking ask
    
    # Identitas default
    IdentityFile ~/.ssh/id_ed25519
    IdentityFile ~/.ssh/id_rsa

# ─────────────────────────────────────────
# PRODUCTION SERVER
# ─────────────────────────────────────────
Host prod
    HostName 203.0.113.10
    User admin
    Port 22
    IdentityFile ~/.ssh/prod_ed25519
    ForwardAgent no
    ServerAliveInterval 30

# ─────────────────────────────────────────
# DEVELOPMENT SERVER
# ─────────────────────────────────────────
Host dev
    HostName 192.168.1.100
    User developer
    Port 22
    IdentityFile ~/.ssh/dev_rsa
    ForwardAgent yes

# ─────────────────────────────────────────
# BASTION/JUMP HOST
# ─────────────────────────────────────────
Host bastion
    HostName bastion.example.com
    User jumpuser
    Port 22
    IdentityFile ~/.ssh/bastion_key

# Server di balik bastion host
Host internal-server
    HostName 10.0.0.50
    User internaluser
    ProxyJump bastion
    # Atau syntax lama:
    # ProxyCommand ssh -W %h:%p bastion

# ─────────────────────────────────────────
# GITHUB
# ─────────────────────────────────────────
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/github_ed25519
    IdentitiesOnly yes

# ─────────────────────────────────────────
# GITLAB
# ─────────────────────────────────────────
Host gitlab.com
    HostName gitlab.com
    User git
    IdentityFile ~/.ssh/gitlab_ed25519

# ─────────────────────────────────────────
# SERVER DENGAN PORT CUSTOM
# ─────────────────────────────────────────
Host custom-port
    HostName server.example.com
    User myuser
    Port 2222
    
# ─────────────────────────────────────────
# LOCAL TUNNEL (Port Forwarding)
# ─────────────────────────────────────────
Host db-tunnel
    HostName server.example.com
    User tunneluser
    LocalForward 5432 localhost:5432
    # Akses database dengan: psql -h localhost -p 5432

# ─────────────────────────────────────────
# SFTP-ONLY USER (Chrooted)
# ─────────────────────────────────────────
Host sftp-server
    HostName sftp.example.com
    User sftpuser
    Port 22
    IdentityFile ~/.ssh/sftp_key
    Subsystem sftp internal-sftp

# ─────────────────────────────────────────
# WILDCARD MATCHING
# ─────────────────────────────────────────
Host *.internal.example.com
    User admin
    IdentityFile ~/.ssh/internal_key
    ProxyJump bastion
```

#### Membuat Direktori Control

```bash
# Buat direktori untuk ControlPath
mkdir -p ~/.ssh/control
chmod 700 ~/.ssh/control
```

***

### 7. Autentikasi SSH <a href="#autentikasi" id="autentikasi"></a>

#### Metode Autentikasi

**1. Password Authentication**

```bash
# Koneksi dengan password (akan diminta memasukkan password)
ssh user@hostname

# Atau dengan password inline (TIDAK DIREKOMENDASIKAN untuk keamanan)
# sshpass diperlukan
sshpass -p 'password123' ssh user@hostname

# Install sshpass
sudo apt install sshpass  # Debian/Ubuntu
sudo yum install sshpass  # CentOS/RHEL
```

**2. Public Key Authentication (Direkomendasikan)**

```
┌─────────────────────────────────────────────────────────────┐
│              Public Key Authentication Flow                  │
│                                                             │
│  CLIENT                              SERVER                 │
│                                                             │
│  Private Key (RAHASIA)   Public Key (di server)            │
│  ~/.ssh/id_ed25519  ───► ~/.ssh/authorized_keys            │
│                                                             │
│  1. Client: "Saya ingin login sebagai 'user'"              │
│  2. Server: "Buktikan! Ini challenge terenkripsi"          │
│  3. Client: Dekripsi dengan private key, kirim proof        │
│  4. Server: Verifikasi dengan public key → AUTH SUCCESS     │
└─────────────────────────────────────────────────────────────┘
```

**3. Multi-Factor Authentication (MFA)**

```bash
# Install Google Authenticator
sudo apt install libpam-google-authenticator

# Setup untuk user
google-authenticator

# Konfigurasi PAM
sudo nano /etc/pam.d/sshd
# Tambahkan:
# auth required pam_google_authenticator.so

# Konfigurasi sshd_config
sudo nano /etc/ssh/sshd_config
# Tambahkan/ubah:
# ChallengeResponseAuthentication yes
# AuthenticationMethods publickey,keyboard-interactive
```

**4. Certificate-Based Authentication**

```bash
# Membuat CA (Certificate Authority)
ssh-keygen -t ed25519 -f ssh_ca -C "SSH CA"

# Sign user key dengan CA
ssh-keygen -s ssh_ca -I "user_id" -n "username" \
  -V +52w ~/.ssh/id_ed25519.pub
# -s = signing key (CA private key)
# -I = certificate identity
# -n = principals (usernames yang diizinkan)
# -V = validity period (+52w = 52 minggu dari sekarang)

# Hasil: ~/.ssh/id_ed25519-cert.pub

# Konfigurasi server untuk mempercayai CA
echo "TrustedUserCAKeys /etc/ssh/ca.pub" >> /etc/ssh/sshd_config

# Copy CA public key ke server
sudo cp ssh_ca.pub /etc/ssh/ca.pub

# Koneksi menggunakan certificate
ssh -i ~/.ssh/id_ed25519 user@server
```

***

### 8. SSH Key Management <a href="#key-management" id="key-management"></a>

#### Generate SSH Key Pairs

**Ed25519 (Direkomendasikan - Modern)**

```bash
# Generate Ed25519 key (PALING DIREKOMENDASIKAN)
ssh-keygen -t ed25519 -C "email@example.com"

# Dengan nama file custom
ssh-keygen -t ed25519 -C "email@example.com" -f ~/.ssh/myserver_ed25519

# Output:
# Generating public/private ed25519 key pair.
# Enter file in which to save the key (/home/user/.ssh/id_ed25519):
# Enter passphrase (empty for no passphrase): [MASUKKAN PASSPHRASE!]
# Enter same passphrase again:
# Your identification has been saved in /home/user/.ssh/id_ed25519
# Your public key has been saved in /home/user/.ssh/id_ed25519.pub
```

**RSA (Kompatibilitas Luas)**

```bash
# Generate RSA key 4096 bit
ssh-keygen -t rsa -b 4096 -C "email@example.com"

# RSA dengan nama custom
ssh-keygen -t rsa -b 4096 -C "email@example.com" -f ~/.ssh/myserver_rsa
```

**ECDSA**

```bash
# Generate ECDSA key
ssh-keygen -t ecdsa -b 521 -C "email@example.com"
```

**Perbandingan Jenis Key**

| Tipe      | Ukuran   | Keamanan      | Performa     | Rekomendasi      |
| --------- | -------- | ------------- | ------------ | ---------------- |
| Ed25519   | 256 bit  | Sangat Tinggi | Sangat Cepat | ⭐ Utama          |
| ECDSA-521 | 521 bit  | Sangat Tinggi | Cepat        | ✅ OK             |
| RSA-4096  | 4096 bit | Tinggi        | Lambat       | ✅ Kompatibilitas |
| RSA-2048  | 2048 bit | Cukup         | Sedang       | ⚠️ Minimum       |
| DSA       | 1024 bit | RENDAH        | -            | ❌ Jangan         |

#### Menyalin Public Key ke Server

```bash
# Method 1: ssh-copy-id (Termudah)
ssh-copy-id -i ~/.ssh/id_ed25519.pub user@hostname

# Dengan port custom
ssh-copy-id -i ~/.ssh/id_ed25519.pub -p 2222 user@hostname

# Method 2: Manual
# Baca public key
cat ~/.ssh/id_ed25519.pub

# Salin output, lalu di server:
mkdir -p ~/.ssh
chmod 700 ~/.ssh
echo "PASTE_PUBLIC_KEY_HERE" >> ~/.ssh/authorized_keys
chmod 600 ~/.ssh/authorized_keys

# Method 3: Pipe
cat ~/.ssh/id_ed25519.pub | ssh user@hostname "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys"

# Method 4: Untuk server yang sudah bisa diakses
ssh user@hostname "mkdir -p ~/.ssh; echo '$(cat ~/.ssh/id_ed25519.pub)' >> ~/.ssh/authorized_keys; chmod 700 ~/.ssh; chmod 600 ~/.ssh/authorized_keys"
```

#### Mengelola Authorized Keys

```bash
# Lihat authorized keys
cat ~/.ssh/authorized_keys

# Format file authorized_keys:
# [options] keytype key comment
# ssh-ed25519 AAAA...xyz user@machine
# no-pty,no-X11-forwarding ssh-ed25519 AAAA...xyz restricted-user

# Options yang tersedia:
# no-port-forwarding    = Disable port forwarding
# no-X11-forwarding     = Disable X11 forwarding
# no-agent-forwarding   = Disable agent forwarding
# no-pty               = Disable TTY allocation
# command="cmd"        = Force specific command
# from="IP"            = Restrict source IP
# expiry-time="date"   = Set expiry

# Contoh restricted key
echo 'from="192.168.1.0/24",no-pty,command="/usr/local/bin/backup.sh" ssh-ed25519 AAAA...xyz backup-user' >> ~/.ssh/authorized_keys
```

#### Manajemen Key di Lokal

```bash
# Lihat semua key yang tersimpan
ls -la ~/.ssh/

# Lihat fingerprint key
ssh-keygen -l -f ~/.ssh/id_ed25519.pub
# Output: 256 SHA256:ABC...xyz email@example.com (ED25519)

# Lihat fingerprint dalam format lama (MD5)
ssh-keygen -l -E md5 -f ~/.ssh/id_ed25519.pub

# Tampilkan random art
ssh-keygen -lv -f ~/.ssh/id_ed25519.pub

# Ubah passphrase key
ssh-keygen -p -f ~/.ssh/id_ed25519

# Ubah comment key
ssh-keygen -c -C "new-comment@example.com" -f ~/.ssh/id_ed25519

# Convert format key (PEM ke OpenSSH)
ssh-keygen -p -m OpenSSH -f ~/.ssh/id_rsa

# Extract public key dari private key
ssh-keygen -y -f ~/.ssh/id_ed25519 > ~/.ssh/id_ed25519.pub
```

#### Keamanan File SSH

```bash
# Permission yang WAJIB untuk SSH
chmod 700 ~/.ssh                        # Directory SSH
chmod 600 ~/.ssh/id_ed25519             # Private key
chmod 644 ~/.ssh/id_ed25519.pub         # Public key
chmod 600 ~/.ssh/authorized_keys        # Authorized keys
chmod 600 ~/.ssh/config                 # Config file
chmod 600 ~/.ssh/known_hosts            # Known hosts

# Script untuk fix semua permission sekaligus
fix_ssh_permissions() {
    chmod 700 ~/.ssh
    chmod 600 ~/.ssh/* 2>/dev/null
    chmod 644 ~/.ssh/*.pub 2>/dev/null
    echo "SSH permissions fixed!"
}
```

#### Known Hosts Management

```bash
# File known_hosts
cat ~/.ssh/known_hosts

# Hapus entry host tertentu (berguna saat host key berubah)
ssh-keygen -R hostname
ssh-keygen -R 192.168.1.100

# Hapus entry dengan port custom
ssh-keygen -R [hostname]:2222

# Scan dan tambahkan host key ke known_hosts
ssh-keyscan hostname >> ~/.ssh/known_hosts
ssh-keyscan -H hostname >> ~/.ssh/known_hosts  # Hash hostname

# Scan dengan port custom
ssh-keyscan -p 2222 hostname >> ~/.ssh/known_hosts

# Scan tipe key tertentu
ssh-keyscan -t ed25519 hostname >> ~/.ssh/known_hosts

# Verifikasi known_hosts
ssh-keygen -F hostname          # Cari host di known_hosts
ssh-keygen -l -F hostname       # Lihat fingerprint
```

***

### 9. Perintah-Perintah SSH <a href="#perintah" id="perintah"></a>

#### Koneksi Dasar

```bash
# Koneksi SSH dasar
ssh user@hostname
ssh user@ip_address

# Dengan port custom
ssh -p 2222 user@hostname

# Dengan identity file tertentu
ssh -i ~/.ssh/mykey user@hostname

# Verbose mode (debugging)
ssh -v user@hostname      # Level 1
ssh -vv user@hostname     # Level 2
ssh -vvv user@hostname    # Level 3 (Paling detail)

# Disable strict host key checking (TIDAK AMAN untuk produksi)
ssh -o StrictHostKeyChecking=no user@hostname

# Koneksi dengan timeout
ssh -o ConnectTimeout=10 user@hostname

# Disable password authentication
ssh -o PasswordAuthentication=no user@hostname

# Gunakan config file berbeda
ssh -F /path/to/config user@hostname

# Koneksi dengan environment variable
ssh -o SendEnv=MY_VAR user@hostname
```

#### Eksekusi Remote Command

```bash
# Jalankan command di remote server
ssh user@hostname "command"
ssh user@hostname "ls -la /home"
ssh user@hostname "df -h"
ssh user@hostname "free -h"

# Jalankan multiple commands
ssh user@hostname "command1; command2; command3"
ssh user@hostname "cd /var/log && tail -100 syslog"

# Jalankan script lokal di server remote
ssh user@hostname < local_script.sh
cat local_script.sh | ssh user@hostname bash

# Jalankan command dengan sudo
ssh user@hostname "sudo command"
ssh -t user@hostname "sudo command"  # -t untuk alokasi PTY

# Jalankan command dengan heredoc
ssh user@hostname << 'EOF'
    echo "Hello from remote"
    df -h
    uptime
EOF

# Tangkap output dari remote
output=$(ssh user@hostname "hostname && date")
echo "$output"

# Pipe data ke dan dari remote
local_data | ssh user@hostname "remote_command"
ssh user@hostname "remote_command" | local_command
```

#### SSH dengan Pseudo-Terminal (PTY)

```bash
# Alokasi PTY (diperlukan untuk interactive commands)
ssh -t user@hostname "top"
ssh -t user@hostname "htop"
ssh -t user@hostname "sudo vim /etc/hosts"

# Force no PTY
ssh -T user@hostname "command"

# Multiple -t untuk nested SSH
ssh -t gateway "ssh -t internal-server 'sudo bash'"
```

#### File Transfer dengan SSH

```bash
# SCP - Secure Copy Protocol
# Syntax: scp [options] source destination

# Copy file dari lokal ke remote
scp file.txt user@hostname:/remote/path/
scp file.txt user@hostname:~/

# Copy file dari remote ke lokal
scp user@hostname:/remote/path/file.txt /local/path/

# Copy direktori secara rekursif
scp -r /local/directory/ user@hostname:/remote/path/

# Copy dengan port custom
scp -P 2222 file.txt user@hostname:/remote/path/

# Copy dengan progress bar dan kompresi
scp -Cv file.txt user@hostname:/remote/path/

# Copy antara dua remote server
scp user1@host1:/path/file.txt user2@host2:/path/

# Preserve timestamps dan permissions
scp -p file.txt user@hostname:/remote/path/

# Bandwidth limit (dalam Kbps)
scp -l 1024 file.txt user@hostname:/remote/path/

# Copy multiple files
scp file1.txt file2.txt file3.txt user@hostname:/remote/path/

# Wildcard
scp /local/path/*.txt user@hostname:/remote/path/
```

***

### 10. SSH Tunneling & Port Forwarding <a href="#tunneling" id="tunneling"></a>

#### Jenis-Jenis Tunneling

```
┌────────────────────────────────────────────────────────────────┐
│                  Jenis SSH Tunneling                           │
│                                                                │
│  1. LOCAL PORT FORWARDING (-L)                                 │
│     Client → [Encrypted Tunnel] → Server → Destination        │
│     Akses remote service dari lokal                           │
│                                                                │
│  2. REMOTE PORT FORWARDING (-R)                                │
│     Destination → Server → [Encrypted Tunnel] → Client        │
│     Expose local service ke remote                            │
│                                                                │
│  3. DYNAMIC PORT FORWARDING (-D)                               │
│     SOCKS Proxy melalui SSH                                   │
│     Semua traffic melalui tunnel                              │
└────────────────────────────────────────────────────────────────┘
```

#### 1. Local Port Forwarding

```bash
# Syntax:
# ssh -L [local_addr:]local_port:remote_host:remote_port user@ssh_server

# ─────────────────────────────────────────
# USE CASE: Akses database remote secara aman
# ─────────────────────────────────────────

# Scenario:
# - SSH Server: myserver.com (bisa diakses dari internet)
# - Database: 192.168.1.10:5432 (hanya bisa diakses dari jaringan internal)
# - Tujuan: Akses database dari laptop lokal

ssh -L 5432:192.168.1.10:5432 user@myserver.com

# Sekarang akses database dengan:
# psql -h localhost -p 5432 -U dbuser mydb

# ─────────────────────────────────────────
# USE CASE: Akses web server internal
# ─────────────────────────────────────────

ssh -L 8080:internal-web.example.com:80 user@bastion.example.com
# Buka browser: http://localhost:8080

# ─────────────────────────────────────────
# USE CASE: Akses Redis internal
# ─────────────────────────────────────────

ssh -L 6379:localhost:6379 user@myserver.com
# Sekarang: redis-cli -h localhost -p 6379

# ─────────────────────────────────────────
# Multiple port forwarding sekaligus
# ─────────────────────────────────────────

ssh -L 5432:db.internal:5432 \
    -L 6379:redis.internal:6379 \
    -L 9200:elastic.internal:9200 \
    user@myserver.com

# Background tunnel (tanpa shell)
ssh -fNL 5432:localhost:5432 user@myserver.com
# -f = background
# -N = no command (hanya forward)

# Dengan config file:
# Host db-tunnel
#     HostName myserver.com
#     User user
#     LocalForward 5432 192.168.1.10:5432
```

#### 2. Remote Port Forwarding

```bash
# Syntax:
# ssh -R [remote_addr:]remote_port:local_host:local_port user@ssh_server

# ─────────────────────────────────────────
# USE CASE: Expose local web server ke internet
# ─────────────────────────────────────────

# Scenario:
# - Anda memiliki web server di localhost:3000
# - Ingin akses dari internet melalui ssh_server:8080

ssh -R 8080:localhost:3000 user@ssh_server.com
# Sekarang orang bisa akses: http://ssh_server.com:8080

# ─────────────────────────────────────────
# USE CASE: Remote debugging
# ─────────────────────────────────────────

# Expose debugger lokal ke server remote
ssh -R 9229:localhost:9229 user@remote_server.com

# ─────────────────────────────────────────
# Konfigurasi server untuk Remote Forwarding
# ─────────────────────────────────────────

# Di /etc/ssh/sshd_config:
# GatewayPorts yes  # Izinkan bind ke semua interface
# GatewayPorts clientspecified  # Izinkan client tentukan bind address

# Background remote tunnel
ssh -fNR 8080:localhost:3000 user@ssh_server.com
```

#### 3. Dynamic Port Forwarding (SOCKS Proxy)

```bash
# Syntax:
# ssh -D [bind_addr:]port user@ssh_server

# Buat SOCKS5 proxy di localhost:1080
ssh -D 1080 user@ssh_server.com

# Background
ssh -fND 1080 user@ssh_server.com

# ─────────────────────────────────────────
# Menggunakan SOCKS proxy
# ─────────────────────────────────────────

# Browser: Konfigurasi SOCKS5 proxy ke localhost:1080

# curl melalui SOCKS proxy
curl --socks5 localhost:1080 http://example.com

# wget melalui SOCKS proxy
export http_proxy=socks5://localhost:1080
wget http://example.com

# git melalui SOCKS proxy
git config --global http.proxy socks5://localhost:1080

# ssh melalui SOCKS proxy
ssh -o ProxyCommand="nc -x localhost:1080 %h %p" user@target
```

#### 4. ProxyJump / Jump Host

```bash
# ─────────────────────────────────────────
# Koneksi melalui Jump Host
# ─────────────────────────────────────────

# Syntax: ssh -J jumphost target
ssh -J bastion.example.com user@internal-server.local

# Multiple jump hosts
ssh -J bastion1,bastion2 user@final-server

# Dengan user berbeda di setiap hop
ssh -J jumpuser@bastion.com:22 targetuser@internal.server

# Via ProxyCommand (cara lama)
ssh -o ProxyCommand="ssh -W %h:%p bastion.example.com" user@internal-server

# Config file untuk ProxyJump:
# Host internal
#     HostName 10.0.0.50
#     User internaluser
#     ProxyJump bastion.example.com

# ─────────────────────────────────────────
# Diagram ProxyJump
# ─────────────────────────────────────────
#
#  [Your Machine] ──SSH──► [Bastion Host] ──SSH──► [Internal Server]
#       │                                                │
#       └──────────────── Encrypted ────────────────────┘
```

#### 5. SSH VPN dengan tun Interface

```bash
# ─────────────────────────────────────────
# Setup SSH VPN
# ─────────────────────────────────────────

# Konfigurasi server (sshd_config):
# PermitTunnel yes

# Koneksi dengan tunnel
sudo ssh -w 0:0 user@server
# -w local_tun:remote_tun

# Setup interface di lokal
sudo ip addr add 10.1.1.1/30 dev tun0
sudo ip link set tun0 up

# Setup interface di server
sudo ip addr add 10.1.1.2/30 dev tun0
sudo ip link set tun0 up

# Routing
sudo ip route add 192.168.2.0/24 via 10.1.1.2
```

***

### 11. SCP & SFTP <a href="#scp-sftp" id="scp-sftp"></a>

#### SCP (Secure Copy Protocol) - Detail

```bash
# ─────────────────────────────────────────
# SCP CHEAT SHEET
# ─────────────────────────────────────────

# Upload file
scp localfile.txt user@server:/path/to/destination/

# Upload dengan rename
scp localfile.txt user@server:/path/to/destination/newname.txt

# Upload direktori
scp -r /local/dir/ user@server:/remote/dir/

# Download file
scp user@server:/path/to/file.txt /local/destination/

# Download direktori
scp -r user@server:/remote/dir/ /local/dir/

# Download ke direktori sekarang
scp user@server:/path/to/file.txt .

# Copy antar dua server
scp user1@server1:/path/file user2@server2:/path/

# Opsi-opsi SCP
scp -v    # Verbose
scp -q    # Quiet
scp -C    # Compress
scp -P    # Port
scp -p    # Preserve timestamps
scp -r    # Recursive
scp -l    # Limit bandwidth (Kbps)
scp -i    # Identity file
scp -4    # Force IPv4
scp -6    # Force IPv6

# Contoh lengkap
scp -v -C -P 2222 -i ~/.ssh/mykey -r /local/dir/ user@server:/remote/dir/
```

#### SFTP (SSH File Transfer Protocol)

```bash
# ─────────────────────────────────────────
# SFTP Interactive Mode
# ─────────────────────────────────────────

# Buka sesi SFTP
sftp user@hostname
sftp -P 2222 user@hostname
sftp -i ~/.ssh/mykey user@hostname

# SFTP Commands (di dalam shell SFTP):
help                    # Tampilkan help
pwd                     # Print working directory (remote)
lpwd                    # Print working directory (local)
ls                      # List remote files
ls -la                  # List detail
lls                     # List local files
cd /remote/path         # Change remote directory
lcd /local/path         # Change local directory
mkdir remotedir         # Buat direktori remote
lmkdir localdir         # Buat direktori local
rmdir remotedir         # Hapus direktori remote
rm remotefile           # Hapus file remote
rename old new          # Rename remote file
chmod 644 remotefile    # Ubah permission remote file

# Upload (put)
put localfile.txt               # Upload ke current remote dir
put localfile.txt remotefile    # Upload dengan rename
put -r /local/dir/              # Upload direktori rekursif
mput *.txt                      # Upload multiple files

# Download (get)
get remotefile.txt              # Download ke current local dir
get remotefile.txt localfile    # Download dengan rename
get -r remotedir/               # Download direktori rekursif
mget *.txt                      # Download multiple files

# Transfer dengan progress
put -a largefile.tar.gz         # Resume transfer

# Exit
quit
exit
bye

# ─────────────────────────────────────────
# SFTP Batch Mode (Non-Interactive)
# ─────────────────────────────────────────

# Menggunakan batch file
cat > sftp_commands.txt << 'EOF'
cd /remote/path
put localfile.txt
get remotefile.txt
ls
quit
EOF

sftp -b sftp_commands.txt user@hostname

# Inline batch commands
sftp -b - user@hostname << 'EOF'
put file1.txt
put file2.txt
quit
EOF

# ─────────────────────────────────────────
# Setup Chrooted SFTP
# ─────────────────────────────────────────

# Di /etc/ssh/sshd_config:
# Subsystem sftp internal-sftp
#
# Match Group sftpusers
#     ChrootDirectory /home/%u
#     ForceCommand internal-sftp
#     AllowTcpForwarding no
#     X11Forwarding no

# Buat user SFTP
sudo useradd -m -G sftpusers -s /bin/false sftpuser
sudo passwd sftpuser

# Setup direktori chroot
sudo chown root:root /home/sftpuser  # WAJIB dimiliki root
sudo chmod 755 /home/sftpuser

# Buat direktori upload
sudo mkdir /home/sftpuser/uploads
sudo chown sftpuser:sftpusers /home/sftpuser/uploads
```

#### rsync over SSH

```bash
# rsync lebih efisien dari SCP untuk sync direktori
# Hanya transfer file yang berubah

# Sync lokal ke remote
rsync -avz /local/dir/ user@server:/remote/dir/

# Sync remote ke lokal
rsync -avz user@server:/remote/dir/ /local/dir/

# Opsi rsync
rsync -a    # Archive (preserve permissions, timestamps, dll)
rsync -v    # Verbose
rsync -z    # Compress
rsync -P    # Progress + partial (bisa resume)
rsync -n    # Dry run (simulasi)
rsync -e    # Specify SSH command
rsync --delete     # Hapus file di dest yang tidak ada di source
rsync --exclude    # Exclude pattern

# Dengan SSH options
rsync -avz -e "ssh -p 2222 -i ~/.ssh/mykey" \
  /local/dir/ user@server:/remote/dir/

# Exclude directory
rsync -avz --exclude='.git' --exclude='node_modules' \
  /local/project/ user@server:/remote/project/

# Dry run untuk cek apa yang akan diubah
rsync -avzn /local/dir/ user@server:/remote/dir/

# Backup dengan delete (mirror)
rsync -avz --delete /local/dir/ user@server:/remote/backup/

# Progress bar
rsync -avz --progress /local/dir/ user@server:/remote/dir/
```

***

### 12. SSH Agent <a href="#ssh-agent" id="ssh-agent"></a>

#### Apa itu SSH Agent?

```
┌──────────────────────────────────────────────────────────────┐
│                     SSH Agent                                │
│                                                              │
│  SSH Agent adalah program yang menyimpan private key        │
│  dalam memori (ter-decrypt) sehingga Anda tidak perlu       │
│  memasukkan passphrase berulang kali.                        │
│                                                              │
│  [Private Key + Passphrase] → [SSH Agent] → [Proses SSH]    │
│                                   ↑                          │
│              Hanya masukkan passphrase SEKALI               │
└──────────────────────────────────────────────────────────────┘
```

#### Penggunaan SSH Agent

```bash
# ─────────────────────────────────────────
# Start SSH Agent
# ─────────────────────────────────────────

# Start agent dan set environment variables
eval $(ssh-agent)
# atau
eval "$(ssh-agent -s)"

# Output:
# Agent pid 12345

# Check apakah agent berjalan
echo $SSH_AUTH_SOCK
echo $SSH_AGENT_PID

# ─────────────────────────────────────────
# Mengelola Keys di Agent
# ─────────────────────────────────────────

# Tambahkan default key (~/.ssh/id_*)
ssh-add

# Tambahkan key tertentu
ssh-add ~/.ssh/id_ed25519
ssh-add ~/.ssh/myserver_key

# Tambahkan dengan timeout (otomatis dihapus setelah N detik)
ssh-add -t 3600 ~/.ssh/id_ed25519  # 1 jam

# List semua keys di agent
ssh-add -l
# Output: 256 SHA256:ABC...xyz user@machine (ED25519)

# List dengan public key lengkap
ssh-add -L

# Hapus key dari agent
ssh-add -d ~/.ssh/id_ed25519

# Hapus semua keys dari agent
ssh-add -D

# Lock agent dengan password
ssh-add -x

# Unlock agent
ssh-add -X

# ─────────────────────────────────────────
# Agent Forwarding
# ─────────────────────────────────────────

# Koneksi dengan agent forwarding
ssh -A user@hostname

# Atau di config:
# Host myserver
#     ForwardAgent yes

# ⚠️ PERHATIAN: Agent forwarding berbahaya!
# Jika server dikompromikan, attacker bisa gunakan agent Anda
# Hanya gunakan ke server yang dipercaya

# ─────────────────────────────────────────
# Auto-start SSH Agent (di ~/.bashrc atau ~/.zshrc)
# ─────────────────────────────────────────

# Tambahkan ke shell profile:
cat >> ~/.bashrc << 'EOF'
# SSH Agent Auto-start
if [ -z "$SSH_AUTH_SOCK" ]; then
    # Check untuk running agent
    export SSH_AUTH_SOCK="$HOME/.ssh/agent.sock"
    
    if ! ssh-add -l > /dev/null 2>&1; then
        rm -f "$SSH_AUTH_SOCK"
        eval $(ssh-agent -a "$SSH_AUTH_SOCK") > /dev/null
        ssh-add ~/.ssh/id_ed25519 2>/dev/null
    fi
fi
EOF

# ─────────────────────────────────────────
# Keychain (Persistent Agent)
# ─────────────────────────────────────────

# Install keychain
sudo apt install keychain

# Gunakan keychain
eval $(keychain --eval --agents ssh id_ed25519)

# Di .bashrc
echo 'eval $(keychain --eval --quiet ~/.ssh/id_ed25519)' >> ~/.bashrc
```

***

### 13. Keamanan SSH <a href="#keamanan" id="keamanan"></a>

#### Hardening SSH Server

```bash
# ─────────────────────────────────────────
# /etc/ssh/sshd_config - SECURE CONFIGURATION
# ─────────────────────────────────────────

# 1. Ubah port default
Port 2222  # Bukan solusi utama, hanya mengurangi noise

# 2. Nonaktifkan root login
PermitRootLogin no
# atau jika perlu root, hanya izinkan key auth:
# PermitRootLogin prohibit-password

# 3. Nonaktifkan password authentication
PasswordAuthentication no
PermitEmptyPasswords no
ChallengeResponseAuthentication no

# 4. Hanya izinkan key-based auth
PubkeyAuthentication yes
AuthenticationMethods publickey

# 5. Batasi user yang bisa SSH
AllowUsers alice bob charlie

# 6. Batasi percobaan login
MaxAuthTries 3
LoginGraceTime 20

# 7. Nonaktifkan fitur yang tidak diperlukan
X11Forwarding no
AllowTcpForwarding no  # Jika tidak perlu tunneling
AllowAgentForwarding no

# 8. Timeout idle sessions
ClientAliveInterval 300
ClientAliveCountMax 2

# 9. Gunakan protokol SSH yang aman
Protocol 2

# 10. Kriptografi yang kuat
KexAlgorithms curve25519-sha256,diffie-hellman-group16-sha512
Ciphers chacha20-poly1305@openssh.com,aes256-gcm@openssh.com
MACs hmac-sha2-512-etm@openssh.com,hmac-sha2-256-etm@openssh.com

# 11. Log level yang informatif
LogLevel VERBOSE

# 12. Disable DNS lookup (mempercepat koneksi)
UseDNS no
```

#### Firewall Configuration

```bash
# ─────────────────────────────────────────
# UFW (Ubuntu Firewall)
# ─────────────────────────────────────────

# Allow SSH default port
sudo ufw allow 22/tcp

# Allow SSH custom port
sudo ufw allow 2222/tcp

# Allow SSH dari IP tertentu saja
sudo ufw allow from 192.168.1.0/24 to any port 22

# Hapus rule
sudo ufw delete allow 22/tcp

# Enable UFW
sudo ufw enable

# Status UFW
sudo ufw status verbose

# ─────────────────────────────────────────
# iptables
# ─────────────────────────────────────────

# Allow SSH dari semua
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT

# Allow SSH dari IP tertentu
sudo iptables -A INPUT -s 192.168.1.0/24 -p tcp --dport 22 -j ACCEPT

# Rate limiting untuk mencegah brute force
sudo iptables -A INPUT -p tcp --dport 22 -m state --state NEW \
  -m recent --set --name SSH

sudo iptables -A INPUT -p tcp --dport 22 -m state --state NEW \
  -m recent --update --seconds 60 --hitcount 4 \
  --name SSH -j DROP

# Simpan iptables rules
sudo iptables-save > /etc/iptables/rules.v4

# ─────────────────────────────────────────
# firewalld (CentOS/RHEL)
# ─────────────────────────────────────────

# Allow SSH
sudo firewall-cmd --permanent --add-service=ssh

# Custom port
sudo firewall-cmd --permanent --add-port=2222/tcp

# Remove default SSH port
sudo firewall-cmd --permanent --remove-service=ssh

# Reload
sudo firewall-cmd --reload

# Status
sudo firewall-cmd --list-all
```

#### Fail2Ban - Proteksi Brute Force

```bash
# ─────────────────────────────────────────
# Install & Konfigurasi Fail2Ban
# ─────────────────────────────────────────

# Install
sudo apt install fail2ban -y

# Buat konfigurasi lokal
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
sudo nano /etc/fail2ban/jail.local

# Konfigurasi jail SSH:
# [sshd]
# enabled = true
# port = ssh
# filter = sshd
# logpath = /var/log/auth.log
# maxretry = 3
# bantime = 3600
# findtime = 600
# ignoreip = 127.0.0.1/8 192.168.1.0/24

# Start fail2ban
sudo systemctl start fail2ban
sudo systemctl enable fail2ban

# Cek status
sudo fail2ban-client status
sudo fail2ban-client status sshd

# Lihat banned IPs
sudo fail2ban-client get sshd banned

# Unban IP
sudo fail2ban-client set sshd unbanip 1.2.3.4

# Cek log fail2ban
sudo tail -f /var/log/fail2ban.log
```

#### Two-Factor Authentication (2FA)

```bash
# ─────────────────────────────────────────
# Setup Google Authenticator untuk SSH
# ─────────────────────────────────────────

# Install
sudo apt install libpam-google-authenticator

# Setup untuk user saat ini
google-authenticator
# Pilih:
# - Time-based (y)
# - Scan QR code dengan Google Authenticator app
# - Update .google_authenticator file (y)
# - Disallow multiple use (y)
# - 30 second window (y)
# - Rate limiting (y)

# Konfigurasi PAM
sudo nano /etc/pam.d/sshd
# Tambahkan di baris PERTAMA:
# auth required pam_google_authenticator.so

# Untuk menggunakan key + 2FA:
# auth required pam_google_authenticator.so
# (dan hapus atau comment: @include common-auth)

# Konfigurasi sshd_config
sudo nano /etc/ssh/sshd_config
# ChallengeResponseAuthentication yes
# AuthenticationMethods publickey,keyboard-interactive
# # (publickey DAN 2FA diperlukan)
# # Atau hanya 2FA:
# # AuthenticationMethods keyboard-interactive

# Restart SSH
sudo systemctl restart sshd
```

#### SSH Certificate Authority (CA)

```bash
# ─────────────────────────────────────────
# Setup SSH CA untuk Enterprise
# ─────────────────────────────────────────

# 1. Generate CA key pair (simpan dengan SANGAT AMAN)
ssh-keygen -t ed25519 -f /etc/ssh/ca -C "Company SSH CA"

# 2. Konfigurasi server untuk trust CA
echo "TrustedUserCAKeys /etc/ssh/ca.pub" | sudo tee -a /etc/ssh/sshd_config

# 3. Sign user public key
ssh-keygen -s /etc/ssh/ca \
  -I "alice@company.com" \
  -n "alice,admin" \
  -V "+52w" \
  ~/.ssh/alice_id_ed25519.pub

# Opsi signing:
# -s = CA signing key
# -I = Certificate identity (audit trail)
# -n = Principals (comma-separated usernames)
# -V = Validity period
# -O = Options (force-command=cmd, no-pty, dll)

# 4. User koneksi dengan certificate
ssh -i ~/.ssh/alice_id_ed25519 user@server
# SSH otomatis menggunakan certificate .pub + cert file

# 5. Revoke certificate (OCSP/KRL)
# Buat Key Revocation List
ssh-keygen -k -f /etc/ssh/revoked_keys
# Tambahkan key yang direvoke:
ssh-keygen -k -f /etc/ssh/revoked_keys -u alice_id_ed25519.pub

# Konfigurasi server
echo "RevokedKeys /etc/ssh/revoked_keys" | sudo tee -a /etc/ssh/sshd_config
```

#### Monitoring & Audit SSH

```bash
# ─────────────────────────────────────────
# Monitoring Login SSH
# ─────────────────────────────────────────

# Lihat login berhasil
sudo grep "Accepted" /var/log/auth.log
sudo grep "Accepted publickey" /var/log/auth.log
sudo grep "Accepted password" /var/log/auth.log

# Lihat login gagal
sudo grep "Failed" /var/log/auth.log
sudo grep "Invalid user" /var/log/auth.log

# Lihat semua aktivitas SSH
sudo grep "sshd" /var/log/auth.log | tail -50

# Siapa yang sedang login
who
w
last
lastb  # Login yang gagal

# Lihat koneksi SSH aktif
sudo ss -tnp | grep :22

# Audit dengan ausearch (jika auditd aktif)
sudo ausearch -m USER_LOGIN -ts today
sudo ausearch -m USER_AUTH --success no

# Real-time monitoring
sudo tail -f /var/log/auth.log | grep sshd

# Script monitoring sederhana
#!/bin/bash
# ssh_monitor.sh
watch -n 5 'echo "=== Active SSH Connections ===" && \
  ss -tnp | grep :22 && \
  echo "" && \
  echo "=== Recent Logins ===" && \
  last | head -20'
```

***

### 14. Troubleshooting <a href="#troubleshooting" id="troubleshooting"></a>

#### Debugging Koneksi

```bash
# ─────────────────────────────────────────
# Client-side Debugging
# ─────────────────────────────────────────

# Level 1: Basic info
ssh -v user@hostname

# Level 2: Detail lebih
ssh -vv user@hostname

# Level 3: Debug lengkap
ssh -vvv user@hostname

# Contoh output debugging:
# debug1: Connecting to hostname [1.2.3.4] port 22.
# debug1: Connection established.
# debug1: identity file /home/user/.ssh/id_ed25519 type 3
# debug1: Remote protocol version 2.0, remote software version OpenSSH_8.9
# debug1: match: OpenSSH_8.9 pat OpenSSH* compat 0x04000000
# debug1: Authenticating to hostname:22 as 'user'
# debug1: SSH2_MSG_KEXINIT sent
# debug1: SSH2_MSG_KEXINIT received
# debug1: kex: algorithm: curve25519-sha256
# debug1: kex: host key algorithm: ssh-ed25519
# debug1: kex: server->client cipher: chacha20-poly1305@openssh.com
# ...

# ─────────────────────────────────────────
# Server-side Debugging
# ─────────────────────────────────────────

# Cek status SSH service
sudo systemctl status sshd

# Cek log SSH server
sudo journalctl -u sshd -f
sudo tail -f /var/log/auth.log
sudo tail -f /var/log/secure  # CentOS/RHEL

# Test konfigurasi sshd
sudo sshd -t
sudo sshd -T

# Jalankan sshd dalam mode debug (port berbeda)
sudo /usr/sbin/sshd -d -p 2223
```

#### Masalah Umum & Solusinya

```bash
# ─────────────────────────────────────────
# MASALAH 1: Connection refused
# ─────────────────────────────────────────
# Error: ssh: connect to host hostname port 22: Connection refused

# Cek apakah SSH service berjalan
sudo systemctl status sshd
sudo systemctl start sshd

# Cek port yang digunakan SSH
sudo ss -tlnp | grep sshd

# Cek firewall
sudo ufw status
sudo iptables -L -n | grep 22

# Test konektivitas
nc -zv hostname 22
telnet hostname 22

# ─────────────────────────────────────────
# MASALAH 2: Permission denied (publickey)
# ─────────────────────────────────────────
# Error: Permission denied (publickey)

# Debug untuk melihat key mana yang dicoba
ssh -vv user@hostname 2>&1 | grep -i "key\|auth\|offer"

# Cek permission file
ls -la ~/.ssh/
ls -la ~/.ssh/authorized_keys

# Fix permission
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
chmod 600 ~/.ssh/id_ed25519
chown -R $USER:$USER ~/.ssh

# Pastikan public key ada di server
cat ~/.ssh/id_ed25519.pub
ssh user@hostname "cat ~/.ssh/authorized_keys"

# Cek log server
sudo grep "Authentication" /var/log/auth.log

# ─────────────────────────────────────────
# MASALAH 3: Host key verification failed
# ─────────────────────────────────────────
# Error: WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!

# Hapus old host key
ssh-keygen -R hostname
ssh-keygen -R ip_address

# Atau edit known_hosts manual
nano ~/.ssh/known_hosts
# Hapus baris yang berisi hostname/IP tersebut

# Koneksi ulang (akan diminta konfirmasi)
ssh user@hostname

# ─────────────────────────────────────────
# MASALAH 4: Connection timeout
# ─────────────────────────────────────────

# Test apakah host reachable
ping hostname
traceroute hostname

# Cek koneksi ke port 22
nc -zv -w 5 hostname 22

# Koneksi dengan timeout eksplisit
ssh -o ConnectTimeout=10 user@hostname

# Cek firewall di kedua sisi
sudo iptables -L -n

# ─────────────────────────────────────────
# MASALAH 5: Too many authentication failures
# ─────────────────────────────────────────
# Error: Received disconnect from host: Too many authentication failures

# Masalah: Agent menawarkan terlalu banyak keys

# Solusi 1: Tentukan identity file secara eksplisit
ssh -i ~/.ssh/specific_key user@hostname

# Solusi 2: Batasi keys yang ditawarkan
ssh -o IdentitiesOnly=yes -i ~/.ssh/specific_key user@hostname

# Solusi 3: Di config
# Host problematic-server
#     IdentityFile ~/.ssh/specific_key
#     IdentitiesOnly yes

# ─────────────────────────────────────────
# MASALAH 6: SSH sangat lambat
# ─────────────────────────────────────────

# Masalah: DNS lookup lambat
# Solusi: Di sshd_config
# UseDNS no

# Masalah: GSSAPI auth lambat
# Solusi: Di ssh_config atau command
ssh -o GSSAPIAuthentication=no user@hostname

# Masalah: Koneksi baru untuk setiap session
# Solusi: Connection multiplexing
# Di ~/.ssh/config:
# Host *
#     ControlMaster auto
#     ControlPath ~/.ssh/control/%r@%h:%p
#     ControlPersist 10m

# ─────────────────────────────────────────
# MASALAH 7: Broken pipe
# ─────────────────────────────────────────
# Error: Write failed: Broken pipe
# atau: packet_write_wait: Connection to host: Broken pipe

# Solusi 1: Di client ~/.ssh/config
# Host *
#     ServerAliveInterval 60
#     ServerAliveCountMax 3

# Solusi 2: Di server sshd_config
# ClientAliveInterval 300
# ClientAliveCountMax 3

# Solusi 3: Command line
ssh -o ServerAliveInterval=60 user@hostname

# ─────────────────────────────────────────
# MASALAH 8: Cannot allocate pseudoterminal
# ─────────────────────────────────────────

# Solusi: Gunakan -t flag
ssh -t user@hostname "sudo command"

# Atau force TTY
ssh -tt user@hostname "interactive_command"

# ─────────────────────────────────────────
# MASALAH 9: Key exchange algorithm mismatch
# ─────────────────────────────────────────
# Error: Unable to negotiate with host: no matching key exchange method

# Cek apa yang ditawarkan server
ssh -vv user@hostname 2>&1 | grep "kex:"

# Override algoritma (untuk server lama)
ssh -o KexAlgorithms=+diffie-hellman-group1-sha1 user@old-server
ssh -o KexAlgorithms=+diffie-hellman-group14-sha1 user@old-server

# ─────────────────────────────────────────
# MASALAH 10: sshd: no hostkeys available
# ─────────────────────────────────────────

# Generate host keys
sudo ssh-keygen -A

# Cek host keys ada
ls -la /etc/ssh/ssh_host_*

# ─────────────────────────────────────────
# DIAGNOSTIC SCRIPT
# ─────────────────────────────────────────

#!/bin/bash
# ssh_diagnostics.sh

TARGET_HOST="${1:-example.com}"
TARGET_PORT="${2:-22}"

echo "=== SSH Diagnostics for $TARGET_HOST:$TARGET_PORT ==="
echo ""

echo "1. Ping test:"
ping -c 3 "$TARGET_HOST" 2>/dev/null && echo "✅ Host reachable" || echo "❌ Host unreachable"

echo ""
echo "2. Port test:"
nc -zv -w 5 "$TARGET_HOST" "$TARGET_PORT" 2>&1 && echo "✅ Port open" || echo "❌ Port closed"

echo ""
echo "3. SSH version:"
ssh -V 2>&1

echo ""
echo "4. SSH key check:"
ls -la ~/.ssh/*.pub 2>/dev/null || echo "No public keys found"

echo ""
echo "5. SSH agent check:"
ssh-add -l 2>/dev/null || echo "No agent or no keys in agent"

echo ""
echo "6. Attempting connection (verbose):"
ssh -vvv -o ConnectTimeout=10 -o BatchMode=yes \
  "$TARGET_HOST" -p "$TARGET_PORT" echo "Connected" 2>&1 | \
  grep -E "debug1:|error:|warning:|Permission|Authen|Connecting"
```

***

### 15. Use Cases Lanjutan <a href="#use-cases" id="use-cases"></a>

#### Automated Deployment

```bash
# ─────────────────────────────────────────
# Deploy Script via SSH
# ─────────────────────────────────────────

#!/bin/bash
# deploy.sh

REMOTE_HOST="deploy@production.example.com"
REMOTE_PATH="/var/www/myapp"
LOCAL_PATH="./dist"

echo "🚀 Starting deployment..."

# 1. Upload files
rsync -avz --delete \
  --exclude='.git' \
  --exclude='node_modules' \
  "$LOCAL_PATH/" "$REMOTE_HOST:$REMOTE_PATH/"

# 2. Run remote commands
ssh "$REMOTE_HOST" << 'ENDSSH'
    set -e  # Exit on error
    cd /var/www/myapp
    
    echo "📦 Installing dependencies..."
    npm install --production
    
    echo "🔄 Running migrations..."
    npm run migrate
    
    echo "♻️  Restarting application..."
    pm2 restart myapp
    
    echo "✅ Deployment complete!"
ENDSSH

echo "🎉 Deployment successful!"

# ─────────────────────────────────────────
# Parallel SSH Deployment ke Multiple Servers
# ─────────────────────────────────────────

#!/bin/bash
# parallel_deploy.sh

SERVERS=("web1.example.com" "web2.example.com" "web3.example.com")
DEPLOY_USER="deploy"
DEPLOY_CMD="cd /app && git pull && systemctl restart myapp"

echo "Deploying to ${#SERVERS[@]} servers..."

for server in "${SERVERS[@]}"; do
    ssh "$DEPLOY_USER@$server" "$DEPLOY_CMD" &
done

wait
echo "All deployments complete!"

# Atau gunakan pssh (parallel-ssh)
# sudo apt install pssh
# pssh -H "web1 web2 web3" -l deploy "$DEPLOY_CMD"
```

#### SSH Bastion Host Pattern

```bash
# ─────────────────────────────────────────
# Enterprise Bastion Host Setup
# ─────────────────────────────────────────

# Topologi:
# Internet → Bastion Host → Internal Network

# Di Bastion Host:
# /etc/ssh/sshd_config
AllowAgentForwarding yes
AllowTcpForwarding yes
GatewayPorts no
PermitTunnel no

# User hanya bisa forward, tidak bisa exec
# Match Group bastion-users
#     ForceCommand echo "Bastion access only - port forwarding enabled"
#     AllowAgentForwarding yes

# Client config untuk akses melalui bastion
# ~/.ssh/config
Host bastion
    HostName bastion.example.com
    User jumpuser
    IdentityFile ~/.ssh/bastion_key
    ServerAliveInterval 60

Host internal-*
    User admin
    IdentityFile ~/.ssh/internal_key
    ProxyJump bastion
    
Host internal-web
    HostName 10.0.1.10
    
Host internal-db
    HostName 10.0.2.10
    
Host internal-redis
    HostName 10.0.3.10

# Koneksi:
ssh internal-web    # Otomatis melalui bastion
```

#### SSH untuk Backup Automation

```bash
# ─────────────────────────────────────────
# Automated Backup via SSH
# ─────────────────────────────────────────

#!/bin/bash
# backup.sh

BACKUP_HOST="backup@backup-server.example.com"
BACKUP_PATH="/backups/$(hostname)"
SOURCE_DIRS=("/etc" "/home" "/var/www")
DATE=$(date +%Y%m%d_%H%M%S)
BACKUP_NAME="backup_$DATE"

echo "Starting backup: $BACKUP_NAME"

# Buat direktori backup di remote
ssh "$BACKUP_HOST" "mkdir -p $BACKUP_PATH"

# Backup setiap direktori
for dir in "${SOURCE_DIRS[@]}"; do
    dir_name=$(echo "$dir" | tr '/' '_')
    
    echo "Backing up: $dir"
    rsync -az --delete \
        --link-dest="$BACKUP_PATH/latest${dir_name}" \
        "$dir/" \
        "$BACKUP_HOST:$BACKUP_PATH/$BACKUP_NAME${dir_name}/"
done

# Update symlink ke backup terbaru
ssh "$BACKUP_HOST" << ENDSSH
    cd $BACKUP_PATH
    rm -f latest
    ln -s $BACKUP_NAME latest
    echo "Current backups:"
    ls -la
    echo ""
    echo "Disk usage:"
    du -sh */
ENDSSH

echo "Backup complete: $BACKUP_NAME"

# ─────────────────────────────────────────
# Database Backup via SSH Tunnel
# ─────────────────────────────────────────

#!/bin/bash
# db_backup.sh

SSH_HOST="user@db-server.example.com"
DB_NAME="myapp_production"
BACKUP_FILE="db_backup_$(date +%Y%m%d).sql.gz"

echo "Starting database backup..."

# Method 1: Pipe dump melalui SSH
ssh "$SSH_HOST" "mysqldump -u root $DB_NAME | gzip" > "$BACKUP_FILE"

# Method 2: Via tunnel
ssh -fNL 3306:localhost:3306 "$SSH_HOST"
mysqldump -h 127.0.0.1 -P 3306 -u root "$DB_NAME" | \
  gzip > "$BACKUP_FILE"

echo "Backup saved: $BACKUP_FILE"
```

#### SSH untuk Monitoring

```bash
# ─────────────────────────────────────────
# Remote Monitoring via SSH
# ─────────────────────────────────────────

#!/bin/bash
# monitor.sh

SERVERS=("web1.example.com" "web2.example.com" "db.example.com")
ALERT_EMAIL="admin@example.com"

check_server() {
    local server=$1
    
    # Cek konektivitas dan ambil metrics
    metrics=$(ssh -o ConnectTimeout=5 -o BatchMode=yes \
        "monitor@$server" << 'EOF' 2>/dev/null
            cpu=$(top -bn1 | grep "Cpu(s)" | awk '{print $2}')
            mem=$(free | grep Mem | awk '{printf "%.1f", $3/$2*100}')
            disk=$(df -h / | awk 'NR==2 {print $5}')
            load=$(uptime | awk '{print $NF}')
            echo "CPU:$cpu MEM:$mem DISK:$disk LOAD:$load"
EOF
    )
    
    if [ $? -eq 0 ]; then
        echo "✅ $server | $metrics"
    else
        echo "❌ $server | UNREACHABLE"
        echo "Server $server is unreachable!" | \
            mail -s "Alert: SSH Monitor" "$ALERT_EMAIL"
    fi
}

echo "=== Server Health Check: $(date) ==="
for server in "${SERVERS[@]}"; do
    check_server "$server"
done
```

#### SSH untuk CI/CD Pipeline

```bash
# ─────────────────────────────────────────
# GitHub Actions dengan SSH
# ─────────────────────────────────────────

# .github/workflows/deploy.yml
name: Deploy to Production

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - name: Setup SSH
        run: |
          mkdir -p ~/.ssh
          echo "${{ secrets.SSH_PRIVATE_KEY }}" > ~/.ssh/id_ed25519
          chmod 600 ~/.ssh/id_ed25519
          echo "${{ secrets.SSH_KNOWN_HOSTS }}" > ~/.ssh/known_hosts
      
      - name: Deploy
        run: |
          ssh deploy@${{ secrets.PRODUCTION_HOST }} << 'ENDSSH'
            cd /var/www/myapp
            git pull origin main
            npm ci --production
            pm2 restart myapp
          ENDSSH

# ─────────────────────────────────────────
# GitLab CI dengan SSH
# ─────────────────────────────────────────

# .gitlab-ci.yml
stages:
  - deploy

deploy_production:
  stage: deploy
  script:
    - eval $(ssh-agent -s)
    - echo "$SSH_PRIVATE_KEY" | tr -d '\r' | ssh-add -
    - mkdir -p ~/.ssh
    - chmod 700 ~/.ssh
    - echo "$SSH_KNOWN_HOSTS" >> ~/.ssh/known_hosts
    - chmod 644 ~/.ssh/known_hosts
    - ssh deploy@production.example.com "cd /app && ./deploy.sh"
  only:
    - main
  environment:
    name: production
```

***

### 16. Best Practices <a href="#best-practices" id="best-practices"></a>

#### Checklist Keamanan SSH

```
╔════════════════════════════════════════════════════════════════╗
║              SSH SECURITY CHECKLIST                           ║
╠════════════════════════════════════════════════════════════════╣
║                                                               ║
║  SERVER CONFIGURATION                                         ║
║  ─────────────────────────────────────────────────────────   ║
║  ☐ Gunakan OpenSSH versi terbaru                             ║
║  ☐ Nonaktifkan root login (PermitRootLogin no)               ║
║  ☐ Nonaktifkan password auth (PasswordAuthentication no)      ║
║  ☐ Gunakan hanya SSH-2 (Protocol 2)                          ║
║  ☐ Konfigurasi algoritma kriptografi yang kuat               ║
║  ☐ Set MaxAuthTries 3                                         ║
║  ☐ Set LoginGraceTime 20-30 detik                            ║
║  ☐ Batasi akses user (AllowUsers/AllowGroups)                 ║
║  ☐ Enable logging (LogLevel VERBOSE)                          ║
║  ☐ Set ClientAliveInterval dan ClientAliveCountMax            ║
║                                                               ║
║  KEY MANAGEMENT                                               ║
║  ─────────────────────────────────────────────────────────   ║
║  ☐ Gunakan Ed25519 atau RSA-4096                             ║
║  ☐ Selalu gunakan passphrase pada private key                ║
║  ☐ Jaga keamanan private key (chmod 600)                     ║
║  ☐ Rotasi keys secara berkala                                 ║
║  ☐ Hapus keys yang tidak digunakan                            ║
║  ☐ Gunakan separate keys untuk setiap server/tujuan           ║
║                                                               ║
║  NETWORK SECURITY                                             ║
║  ─────────────────────────────────────────────────────────   ║
║  ☐ Konfigurasi firewall (whitelist IP jika memungkinkan)      ║
║  ☐ Pertimbangkan port non-standard                            ║
║  ☐ Install dan konfigurasi fail2ban                           ║
║  ☐ Monitor log SSH secara regular                             ║
║  ☐ Setup alerting untuk login yang mencurigakan               ║
║                                                               ║
║  ADVANCED SECURITY                                            ║
║  ─────────────────────────────────────────────────────────   ║
║  ☐ Pertimbangkan 2FA/MFA                                      ║
║  ☐ Gunakan SSH certificates untuk enterprise                  ║
║  ☐ Implementasikan bastion host pattern                        ║
║  ☐ Audit akses SSH secara regular                             ║
║  ☐ Update dan patch SSH server secara rutin                   ║
║                                                               ║
╚════════════════════════════════════════════════════════════════╝
```

#### Script Hardening Otomatis

```bash
#!/bin/bash
# ssh_hardening.sh - Script hardening SSH server

set -euo pipefail

echo "🔐 SSH Server Hardening Script"
echo "================================"

SSHD_CONFIG="/etc/ssh/sshd_config"
BACKUP_FILE="/etc/ssh/sshd_config.backup.$(date +%Y%m%d_%H%M%S)"

# Backup
echo "📋 Membuat backup konfigurasi..."
sudo cp "$SSHD_CONFIG" "$BACKUP_FILE"
echo "Backup: $BACKUP_FILE"

# Fungsi untuk set/replace konfigurasi
set_config() {
    local key=$1
    local value=$2
    
    if grep -q "^#*\s*${key}\s" "$SSHD_CONFIG"; then
        sudo sed -i "s/^#*\s*${key}\s.*/${key} ${value}/" "$SSHD_CONFIG"
    else
        echo "${key} ${value}" | sudo tee -a "$SSHD_CONFIG"
    fi
    echo "  ✅ ${key} = ${value}"
}

echo ""
echo "🔧 Mengaplikasikan konfigurasi keamanan..."

# Nonaktifkan root login
set_config "PermitRootLogin" "no"

# Nonaktifkan password auth
set_config "PasswordAuthentication" "no"

# Nonaktifkan empty password
set_config "PermitEmptyPasswords" "no"

# Hanya SSH-2
# set_config "Protocol" "2"  # Deprecated tapi untuk info

# Batasi auth tries
set_config "MaxAuthTries" "3"

# Login grace time
set_config "LoginGraceTime" "20"

# Enable public key auth
set_config "PubkeyAuthentication" "yes"

# Idle timeout
set_config "ClientAliveInterval" "300"
set_config "ClientAliveCountMax" "2"

# Logging
set_config "LogLevel" "VERBOSE"

# Nonaktifkan DNS lookup
set_config "UseDNS" "no"

# Nonaktifkan X11 forwarding
set_config "X11Forwarding" "no"

# Kriptografi kuat
set_config "KexAlgorithms" "curve25519-sha256,curve25519-sha256@libssh.org,diffie-hellman-group16-sha512,diffie-hellman-group18-sha512"
set_config "Ciphers" "chacha20-poly1305@openssh.com,aes256-gcm@openssh.com,aes128-gcm@openssh.com"
set_config "MACs" "hmac-sha2-512-etm@openssh.com,hmac-sha2-256-etm@openssh.com,umac-128-etm@openssh.com"

# Test konfigurasi
echo ""
echo "🧪 Testing konfigurasi..."
if sudo sshd -t; then
    echo "✅ Konfigurasi valid!"
    
    # Reload SSH
    echo ""
    echo "🔄 Mereload SSH service..."
    sudo systemctl reload sshd
    echo "✅ SSH berhasil di-reload!"
else
    echo "❌ Konfigurasi ERROR! Mengembalikan backup..."
    sudo cp "$BACKUP_FILE" "$SSHD_CONFIG"
    echo "✅ Backup dikembalikan"
    exit 1
fi

echo ""
echo "================================"
echo "✅ SSH Hardening selesai!"
echo ""
echo "⚠️  PENTING:"
echo "   - Pastikan Anda sudah setup SSH key SEBELUM logout"
echo "   - Test koneksi di sesi baru sebelum menutup yang sekarang"
echo "   - Backup tersimpan di: $BACKUP_FILE"
```

#### SSH Key Rotation Script

```bash
#!/bin/bash
# ssh_key_rotation.sh

SERVERS=("server1.example.com" "server2.example.com")
REMOTE_USER="admin"
KEY_DIR="$HOME/.ssh/keys"
OLD_KEY="$KEY_DIR/id_ed25519"
NEW_KEY="$KEY_DIR/id_ed25519_new"
DATE=$(date +%Y%m%d)

echo "🔑 SSH Key Rotation - $(date)"

# Generate new key
echo "Generating new SSH key..."
ssh-keygen -t ed25519 -f "$NEW_KEY" -C "admin_${DATE}" -N ""

# Deploy new key ke semua server
for server in "${SERVERS[@]}"; do
    echo "Adding new key to $server..."
    
    # Tambahkan new key ke authorized_keys
    cat "${NEW_KEY}.pub" | ssh -i "$OLD_KEY" "$REMOTE_USER@$server" \
        "cat >> ~/.ssh/authorized_keys"
    
    echo "✅ New key added to $server"
done

# Test koneksi dengan key baru
echo ""
echo "Testing new key..."
for server in "${SERVERS[@]}"; do
    if ssh -i "$NEW_KEY" -o BatchMode=yes "$REMOTE_USER@$server" "echo connected" 2>/dev/null; then
        echo "✅ New key works on $server"
    else
        echo "❌ New key FAILED on $server - aborting!"
        exit 1
    fi
done

# Hapus old key dari server
echo ""
echo "Removing old key from servers..."
OLD_KEY_FINGERPRINT=$(ssh-keygen -l -f "${OLD_KEY}.pub" | awk '{print $2}')

for server in "${SERVERS[@]}"; do
    ssh -i "$NEW_KEY" "$REMOTE_USER@$server" \
        "sed -i '/$OLD_KEY_FINGERPRINT/d' ~/.ssh/authorized_keys"
    echo "✅ Old key removed from $server"
done

# Ganti key lokal
mv "$OLD_KEY" "${OLD_KEY}_${DATE}.bak"
mv "${OLD_KEY}.pub" "${OLD_KEY}_${DATE}.bak.pub"
mv "$NEW_KEY" "$OLD_KEY"
mv "${NEW_KEY}.pub" "${OLD_KEY}.pub"

echo ""
echo "✅ Key rotation complete!"
echo "Old key backed up as: ${OLD_KEY}_${DATE}.bak"
```

***

### Referensi & Sumber Daya

#### RFC & Standar

```
RFC 4251 - SSH Protocol Architecture
RFC 4252 - SSH Authentication Protocol
RFC 4253 - SSH Transport Layer Protocol
RFC 4254 - SSH Connection Protocol
RFC 4255 - Using DNS to Securely Publish SSH Key Fingerprints
RFC 4256 - Generic Message Exchange Authentication (GSSAPI)
RFC 8308 - Extension Negotiation in SSH
RFC 8332 - Use of RSA Keys with SHA-2 in SSH
```

#### Tools Penting

```
openssh-server    - SSH server daemon
openssh-client    - SSH client tools
ssh-keygen        - Key generation dan management
ssh-agent         - Key caching agent
ssh-add           - Agent key management
ssh-keyscan       - Host key scanner
sshpass           - Non-interactive password auth
autossh           - Auto-restart SSH tunnels
fail2ban          - Brute force protection
keychain          - Persistent agent management
```

#### File-File Penting

```
/etc/ssh/sshd_config           - Server configuration
/etc/ssh/ssh_config            - System-wide client config
~/.ssh/config                  - User client config
~/.ssh/known_hosts             - Known hosts database
~/.ssh/authorized_keys         - Authorized public keys
~/.ssh/id_ed25519              - Default Ed25519 private key
~/.ssh/id_ed25519.pub          - Default Ed25519 public key
/etc/ssh/ssh_host_*            - Server host keys
/var/log/auth.log              - Auth log (Debian/Ubuntu)
/var/log/secure                - Auth log (CentOS/RHEL)
```

***

_Dokumentasi ini dibuat dengan mempertimbangkan praktik terbaik keamanan terkini. Selalu pastikan untuk menggunakan versi OpenSSH terbaru dan secara berkala memeriksa advisory keamanan._

**Versi Dokumen:** 2.0\
**Terakhir Diperbarui:** 2024\
**Berlaku untuk:** OpenSSH 8.x dan yang lebih baru
