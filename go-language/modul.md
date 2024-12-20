---
icon: book-open
---

# Modul

Berikut adalah modul belajar Bahasa Go (Golang) dari dasar hingga tingkat menengah secara detail. Modul ini dibagi menjadi beberapa bagian berdasarkan tingkat kesulitan dan cakupan materi.

***

#### **Modul 1: Pengantar Golang**

**1.1 Apa itu Golang?**

* Sejarah Golang
* Keunggulan dan Kelebihan Golang
* Instalasi Go di berbagai sistem operasi
* Struktur proyek dasar di Go

**1.2 Memulai dengan Golang**

* Menulis program pertama: **Hello, World!**
* Penjelasan tentang package `main` dan fungsi `main()`
* Penjelasan tools: `go run`, `go build`, `go fmt`, `go test`

**1.3 Dasar-Dasar Golang**

* **Variabel dan Konstanta**: Deklarasi, tipe data bawaan
* **Operator**: Aritmatika, logika, perbandingan
* **Input dan Output**: Menggunakan `fmt.Println`, `fmt.Scanln`
* **Komentar**: Single-line dan Multi-line

**Latihan:**

1. Buat program sederhana untuk menghitung luas persegi panjang.
2. Buat program untuk menerima input nama dan usia, lalu mencetaknya.

***

#### **Modul 2: Struktur Kontrol dan Fungsi**

**2.1 Struktur Kontrol**

* **If-Else**: Struktur kondisi
* **Switch**: Alternatif untuk banyak kondisi
* **For Loop**: Looping di Golang
* **Break and Continue**

**2.2 Fungsi**

* Deklarasi fungsi
* Fungsi dengan parameter dan return value
* Fungsi variadic
* Fungsi anonim (anonymous functions)
* Defer, Panic, Recover

**Latihan:**

1. Buat fungsi untuk menghitung faktorial dari sebuah bilangan.
2. Buat program menggunakan `switch` untuk menentukan grade nilai.

***

#### **Modul 3: Tipe Data Lanjutan dan Struktur Data**

**3.1 Array, Slice, dan Map**

* **Array**: Deklarasi, akses, iterasi
* **Slice**: Pengenalan, manipulasi, fungsi `append`, `copy`
* **Map**: Key-value pair, operasi dasar pada map

**3.2 Struct**

* Pengantar ke struct
* Deklarasi dan penggunaan struct
* Embedded struct

**3.3 Pointer**

* Konsep pointer di Go
* Operasi pointer: Referensi (`&`) dan dereferensi (`*`)

**Latihan:**

1. Buat struct untuk menyimpan data mahasiswa (nama, usia, nilai).
2. Buat slice untuk menambahkan data mahasiswa dinamis.

***

#### **Modul 4: Pemrograman Berorientasi Objek (OOP) di Golang**

**4.1 Konsep OOP dalam Go**

* Tidak ada class, menggunakan struct
* Encapsulation menggunakan export/unexport

**4.2 Method**

* Method pada struct
* Method dengan receiver pointer vs receiver value

**4.3 Interface**

* Pengantar interface
* Implementasi interface pada struct
* Polimorfisme menggunakan interface

**Latihan:**

1. Buat interface untuk kendaraan (berisi metode `Start` dan `Stop`), lalu buat struct `Mobil` dan `Motor` yang mengimplementasikan interface ini.

***

#### **Modul 5: Concurrency di Golang**

**5.1 Goroutines**

* Apa itu Goroutine?
* Membuat dan menjalankan goroutine
* Synchronization menggunakan `sync.WaitGroup`

**5.2 Channel**

* Pengantar ke channel
* Operasi dasar: send dan receive
* Channel buffered vs unbuffered
* Select statement

**Latihan:**

1. Buat program sederhana yang menjalankan 3 goroutine secara bersamaan untuk mencetak angka 1-10.

***

#### **Modul 6: Error Handling**

**6.1 Error di Golang**

* Pendekatan error sederhana menggunakan `errors.New`
* Custom error dengan `fmt.Errorf`

**6.2 Panic dan Recover**

* Kapan menggunakan panic dan recover
* Contoh penggunaan panic dan recover

**Latihan:**

1. Buat fungsi yang menerima dua angka dan mengembalikan hasil pembagian. Jika pembagi adalah nol, kembalikan error.

***

#### **Modul 7: File dan Database**

**7.1 Operasi File**

* Membaca file dengan `os` dan `io/ioutil`
* Menulis file
* Operasi file dasar: membuka, menutup, menghapus

**7.2 Pengantar Database**

* Menggunakan package `database/sql`
* Koneksi ke database MySQL atau SQLite
* Operasi CRUD sederhana

**Latihan:**

1. Buat program untuk membaca file teks dan mencetak isi file ke layar.
2. Buat program CRUD sederhana untuk tabel `users` di database.

***

#### **Modul 8: Web Development dengan Go**

**8.1 Membuat Web Server**

* Pengantar package `net/http`
* Membuat HTTP server sederhana
* Menangani request dan response

**8.2 Routing**

* Membuat routing sederhana
* Menggunakan library routing seperti `gorilla/mux`

**8.3 JSON API**

* Menghasilkan response JSON
* Parsing JSON request

**Latihan:**

1. Buat REST API sederhana untuk menyimpan dan membaca data buku.

***

#### **Modul 9: Testing di Golang**

**9.1 Unit Testing**

* Pengantar package `testing`
* Menulis test case untuk fungsi

**9.2 Benchmark**

* Menulis benchmark test
* Mengoptimalkan performa berdasarkan hasil benchmark

**9.3 Table-Driven Test**

* Penulisan test dengan pendekatan table-driven

**Latihan:**

1. Buat unit test untuk fungsi faktorial yang sudah dibuat sebelumnya.

***

#### **Tips dan Sumber Belajar Lanjutan**

1. **Dokumentasi Resmi**: [golang.org/doc](https://golang.org/doc)
2. **Buku**: "The Go Programming Language" oleh Alan A. A. Donovan dan Brian W. Kernighan
3. **Kursus Online**: Platform seperti Udemy, Coursera, atau YouTube
4. **Proyek Open Source**: Berkontribusi di proyek open source berbasis Golang untuk belajar lebih dalam.

Semoga modul ini membantu dalam proses pembelajaran! 🎉
