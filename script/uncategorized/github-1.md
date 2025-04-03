# GitHub

## Summary

* Membuat repo baru : `git init .`
* Menambahkan stagging : `git add . git add [namafile]`
* Mengecek staging : `git status`
* Git Commit -> menyimpan perubahan pada repo :  `git commit -m [isi pesan]`
* Git Log -> melihat history commit :  `git log`
* Mengebalikan repo ke kondisi commit tertentu : `git checkout [commithash]`
* Kembali ke commit tertentu dengan spesifik file git checkout \[commithash] -- \[namafile]
* Menambahkan GIT Remote : git remote add \[namaorigin/bebas] \[alamatgit]
* Mengecek kondisi atau perubahan pada repo remote : `git fetch`
* Mengambil data dari remote : `git pull`
* Mengirim perubahan ke remote : `git push -u [namaorigin] [branch] git push`
* Git Fork : Mengopy repo orang laing ke dalam repo kita git fork
* Git Branch : merupakan cabang sub bagian dari repo. ketika git pertama kali dibuat branch yang digunakan adalah main atau master.
* Membuat brach : `git branch [namabranch]`
* Masuk dalam branch tertentu :  `git checkout [namabranch]`
* Menggabungkan branch : `git branch merge`
* Mengatasi gagal push (! \[rejected] main -> main (non-fast-forward)) Ketika di sisi remote terdapat perubahan atau ada commit baru, lalu di local juga terdapat commit baru. Maka kemungkinan ketika dilakukan push akan gagal, maka cara yang dilakukan adalah : - mengecek perubahan di repo remote : git fetch - merge commit : git pull - commit ulang : git commit -m \[pesan] - Push ulang git push / git push -u \[namaorigin] \[namabrach]
* Mengatasi fatal: refusing to merge unrelated histories Hal tersebut terjadi ketika melakukan Pull dari remote dan commit yang terdapat pada Repo Remote dan Local tidak sama. Cara mengatasi : - git pull \[namaremote] \[namabranch] --allow-unrelated-histories



* Membuat repo baru :

```bash
  git init .
```

* Menambahkan stagging :

```bash
  git add .
  git add [namafile]
```

* Mengecek staging :

```
  git status
```

* Git Commit -> menyimpan perubahan pada repo

```
  git commit -m [isi pesan]
```

* Git Log -> melihat history commit

```
  git log
```

* Mengebalikan repo ke kondisi commit tertentu

```
  git checkout [commit hash]
```

* Kembali ke commit tertentu dengan spesifik file

```
  git checkout [commit hash] -- [namafile]
```

* Menambahkan GIT Remote :

```
  git remote add [nama origin/bebas] [alamatgit]
```

## Git Remote

* Mengecek kondisi atau perubahan pada repo remote :

```
  git fetch
```

* Mengambil data dari remote :

```
  git pull
```

* Mengirim perubahan ke remote :

```
  git push -u [nama origin] [branch]
  git push
```

*   Git Fork : Mengopy repo orang laing ke dalam repo kita

    ```bash
    git fork
    ```
* Git Branch : merupakan cabang sub bagian dari repo. ketika git pertama kali dibuat branch yang digunakan adalah main atau master.
* Membuat brach :

```
  git branch [nama branch]
```

* Masuk dalam branch tertentu

```
  git checkout [nama branch]
```

* Menggabungkan branch :

```
  git branch merge
```

## Git Conflict

Mengatasi gagal push (! \[rejected] main -> main (non-fast-forward))

Ketika di sisi remote terdapat perubahan atau ada commit baru, lalu di local juga terdapat commit baru. Maka kemungkinan ketika dilakukan push akan gagal, maka cara yang dilakukan adalah :thumbsup:mengecek perubahan di repo remote :

```bash
git fetch - merge commit
```

```
  git pull
```

* commit ulang :

```
git commit -m [pesan] - Push ulang
```

```
git push / git push -u [namaorigin] [namabrach]
```



## 2 User Github

Untuk menggunakan dua akun GitHub di Linux, Anda perlu mengonfigurasi Git dan SSH untuk membedakan antara kedua akun. Berikut adalah langkah-langkah lengkap untuk mencapai hal ini:

***

#### **1. Siapkan Dua SSH Key**

Setiap akun GitHub akan memiliki SSH key yang berbeda. Pastikan Anda membuat dua SSH key yang unik.

**Langkah-langkah:**

1.  **Buat SSH Key untuk Akun Pertama:**

    ```bash
    ssh-keygen -t ed25519 -C "email_akun_pertama@example.com"
    ```

    Simpan file ke lokasi default (misalnya: `~/.ssh/id_ed25519`).
2.  **Buat SSH Key untuk Akun Kedua:**

    ```bash
    ssh-keygen -t ed25519 -C "email_akun_kedua@example.com"
    ```

    Simpan file dengan nama yang berbeda (misalnya: `~/.ssh/id_ed25519_second`).

***

#### **2. Tambahkan SSH Key ke SSH Agent**

SSH agent digunakan untuk mengelola kunci SSH yang telah dibuat.

**Langkah-langkah:**

1.  Mulai SSH agent:

    ```bash
    eval "$(ssh-agent -s)"
    ```
2.  Tambahkan kedua SSH key ke SSH agent:

    ```bash
    ssh-add ~/.ssh/id_ed25519
    ssh-add ~/.ssh/id_ed25519_second
    ```

***

#### **3. Tambahkan SSH Key ke Akun GitHub**

Anda perlu menambahkan public key dari masing-masing SSH key ke akun GitHub yang sesuai.

**Langkah-langkah:**

1.  Salin public key untuk akun pertama:

    ```bash
    cat ~/.ssh/id_ed25519.pub
    ```

    Tambahkan public key ini ke akun GitHub pertama melalui [GitHub SSH Keys](https://github.com/settings/keys).
2.  Salin public key untuk akun kedua:

    ```bash
    cat ~/.ssh/id_ed25519_second.pub
    ```

    Tambahkan public key ini ke akun GitHub kedua.

***

#### **4. Konfigurasi SSH untuk Menggunakan Multiple Account**

Agar SSH tahu kapan harus menggunakan kunci mana, Anda perlu mengonfigurasi file `~/.ssh/config`.

**Contoh Konfigurasi:**

Edit file `~/.ssh/config` (buat jika tidak ada):

```bash
nano ~/.ssh/config
```

Tambahkan konfigurasi berikut:

```plaintext
# Akun Pertama
Host github.com-akunpertama
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519
    IdentitiesOnly yes

# Akun Kedua
Host github.com-akundua
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_second
    IdentitiesOnly yes
```

Penjelasan:

* `Host`: Nama alias untuk repositori GitHub.
* `HostName`: Domain GitHub (`github.com`).
* `User`: Selalu gunakan `git`.
* `IdentityFile`: Lokasi private key untuk akun tersebut.

***

#### **5. Ubah Remote URL Repositori**

Untuk setiap repositori, ubah remote URL agar sesuai dengan alias SSH yang telah dikonfigurasi.

**Contoh:**

1.  Untuk repositori akun pertama:

    ```bash
    git remote set-url origin git@github.com-akunpertama:username-pertama/repo-pertama.git
    ```
2.  Untuk repositori akun kedua:

    ```bash
    git remote set-url origin git@github.com-akundua:username-kedua/repo-kedua.git
    ```

Perhatikan bahwa `github.com-akunpertama` dan `github.com-akundua` adalah alias yang didefinisikan dalam file `~/.ssh/config`.

***

#### **6. Verifikasi Konfigurasi**

Pastikan konfigurasi SSH berfungsi dengan benar.

**Langkah-langkah:**

1.  Uji koneksi untuk akun pertama:

    ```bash
    ssh -T git@github.com-akunpertama
    ```

    Jika berhasil, Anda akan melihat pesan seperti:

    ```plaintext
    Hi username-pertama! You've successfully authenticated.
    ```
2.  Uji koneksi untuk akun kedua:

    ```bash
    ssh -T git@github.com-akundua
    ```

    Jika berhasil, Anda akan melihat pesan seperti:

    ```plaintext
    Hi username-kedua! You've successfully authenticated.
    ```

***

#### **7. Konfigurasi Global Git untuk Setiap Repositori**

Jika Anda ingin mengatur nama dan email Git secara terpisah untuk setiap repositori, Anda dapat mengonfigurasinya secara lokal.

**Contoh:**

1.  Untuk repositori akun pertama:

    ```bash
    git config user.name "Nama Akun Pertama"
    git config user.email "email_akun_pertama@example.com"
    ```
2.  Untuk repositori akun kedua:

    ```bash
    git config user.name "Nama Akun Kedua"
    git config user.email "email_akun_kedua@example.com"
    ```

Verifikasi konfigurasi:

```bash
git config --list
```

***

#### **8. Debugging Jika Masih Ada Masalah**

Jika Anda masih mengalami masalah, coba jalankan perintah Git dengan verbose mode untuk mendapatkan informasi lebih rinci:

```bash
GIT_TRACE=1 GIT_CURL_VERBOSE=1 git clone git@github.com-akunpertama:username-pertama/repo-pertama.git
```

***

#### **Kesimpulan**

Dengan mengonfigurasi SSH key yang berbeda dan file `~/.ssh/config`, Anda dapat menggunakan dua akun GitHub di Linux tanpa konflik. Pastikan Anda selalu menggunakan alias SSH yang benar untuk setiap repositori.

Jika Anda masih mengalami masalah, silakan berikan detail tambahan seperti:

* Apakah error spesifik yang muncul?
* Apakah Anda sudah mencoba semua langkah di atas?

Dengan informasi tersebut, saya dapat memberikan solusi yang lebih spesifik.
