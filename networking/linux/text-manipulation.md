# Text Manipulation

Berikut adalah **panduan lengkap perintah-perintah untuk text manipulation di Linux**, sangat berguna untuk sysadmin, DevOps, programmer, dan pengguna umum Linux. Semua disusun per kategori agar mudah dipahami dan langsung bisa dipraktikkan di terminal.

***

## 📘 PANDUAN LENGKAP TEXT MANIPULATION DI LINUX

***

### 📌 1. **Perintah Dasar**

| Perintah | Fungsi                                  | Contoh          |
| -------- | --------------------------------------- | --------------- |
| `cat`    | Menampilkan isi file                    | `cat file.txt`  |
| `tac`    | Menampilkan isi file secara terbalik    | `tac file.txt`  |
| `nl`     | Menampilkan isi file dengan nomor baris | `nl file.txt`   |
| `head`   | Menampilkan 10 baris awal               | `head file.txt` |
| `tail`   | Menampilkan 10 baris akhir              | `tail file.txt` |
| `less`   | Navigasi isi file (↑↓/space/q)          | `less file.txt` |
| `more`   | Seperti `less`, tapi lebih sederhana    | `more file.txt` |

***

### 🧵 2. **Perintah Pemotongan (Cut, Awk)**

#### 🔹 `cut` — Potong teks berdasarkan kolom atau karakter

```bash
cut -d':' -f1 /etc/passwd     # Ambil kolom pertama, delimiter ':'
cut -c1-5 file.txt            # Ambil karakter 1 sampai 5
```

#### 🔹 `awk` — Proses teks per kolom

```bash
awk '{print $1}' file.txt     # Ambil kolom pertama
awk -F':' '{print $1,$3}' /etc/passwd  # Custom delimiter
```

***

### 🧪 3. **Pencarian & Filter (grep, find, locate)**

| Perintah  | Fungsi                                  | Contoh                    |
| --------- | --------------------------------------- | ------------------------- |
| `grep`    | Cari baris yang cocok dengan pola regex | `grep "error" log.txt`    |
| `grep -i` | Tidak case-sensitive                    | `grep -i "fatal" log.txt` |
| `grep -r` | Rekursif di direktori                   | `grep -r "nginx" /etc/`   |
| `find`    | Cari file di direktori                  | `find /var -name "*.log"` |
| `locate`  | Cari file cepat via database            | `locate nginx.conf`       |

***

### 🔄 4. **Penggantian (sed, tr)**

#### 🔹 `sed` — Stream editor

```bash
sed 's/foo/bar/' file.txt       # Ganti "foo" dengan "bar" (1x)
sed 's/foo/bar/g' file.txt      # Ganti semua "foo"
sed '/hapuskan/d' file.txt      # Hapus baris berisi "hapuskan"
sed -n '5p' file.txt            # Tampilkan baris ke-5
```

#### 🔹 `tr` — Translate atau delete karakter

```bash
echo "halo" | tr 'a-z' 'A-Z'    # Konversi ke huruf besar
tr -d '\r' < winfile.txt        # Hapus karakter carriage return
```

***

### 🔀 5. **Sortir, Unik, Gabung**

| Perintah  | Fungsi                             | Contoh                       |
| --------- | ---------------------------------- | ---------------------------- |
| `sort`    | Urutkan isi file                   | `sort data.txt`              |
| `sort -r` | Urutkan terbalik                   | `sort -r data.txt`           |
| `uniq`    | Hapus duplikat baris berurutan     | \`sort file.txt              |
| `uniq -c` | Hitung duplikat                    | \`sort file.txt              |
| `paste`   | Gabung file per baris (horizontal) | `paste file1.txt file2.txt`  |
| `join`    | Gabung file per kolom kunci        | `join -1 1 -2 1 file1 file2` |

***

### 🔢 6. **Hitung Jumlah**

| Perintah | Fungsi                      | Contoh           |
| -------- | --------------------------- | ---------------- |
| `wc -l`  | Hitung jumlah baris         | `wc -l file.txt` |
| `wc -w`  | Hitung jumlah kata          | `wc -w file.txt` |
| `wc -c`  | Hitung jumlah byte/karakter | `wc -c file.txt` |

***

### 🧹 7. **Pembersihan Teks**

| Perintah        | Fungsi                                   | Contoh                   |
| --------------- | ---------------------------------------- | ------------------------ |
| `dos2unix`      | Konversi newline Windows ke Linux        | `dos2unix file.txt`      |
| `sed '/^$/d'`   | Hapus baris kosong                       | `sed '/^$/d' file.txt`   |
| `sed 's/ *$//'` | Hapus spasi di akhir baris               | `sed 's/ *$//' file.txt` |
| `tr -s ' '`     | Replace multiple space dengan satu space | \`cat file.txt           |

***

### 🔄 8. **Text Replacement Multiple File**

```bash
grep -rl 'lama' ./ | xargs sed -i 's/lama/baru/g'
```

> Ganti semua kata "lama" menjadi "baru" secara rekursif di semua file

***

### 🧠 9. **Perintah Kombinasi & Pipeline**

```bash
cat file.txt | grep 'error' | cut -d' ' -f2 | sort | uniq -c | sort -nr
```

> Contoh pipeline: cari error, ambil kolom 2, sort, hitung, dan urutkan

***

### 🔧 10. **Alat Ekstra Modern**

| Tool     | Keterangan                                 |
| -------- | ------------------------------------------ |
| `jq`     | JSON processor                             |
| `xargs`  | Eksekusi per baris/argumen dari hasil pipe |
| `fzf`    | Fuzzy finder interaktif                    |
| `csvkit` | Tools powerful untuk CSV                   |

***

### 📦 Bonus: JSON View dengan CLI

#### Lihat isi JSON dengan format rapi:

```bash
cat data.json | jq
```

***

### 🎯 Rekomendasi Belajar

1. **Latihan di file log atau konfigurasi Linux**
2. Buat script bash harian (misalnya filter error log, parsing IP)
3. Gunakan `watch`, `cron`, dan logging

***

### 📁 Simpan Panduan Ini

Untuk menyimpan panduan ini ke file teks:

```bash
nano text-manipulation-cheatsheet.txt
```

***

Kalau kamu ingin saya buatkan:

* **Latihan harian text manipulation**
* **Script bash parsing log / monitoring**
* **Proyek mini parsing teks nyata**

Tinggal bilang, saya siap bantu!
