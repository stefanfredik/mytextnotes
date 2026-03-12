# GIT

Error ini terjadi karena di _remote repository_ (GitLab) terdapat perubahan baru yang belum ada di lokasi (komputer) Anda. Git menolak `push` untuk mencegah agar perubahan dari orang lain (atau dari Anda sendiri di komputer lain) tidak tertimpa/hilang.

Untuk mengatasinya, Anda perlu mengintegrasikan (menggabungkan) perubahan dari remote terlebih dahulu, baru kemudian melakukan push ulang.

Berikut adalah dua cara yang umum digunakan. Anda bisa memilih salah satu melalui terminal tersebut:

**Cara 1: Menggunakan Git Pull (Direkomendasikan - Paling Aman)** Cara ini akan mengambil perubahan terbaru dari remote dan menggabungkannya dengan commit lokal Anda (akan membuat _merge commit_ jika ada konflik history).

Jalankan perintah ini:

```
bashgit pull origin main
```

_(Jika muncul layar teks/editor yang meminta Anda memasukkan pesan commit untuk merge, Anda bisa langsung menyimpannya. Jika terbuka dengan `nano` tekan `Ctrl+O`, `Enter`, lalu `Ctrl+X`. Jika terbuka di `vim`, ketik `:wq` lalu `Enter`)_

Setelah pull berhasil, silakan push kembali:

```
bashgit push origin main
```

**Cara 2: Menggunakan Git Pull Rebase (History akan lebih rapi)** Daripada membuat merge commit, cara ini akan menaruh commit-commit yang baru saja Anda buat di atas perubahan terbaru dari remote.

```
bashgit pull --rebase origin main
```

Setelah rebase selesai:

```
bashgit push origin main
```

**Catatan (Hanya jika Anda benar-benar yakin perubahan di remote tidak penting):** Jika Anda **sangat yakin** bahwa perubahan di GitLab tersebut tidak penting / boleh ditimpa seutuhnya oleh kode lokal Anda saat ini, Anda bisa memaksa push (namun ini berbahaya dan bisa menghilangkan kerjaan orang lain). Perintahnya:

```
bashgit push origin main --force
```

Namun, hindari cara ketiga ini kecuali Anda betul-betul yakin. Cobalah Cara 1 atau Cara 2 terlebih dahulu.

