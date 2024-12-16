---
description: All About DNS
---

# DNS

## Menguji DNS Server

Untuk mengetes kecepatan respon DNS server di sistem operasi Linux, Anda dapat menggunakan beberapa tools bawaan atau pihak ketiga. Berikut adalah beberapa metode yang bisa Anda gunakan:

***

#### **1. Menggunakan `dig`**

Tool `dig` (Domain Information Groper) digunakan untuk melakukan query ke DNS server dan mengukur waktu respon.

**Langkah:**

1. Buka terminal.
2.  Gunakan perintah berikut:

    ```bash
    dig @<IP_DNS_SERVER> <domain> +noall +stats
    ```

    Contoh:

    ```bash
    dig @8.8.8.8 google.com +noall +stats
    ```
3.  Output akan memberikan waktu respon di bagian `Query time`:

    ```
    ;; Query time: 23 msec
    ```

***

#### **2. Menggunakan `nslookup`**

`nslookup` juga bisa digunakan untuk mengetes waktu respon DNS server.

**Langkah:**

1.  Jalankan perintah berikut:

    ```bash
    nslookup <domain> <IP_DNS_SERVER>
    ```

    Contoh:

    ```bash
    nslookup google.com 8.8.8.8
    ```
2. Perhatikan waktu yang dibutuhkan untuk mendapatkan hasil. Namun, `nslookup` tidak memberikan waktu respon secara langsung, sehingga Anda perlu mencatat waktu secara manual.

***

#### **3. Menggunakan `ping` untuk Nama Domain**

`ping` bisa digunakan untuk mengecek apakah nama domain dapat di-resolve dan mendapatkan IP dengan cepat.

**Langkah:**

1.  Gunakan perintah:

    ```bash
    ping -c 1 <domain>
    ```

    Contoh:

    ```bash
    ping -c 1 google.com
    ```
2. Output akan menunjukkan waktu respon DNS dalam milidetik (waktu round-trip).

***

#### **4. Menggunakan `dnsperf`**

`dnsperf` adalah tool khusus untuk menguji performa DNS server dengan banyak query.

**Instalasi:**

1.  Instal tool ini (tergantung distro Anda):

    ```bash
    sudo apt install dnsperf
    ```
2.  Gunakan perintah berikut:

    ```bash
    dnsperf -s <IP_DNS_SERVER> -d <file_domain_list> -l <duration>
    ```

    Contoh:

    ```bash
    dnsperf -s 8.8.8.8 -d domains.txt -l 10
    ```

    File `domains.txt` berisi daftar domain yang ingin diuji, satu per baris.

***

#### **5. Menggunakan `systemd-resolve`**

Jika Anda menggunakan distribusi Linux modern dengan systemd, gunakan perintah ini:

**Langkah:**

1.  Jalankan:

    ```bash
    systemd-resolve --statistics
    ```
2. Ini akan menunjukkan statistik termasuk waktu respon cache DNS lokal.

***

#### **6. Menggunakan `drill` (Bagian dari `ldnsutils`)**

`drill` adalah alternatif `dig` dengan output lebih sederhana.

**Langkah:**

1.  Instal `drill` jika belum ada:

    ```bash
    sudo apt install ldnsutils
    ```
2.  Gunakan perintah berikut:

    ```bash
    drill @<IP_DNS_SERVER> <domain>
    ```

    Contoh:

    ```bash
    drill @8.8.8.8 google.com
    ```
3. Perhatikan waktu respon di bagian `Query time`.

***

Metode-metode di atas akan membantu Anda mengukur performa DNS server untuk kebutuhan troubleshooting atau analisis kecepatan respon. Pilih tool yang paling sesuai dengan preferensi Anda.
