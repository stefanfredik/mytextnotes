---
description: Mikrotik QOS
---

# Qos

## Queue

Apa itu Burst di Mikrotik?Burst adalah mekanisme di Simple Queue yang memberikan "kecepatan tambahan" di atas Max-Limit untuk durasi tertentu. Misalnya, jika pengguna dibatasi pada kecepatan 2 Mbps, dengan Burst mereka bisa mendapatkan hingga 4 Mbps untuk waktu tertentu (misalnya 10 detik), tergantung pada penggunaan sebelumnya dan pengaturan lainnya. Setelah waktu Burst habis, kecepatan kembali ke Max-Limit.Fitur ini biasanya digunakan untuk:

* Memberikan kecepatan lebih saat pengguna membuka halaman web atau memulai streaming.
* Memberi insentif kepada pengguna agar tidak terus-menerus memaksimalkan bandwidth.
* Mengoptimalkan pengalaman pengguna di jaringan dengan sumber daya terbatas.

***

Komponen Utama BurstDalam Simple Queue, Burst memiliki beberapa parameter yang perlu dikonfigurasi:

1. Burst Limit: Kecepatan maksimum yang diizinkan saat Burst aktif (harus lebih tinggi dari Max-Limit).
2. Burst Threshold: Ambang batas penggunaan bandwidth yang menentukan kapan Burst bisa aktif.
3. Burst Time: Durasi maksimum Burst aktif dalam satu kali penggunaan.
4. Max-Limit: Kecepatan normal yang menjadi acuan sebelum dan sesudah Burst.

***

Cara Kerja BurstBurst bekerja berdasarkan logika "kredit" atau "penggunaan rata-rata". Berikut adalah langkah-langkah kerjanya:

1. Perhitungan Rata-Rata: Mikrotik menghitung rata-rata penggunaan bandwidth pengguna dalam periode tertentu (biasanya beberapa detik hingga menit sebelumnya).
2. Pemicu Burst: Jika rata-rata penggunaan lebih rendah dari Burst Threshold, pengguna berhak mendapatkan Burst.
3. Aktivasi Burst: Ketika pengguna mulai menggunakan bandwidth lebih tinggi (misalnya untuk download atau streaming), kecepatan akan naik hingga Burst Limit selama Burst Time.
4. Kembali ke Normal: Setelah Burst Time habis atau penggunaan melebihi ambang batas, kecepatan kembali ke Max-Limit.

Contoh Sederhana:

* Max-Limit: 2 Mbps
* Burst Limit: 4 Mbps
* Burst Threshold: 1.5 Mbps
* Burst Time: 10 detik Jika pengguna sebelumnya hanya menggunakan 1 Mbps (di bawah 1.5 Mbps), saat mereka mulai download, kecepatan bisa naik ke 4 Mbps selama 10 detik.

***

Konfigurasi Burst di Simple QueueBerikut langkah-langkah untuk mengatur Burst di Mikrotik:Melalui WinBox

1. Buka menu Queues > Simple Queue.
2. Tambah aturan baru dengan klik tanda "+", atau edit aturan yang sudah ada.
3. Pada tab General:
   * Isi Target (misalnya IP 192.168.1.10).
   * Set Max-Limit (misalnya 2M/2M untuk download/upload).
4. Pada tab Advanced:
   * Aktifkan Burst dengan memilih Yes di opsi Burst.
   * Isi Burst Limit (misalnya 4M/4M).
   * Isi Burst Threshold (misalnya 1.5M/1.5M).
   * Isi Burst Time (misalnya 00:00:10 untuk 10 detik).
5. Klik OK untuk menyimpan.

Melalui CLI (Command Line Interface)

```
/queue simple
add name="User1" target=192.168.1.10 max-limit=2M/2M burst-limit=4M/4M burst-threshold=1.5M/1.5M burst-time=10s
```

***

Parameter Burst Secara Detail

1. Burst Limit:
   * Menentukan kecepatan maksimum saat Burst aktif.
   * Harus lebih besar dari Max-Limit, misalnya 2x atau 3x lipat.
   * Contoh: Jika Max-Limit 2 Mbps, Burst Limit bisa 4 Mbps atau 6 Mbps.
2. Burst Threshold:
   * Ambang batas yang menentukan apakah pengguna "layak" mendapatkan Burst.
   * Jika rata-rata penggunaan di bawah nilai ini, Burst akan tersedia.
   * Biasanya diatur sedikit di bawah Max-Limit (misalnya 75%-80% dari Max-Limit).
3. Burst Time:
   * Lama waktu Burst aktif dalam satu sesi.
   * Formatnya HH:MM:SS (jam:menit:detik), misalnya 00:00:10 = 10 detik.
   * Setelah waktu habis, pengguna harus "mengumpulkan kredit" lagi dengan menggunakan bandwidth di bawah Threshold.
4. Max-Limit:
   * Kecepatan normal sebelum dan sesudah Burst.
   * Menjadi dasar perhitungan Burst Threshold.

***

Contoh SkenarioSkenario 1: Pengguna Rumahan

* Max-Limit: 5 Mbps (download), 1 Mbps (upload)
* Burst Limit: 10 Mbps (download), 2 Mbps (upload)
* Burst Threshold: 4 Mbps (download), 0.8 Mbps (upload)
* Burst Time: 20 detik
* Hasil: Jika pengguna sebelumnya hanya browsing (pakai < 4 Mbps), saat mulai streaming, kecepatan naik ke 10 Mbps selama 20 detik, lalu kembali ke 5 Mbps.

Skenario 2: Warnet

* Max-Limit: 1 Mbps per pengguna
* Burst Limit: 3 Mbps
* Burst Threshold: 0.8 Mbps
* Burst Time: 15 detik
* Hasil: Setiap pengguna bisa "ngebut" sampai 3 Mbps saat membuka website atau game, tapi hanya 15 detik, sehingga bandwidth tidak boros.

***

Kelebihan dan Kekurangan BurstKelebihan:

* Meningkatkan pengalaman pengguna tanpa menambah bandwidth permanen.
* Fleksibel untuk kebutuhan sesaat seperti membuka halaman web atau buffering video.
* Mudah dikonfigurasi di Simple Queue.

Kekurangan:

* Jika Burst Time terlalu lama atau Burst Limit terlalu tinggi, bisa membebani jaringan.
* Pengguna yang terus-menerus memaksimalkan bandwidth tidak akan mendapat Burst (karena melebihi Threshold).
* Tidak cocok untuk kontrol ketat di jaringan besar (lebih baik pakai Queue Tree).

***

Tips Mengoptimalkan Burst

1. Sesuaikan Burst Threshold: Jangan terlalu rendah agar Burst tidak terlalu sulit didapat, tapi juga jangan terlalu tinggi agar tidak boros bandwidth.
2. Batasi Burst Time: 10-30 detik biasanya cukup untuk kebutuhan umum.
3. Monitor Penggunaan: Gunakan Queue Statistics untuk melihat seberapa sering Burst aktif.
4. Kombinasikan dengan PCQ: Jika banyak pengguna, gunakan PCQ untuk membagi bandwidth, lalu tambahkan Burst per pengguna.
