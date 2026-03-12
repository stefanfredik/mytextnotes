# SSH

## Login SSH Key

### Windows

### Login SSH Menggunakan SSH Key dari Windows ke Linux

#### 1. Generate SSH Key di Windows

Buka **PowerShell** atau **Command Prompt**, lalu jalankan:

```powershell
ssh-keygen -t ed25519 -C "email@kamu.com"
```

* Tekan **Enter** untuk lokasi default (`C:\Users\namauser\.ssh\id_ed25519`)
* Masukkan passphrase (opsional, tapi disarankan)

***

#### 2. Copy Public Key ke Server Linux

**Cara A — Pakai `ssh-copy-id` (di Git Bash/WSL):**

```bash
ssh-copy-id -i ~/.ssh/id_ed25519.pub user@ip-server
```

**Cara B — Manual (PowerShell):**

```powershell
type $env:USERPROFILE\.ssh\id_ed25519.pub | ssh user@ip-server "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
```

***

#### 3. Set Permission di Linux (Penting!)

Login dulu via password, lalu jalankan:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

***

#### 4. Login Menggunakan SSH Key

```powershell
ssh -i C:\Users\namauser\.ssh\id_ed25519 user@ip-server
```

Atau cukup:

```powershell
ssh user@ip-server
```

_(jika key disimpan di lokasi default)_

***

#### 5. Simpan Config Agar Lebih Mudah (Opsional)

Buat file `C:\Users\namauser\.ssh\config`:

```
Host myserver
    HostName ip-server
    User user
    IdentityFile ~/.ssh/id_ed25519
```

Setelah itu cukup ketik:

```powershell
ssh myserver
```

***

#### Tips Tambahan

| Tool                               | Keterangan                            |
| ---------------------------------- | ------------------------------------- |
| **OpenSSH** (bawaan Windows 10/11) | Sudah built-in, tidak perlu install   |
| **PuTTY**                          | Alternatif GUI, format key-nya `.ppk` |
| **WSL**                            | Bisa pakai `ssh-copy-id` langsung     |

> **Catatan:** Windows 10 versi 1809 ke atas sudah include OpenSSH secara default.





## Github SSH

Mengonfigurasi SSH untuk GitHub di Linux sangat disarankan karena Anda tidak perlu lagi memasukkan _username_ dan _password_ (atau _personal access token_) setiap kali melakukan `push` atau `pull`.

Berikut adalah panduan langkah demi langkah untuk mengaturnya:

#### 1. Cek Apakah Anda Sudah Memiliki SSH Key

Sebelum membuat yang baru, mari pastikan apakah komputer Anda sudah memiliki SSH key. Buka terminal Linux Anda dan jalankan:

Bash

```
ls -al ~/.ssh
```

* Jika Anda melihat file bernama `id_rsa.pub`, `id_ecdsa.pub`, atau `id_ed25519.pub`, Anda sudah memiliki _key_ dan bisa langsung melompat ke Langkah 3.
* Jika muncul pesan error atau file tersebut tidak ada, lanjut ke Langkah 2.

***

#### 2. Buat SSH Key Baru

Algoritma yang paling direkomendasikan saat ini oleh GitHub adalah Ed25519. Di terminal, jalankan perintah berikut (ganti email dengan email akun GitHub Anda):

Bash

```
ssh-keygen -t ed25519 -C "email_anda@example.com"
```

* Anda akan diminta menentukan lokasi penyimpanan file. Tekan Enter saja untuk menyimpannya di lokasi _default_.
* Selanjutnya, Anda akan diminta memasukkan _passphrase_ (kata sandi tambahan untuk keamanan ekstra). Anda bisa mengisinya, atau tekan Enter dua kali jika ingin mengosongkannya.

***

#### 3. Tambahkan SSH Key ke `ssh-agent`

Agar Linux bisa menggunakan _key_ yang baru saja dibuat, kita perlu menambahkannya ke program bernama `ssh-agent`.

Pertama, jalankan `ssh-agent` di _background_:

Bash

```
eval "$(ssh-agent -s)"
```

Kedua, tambahkan _private key_ Anda ke agen tersebut:

Bash

```
ssh-add ~/.ssh/id_ed25519
```

***

#### 4. Salin Public Key Anda

Sekarang, Anda perlu menyalin isi dari _public key_ Anda untuk dimasukkan ke GitHub. Jalankan perintah ini untuk menampilkannya di layar:

Bash

```
cat ~/.ssh/id_ed25519.pub
```

Blok dan salin (copy) semua teks yang muncul (biasanya berawalan `ssh-ed25519` dan diakhiri dengan alamat email Anda).

***

#### 5. Tambahkan SSH Key ke GitHub

1. Buka browser dan login ke akun GitHub Anda.
2. Klik foto profil Anda di pojok kanan atas, lalu pilih Settings.
3. Di menu sebelah kiri, klik SSH and GPG keys.
4. Klik tombol hijau bertuliskan New SSH key.
5. Isi kolom Title dengan nama yang mudah diingat (misalnya: "Laptop Linux Saya").
6. Pada bagian Key type, biarkan default (Authentication Key).
7. Paste teks yang sudah Anda salin di Langkah 4 ke dalam kolom Key.
8. Klik Add SSH key.

***

#### 6. Uji Koneksi

Untuk memastikan semuanya berjalan lancar, kembali ke terminal Linux Anda dan jalankan perintah ini:

Bash

```
ssh -T git@github.com
```

* Jika muncul peringatan seperti _"Are you sure you want to continue connecting (yes/no/\[fingerprint])?"_, ketik yes lalu tekan Enter.
* Jika berhasil, Anda akan melihat pesan seperti ini:

> _Hi username! You've successfully authenticated, but GitHub does not provide shell access._

***

Proses selesai! Sekarang Anda sudah bisa menggunakan Git dengan URL SSH (misalnya: `git@github.com:username/repository.git`).

Apakah Anda juga ingin saya pandu cara mengatur _username_ dan _email_ Git (`git config`) di Linux Anda agar siap untuk melakukan _commit_?
