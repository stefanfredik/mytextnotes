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
