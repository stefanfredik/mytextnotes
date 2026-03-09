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
