# Tmux

### Apa itu tmux?

**tmux (Terminal Multiplexer)** adalah tool di Linux yang memungkinkan kamu menjalankan banyak session terminal dalam satu terminal, dan tetap berjalan meskipun SSH terputus.

Sebagai network engineer (apalagi kalau sering SSH ke server, OLT, atau router), ini sangat berguna untuk:

* Monitoring log sambil konfigurasi
* Menjalankan script lama tanpa takut putus
* Multi-session SSH dalam satu window

***

### 1️⃣ Install tmux

#### Ubuntu / Debian

```bash
sudo apt install tmux
```

```bash
tmux -V
```

***

### 2️⃣ Menjalankan tmux

Buat session baru:

```bash
tmux
```

Atau beri nama session:

```bash
tmux new -s monitoring
```

***

### 3️⃣ Detach & Attach (Fitur Paling Penting)

#### Detach (keluar tanpa menghentikan proses)

Tekan:

```
Ctrl + b
lalu tekan d
```

Session tetap jalan di background.

***

#### Melihat daftar session

```bash
tmux ls
```

***

#### Masuk kembali ke session

```bash
tmux attach -t monitoring
```

Kalau cuma satu session:

```bash
tmux attach
```

***

### 4️⃣ Membagi Layar (Split Window)

Default prefix tmux adalah:

```
Ctrl + b
```

Semua command tmux diawali dengan ini.

***

#### Split horizontal

```
Ctrl + b lalu "
```

#### Split vertical

```
Ctrl + b lalu %
```

***

#### Pindah antar pane

```
Ctrl + b lalu panah (← ↑ ↓ →)
```

***

### 5️⃣ Membuat Window Baru

Window di tmux seperti tab.

Buat window baru:

```
Ctrl + b lalu c
```

Pindah window:

```
Ctrl + b lalu n   (next)
Ctrl + b lalu p   (previous)
```

***

### 6️⃣ Menutup Pane / Window

Tutup pane cukup:

```bash
exit
```

Atau tekan:

```
Ctrl + d
```

Kalau semua pane dalam window ditutup → window hilang.

***

### 7️⃣ Kill Session

```bash
tmux kill-session -t monitoring
```

Atau kill semua:

```bash
tmux kill-server
```

***

## 🔥 Contoh Real Use Case Untuk Kamu (ISP / NMS)

Misalnya kamu:

* Pane 1 → `tail -f log`
* Pane 2 → SSH ke Mikrotik
* Pane 3 → Running script Python monitoring
* Detach
* Laptop mati
* Besok attach lagi → semua masih jalan

Itu kekuatan tmux.

***

## 8️⃣ Best Practice (Professional Workflow)

Biasanya engineer pakai:

```bash
tmux new -s project-nms
```

Split jadi 3 pane:

* Coding
* Running server
* Monitoring log

Detach kalau mau pulang.

***

## 9️⃣ Custom Prefix (Opsional)

Kalau mau ubah prefix jadi `Ctrl + a` (seperti screen):

Edit:

```bash
nano ~/.tmux.conf
```

Isi:

```
set -g prefix C-a
unbind C-b
bind C-a send-prefix
```

Reload config:

```
tmux source-file ~/.tmux.conf
```

