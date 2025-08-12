# Uboot

## Pendahuluan

Siap, saya akan buatkan penjelasan mendalam dan komprehensif tentang U-Boot, mencakup arsitektur internal, cara kerja, fungsi, serta cara penggunaan baik secara umum maupun khusus pada perangkat berbasis Realtek MIPS. Saya juga akan membahas aspek operasional praktis dan teknis (termasuk source code dan alur internal) dari sudut pandang developer maupun end-user.

Saya akan segera kembali dengan hasilnya.

## U-Boot (Das U-Boot) pada Sistem Embedded

**U-Boot** (singkatan dari _Universal Boot Loader_, sering disebut _Das U-Boot_) adalah bootloader open-source untuk perangkat embedded berbasis berbagai arsitektur (PowerPC, ARM, MIPS, dll.). U-Boot bertugas menginisialisasi hardware di perangkat embedded, mengelola variabel lingkungan, memuat dan menjalankan kernel OS. Ia mendukung banyak antarmuka (flash, SD/MMC, Ethernet, USB) dan sistem file (FAT, ext2/3/4, UBI, dll.), serta protokol jaringan (TFTP, DHCP, NFS, dsb.). U-Boot juga menyediakan shell interaktif (monitor) yang kaya perintah, memungkinkan debugging (mis. membaca/mengubah memori, flash, dsb.).

### 1. Arsitektur Internal U-Boot

#### 1.1 Tahapan SPL/TPL dan U-Boot Utama

U-Boot dibagi menjadi beberapa tahap bootloader demi keterbatasan ruang ROM/RAM perangkat. Fase-fase utama adalah:

* **Boot ROM**: Kode boot ROM on-chip pada CPU dijalankan pertama kali setelah power-on, menginisialisasi minimal hardware dan memuat tahap bootloader selanjutnya ke RAM.
* **TPL/SPL**: _Tertiary Program Loader_ (TPL) dan _Secondary Program Loader_ (SPL) adalah binari kecil yang dapat muat di SRAM terbatas. SPL secara khas bertugas menginisialisasi DRAM/SDRAM dan board (misalnya konfigurasi DDR, pemetaan pin), lalu memuat U-Boot utama ke RAM. (Beberapa SoC juga memiliki tahap TPL lebih awal sebelum SPL, misalnya untuk SoC dengan very constrained ROM).
* **U-Boot Utama**: Setelah SPL menyelesaikan awal inisialisasi, U-Boot utama (biasanya berukuran lebih besar) di-relokasi ke RAM dan mulai berjalan. U-Boot ini memuat environment, menginisialisasi subsistem (driver perangkat, jaringan, USB, dsb.), lalu mengeksekusi perintah boot (bootcmd) untuk memuat kernel. U-Boot utama juga menyediakan antarmuka monitor interaktif dan runtime API.

Dokumentasi resmi U-Boot menjelaskan urutan ini: U-Boot dapat mempunyai fase _TPL → SPL → U-Boot utama_, di mana SPL mengatur memori dan memuat binari selanjutnya. Setelah itu U-Boot utama lah yang mengimplementasikan logika pemuatan OS dan perintah bootloader.

#### 1.2 Modularitas dan _Driver Model_

Kode U-Boot bersifat modular: ada kode umum (`common/`), kode spesifik arsitektur (`arch/`), kode spesifik board (`board/`), driver perangkat (`drivers/`), perintah monitor (`cmd/`), sistem file (`fs/`), jaringan (`net/`), utilitas (`lib/`), dan lain-lain. Struktur direktori sumber U-Boot (dari repositori DENX) misalnya meliputi:

* `arch/`, `board/`: kode inisialisasi dan konstanta spesifik CPU/board.
* `cmd/`: implementasi perintah console.
* `drivers/`: driver perangkat keras (flash, MMC, jaringan, dsb.).
* `env/`: pengelola variabel lingkungan (environment).
* `fs/`, `net/`: dukungan sistem file dan protokol jaringan.
* `include/`, `lib/`: header dan pustaka bantu umum.
* `scripts/`, `tools/`: skrip build dan alat bantu (mkimage, dsb.).

Struktur ini dapat dilihat pada repositori U-Boot, misalnya:

```
arch/  board/  cmd/  common/  configs/  drivers/  env/  fs/  include/  lib/  net/  scripts/  tools/ ...
```

. Dokumentasi `README` U-Boot juga menjelaskan topik-topik seperti struktur sumber, konfigurasi Kconfig, dan instruksi build. U-Boot menggunakan sistem build berbasis KConfig/Kbuild (mirip Linux): pemilihan board/defconfig, dan `make CROSS_COMPILE=...` untuk menyusun binari (seperti `u-boot.bin`, `SPL/u-boot-spl.bin`, dsb.) sesuai arsitektur dan hardware target.

#### 1.3 Proses _Build_ U-Boot

Proses kompilasi U-Boot umumnya dilakukan di host PC cross-compiler. Langkah utama adalah:

1. **Clone repositori** U-Boot (git dari _u-boot/u-boot_).
2. **Defconfig**: Pilih konfigurasi board dengan `make <board>_defconfig` (atau `make <board>_defconfig` jika tersedia). Ini membuat konfigurasi default di `.config`.
3. **Kompilasi**: Jalankan `make CROSS_COMPILE=<gcc-prefix>` (mis. `arm-linux-gnueabihf-`) untuk menyusun kode. Hasilnya berupa binari U-Boot (dan SPL jika diaktifkan) pada `./` (biasanya `u-boot.img`, `u-boot.bin`, atau `SPL/u-boot-spl.bin` sesuai opsi).
4. **Mkimage**: Jika menggunakan image uImage/FIT, gunakan `tools/mkimage` untuk membuat header U-Boot di kernel/ramdisk.

Petunjuk lengkap build ada di `README` U-Boot dan dokumen board-specific.

### 2. Proses Kerja U-Boot dari Power-On hingga Kernel

Urutan proses boot di U-Boot secara umum adalah:

1. **Power-On & CPU Startup**: Setelah perangkat di-ON, CPU melakukan reset dan mengeksekusi kode dari **Boot ROM** internal (mask ROM pada SoC). Kode ini menginisialisasi CPU/clock dasar dan memilih sumber boot selanjutnya.
2. **Boot ROM memuat SPL**: Boot ROM membaca SPL (atau U-Boot langsung jika tidak ada SPL) dari media non-volatil (misalnya SPI NOR, NAND, eMMC, dsb.) ke SRAM/RAM.
3. **SPL menyiapkan DRAM**: SPL di RAM melakukan inisialisasi awal seperti setup controller memori (SDRAM), inisialisasi board minimal, lalu memuat U-Boot utama dari flash ke RAM.
4. **U-Boot Utama**: Setelah dipindahkan ke RAM dan di-relokasi, U-Boot utama melakukan inisialisasi penuh: menginisiasi konsol serial, peripheral (Ethernet, USB, MMC, flash, dsb.), memuat variabel lingkungan, dan menampilkan prompt jika diperlukan.
5. **Boot Kernel**: U-Boot kemudian mengeksekusi perintah boot (biasanya dari variabel `bootcmd`) yang memuat kernel ke memori. Ini bisa melalui TFTP (`tftpboot`), dari flash (`bootm`, `bootz` untuk UImage/zImage), atau sumber lain.
6. **Transfer Kontrol ke Kernel**: U-Boot menyiapkan parameter boot (mis. `bootargs`), memuat devicetree (FDT), lalu melompat ke entry-point kernel, menyerahkan kontrol sepenuhnya. Kernel mulai inisialisasi OS.

Menurut Synacktiv, tahapan ini diringkas sebagai berikut: CPU reset → Boot ROM → Bootloader (U-Boot) load → U-Boot init → OS load → OS init. Poin pentingnya: _Boot ROM_ memuat U-Boot (SPL → main U-Boot), U-Boot inisialisasi hardware lebih lanjut dan memuat OS. Setelah kernel siap, U-Boot menyerahkan kontrol pada kernel.

### 3. Fungsi U-Boot dalam Sistem Embedded

U-Boot memiliki berbagai fungsi penting:

* **Bootloader Primier**: Fungsi utama U-Boot adalah sebagai bootloader. Ia menyediakan mekanisme memuat kernel OS dari berbagai media (flash, SD, USB, jaringan) ke RAM dan menjalankannya. Perintah standar seperti `bootm` (boot image U-Boot), `bootz` (boot zImage), `booti` (boot EL2 Image, bagi ARM64), serta dukungan image FIT (Flexible Image Tree) memungkinkan mem-boot kernel Linux, ramdisk, dan devicetree secara fleksibel. U-Boot juga dapat melakukan _bootm_ ke aplikasi lain selain Linux (misalnya VxWorks) dengan format uImage.
*   **Flashing Firmware**: U-Boot sering dipakai untuk upgrade/flash firmware. Ia menyediakan perintah untuk menulis flash internal. Contohnya, untuk flash SPI NOR ada perintah `sf` (SPI flash) seperti `sf probe`, `sf erase`, `sf write`, `sf read`. Untuk flash NAND ada perintah `nand` seperti `nand erase`, `nand write`, `nand read`. Misalnya:

    ```
    => sf probe        # Inisialisasi SPI flash
    => sf erase 0 0x400000   # Hapus 4MB di offset 0
    => sf write 0x82000000 0 0x400000   # Tulis 4MB dari alamat RAM ke flash offset 0
    ```

    Teknik ini memungkinkan pengguna melakukan backup atau upgrade firmware secara in-circuit melalui konsol U-Boot.
* **Debugging dan Maintenance**: U-Boot mempermudah diagnosis perangkat. Di shell U-Boot tersedia perintah memory dump/write: `md` (memory display), `mw`/`mm` (memory write), `cmp` (bandingkan blok memori), serta tes memori (`mtest`). Selain itu, U-Boot mencetak log boot di serial, memungkinkan langkah-langkah inisialisasi di-trace. Pengembang dapat memanfaatkan ini untuk memeriksa apakah perangkat keras diinisialisasi benar. Sebagaimana dicatat, shell U-Boot memungkinkan membaca/mengubah memori, bahkan “membongkar isi flash dan memuat firmware lain” untuk tujuan recovery.
*   **Variabel Lingkungan (Environment)**: U-Boot menggunakan variabel lingkungan (environment variables) untuk mengonfigurasi perilaku boot. Variabel ini disimpan di area flash dan dapat diedit saat run-time. Perintah utamanya adalah `setenv` (atau `env set`) untuk menetapkan variabel, `printenv` (atau `env print`) untuk menampilkan nilai, dan `saveenv` (atau `env save`) untuk menyimpan ke flash. Contohnya:

    ```
    => printenv   # Tampilkan semua variabel
    => setenv ipaddr 192.168.1.100
    => setenv serverip 192.168.1.1
    => saveenv    # Simpan ke flash
    ```

    Variabel `bootcmd` berisi perintah yang dijalankan otomatis saat boot selesai (setelah delay). Variabel lain seperti `bootargs` menentukan argumen kernel, `bootdelay` waktu tunggu, serta variabel jaringan (`ipaddr`, `serverip`) untuk TFTP.
* **Sistem File dan Protokol**: U-Boot mendukung berbagai sistem file: FAT (SD card, USB), ext2/3/4 (SD, eMMC), serta UBI/UBIFS (NAND) dan SQUASHFS (embedded). Ini memungkinkan memuat file kernel/ramdisk dari partisi yang terformat. Juga tersedia dukungan protokol jaringan (TFTP, BOOTP/DHCP, NFS, ping, ARP, dll.) melalui antarmuka Ethernet. U-Boot bahkan dapat menjadi perangkat USB (DFU, USB Mass-Storage/UMS) untuk flashing firmware. Sebagai contoh, perintah `tftpboot` dapat mengunduh image melalui jaringan TFTP, sedangkan `nfs` dapat boot melalui NFS server. Dukungan ini membuat U-Boot sangat fleksibel untuk berbagai skenario.

Ringkasan fungsi di atas terlihat di dokumentasi: U-Boot dilengkapi “network support, USB protocol stack, loading ramdisk, dan implementasi file system (FAT32, ext2/3/4, dst.)”. Selain itu, U-Boot mendukung driver model baru untuk memudahkan penambahan driver (embedded {\`CONFIG\_DM\`} sebagai unified driver model).

### 4. Operasi U-Boot: Interaktif dan Otomatis

U-Boot memiliki antarmuka baris-perintah (prompt) yang dapat digunakan interaktif. Setelah proses SPL, U-Boot utama menampilkan prompt (`=>`) dan menunggu input. Pengguna dapat mengetik perintah secara langsung, atau menekan tombol untuk menghentikan autoboot dan masuk ke prompt.

*   **Interaktif (Prompt)**: Di prompt U-Boot, ketik `help` atau `?` untuk melihat daftar perintah. Contoh perintah umum:

    * `printenv` – tampilkan variabel lingkungan (environment).
    * `setenv VAR value` – set variabel lingkungan. `saveenv` simpan perubahan ke flash.
    * `ping <ip>` – kirim ICMP echo untuk cek koneksi jaringan.
    * `tftpboot <addr> <file>` – unduh file via TFTP ke memori (contoh: `tftpboot 0x82000000 uImage`).
    * `bootm <addr>` / `bootz <addr> [initrd] [fdt]` – jalankan image Linux yang telah dimuat ke memori.
    * `sf read <addr> <offset> <len>` – baca data dari flash SPI ke memori.
    * `sf write <addr> <offset> <len>` – tulis data dari memori ke flash SPI.
    * `nand read <addr> <offset> <len>` – baca dari flash NAND ke memori.
    * `nand write <addr> <offset> <len>` – tulis ke flash NAND.
    * `mmc read/write` – akses MMC/SD/eMMC.
    * `md <addr> <count>` – dump isi memori.
    * `mw <addr> <val> <count>` – tulis nilai ke memori.
    * `run <var>` – jalankan serangkaian perintah dari variabel lingkungan.
    * `bootcmd` – contohnya `=> printenv bootcmd` untuk melihat perintah otomatis.

    U-Boot mendukung shell “hush” yang lebih canggih (mirip shell Unix), dan shell sederhana. SPL biasanya tidak menyertakan shell penuh. Namun, U-Boot utama umumnya menggunakan shell Hush jika diaktifkan, sehingga mendukung scripting dasar.
* **Otomatis (Script Booting)**: U-Boot dapat menjalankan perintah otomatis tanpa interaksi. Variabel khusus `bootcmd` menyimpan perintah yang dieksekusi setelah hitungan mundur `bootdelay` habis. Misalnya `bootcmd=fatload mmc 0 ${loadaddr} uImage; bootm` – akan melakukan boot kernel secara otomatis. Jika pengguna menekan tombol sebelum timer habis, U-Boot akan berhenti di prompt. Selain itu, bisa juga memuat skrip (misal `boot.scr`) dari media dan menjalankannya (`source` atau `run` variabel) untuk automasi kompleks.
* **Contoh Perintah Umum** (tidak eksklusif):
  * Jaringan: `ping`, `tftpboot`, `dhcp`, `nfs`.
  * Flashing: `sf probe/erase/write/read`, `nand erase/write/read`.
  * Filesystem: `fatload` (FAT), `ext4load`, `ubi part/mount`.
  * Debug: `md`, `mw`, `cmp`, `mtest`.
  * Pengelolaan Env: `printenv`, `setenv`, `saveenv`.
  * Boot: `bootm`, `bootz`, `booti`.
  * Lain: `help`, `reset`, `version`.

Referensi _help_ diatas _hal UI Xilinx Wiki_ menunjukkan perintah seperti `printenv`, `setenv`, `sf`, `tftpboot`, dsb. sebagai fungsi bawaan U-Boot.

### 5. Studi Kasus: Arsitektur MIPS (Realtek)

Pada SoC berbasis MIPS (khususnya seri Realtek), U-Boot sering diimplementasikan untuk mengelola flash internal dan partisi. Beberapa poin penting:

*   **Partisi Flash Realtek**: Realtek (mis. platform “PlayonHD” atau router) sering memiliki beberapa partisi MTD pada NAND/SPI flash. Contohnya, sebuah perangkat Realtek mencatat layout MTD:

    ```
    Creating 4 MTD partitions on "rtk_nand":  
    0x00000000-0x09d80000 : "Partition_000"  
    0x09d80000-0x0eb20000 : "/"  
    0x0eb20000-0x0f320000 : "/usr/local/etc"  
    0x0f320000-0x10000000 : "Partition_003"  
    ```

    Artinya U-Boot/OS mempartisi flash NAND menjadi area **Partition\_000** (biasanya bootloader/UTL), partisi root (`/`), partisi konfigurasi (`/usr/local/etc`), dan partisi sisanya. Tabel partisi ini bisa dihardcode atau dihasilkan dinamis oleh U-Boot. Perlu dicatat, skema partisi ini bergantung board: ada partisi untuk U-Boot/Env (sering terletak di awal flash), partisi kernel+rootfs, dsb.
* **Boot dari NAND/SPI**: SoC Realtek umumnya memiliki Boot ROM yang bisa memuat dari SPI NOR (flash serial) atau dari NAND. Saat di-boot, Boot ROM mungkin membaca header pada SPI flash atau header NAND untuk menemukan lokasi U-Boot SPL/main. U-Boot (SPL) kemudian dipindahkan ke RAM dan dieksekusi. Karena kemampuan ROM terbatas (baca cermin kecil ke SRAM), SPL kecil diperlukan. U-Boot utama selanjutnya dapat di-load dari flash ke DRAM. Jika flashnya SPI, penggunaan perintah `sf` di U-Boot (setelah `sf probe`) memungkinkan membaca seluruh flash. Jika flashnya NAND, perintah `nand read` digunakan setelah `nand info`/`nand erase`.
*   **Dumping/Flashing Firmware Praktis**: Dari U-Boot pada perangkat MIPS-Realtek, kita bisa mengekstrak atau menulis ulang firmware. Contohnya:

    *   **Membaca SPI Flash**:

        ```
        => sf probe 0          # Init device SPI flash
        => sf read $loadaddr 0x0 0x400000   # Baca 4MB dari offset 0 ke memori
        ```

        lalu transfer data dari memori ke PC menggunakan TFTP/SFTP (atau `loadb`/kermit) untuk dibackup.
    *   **Tulis SPI Flash**:

        ```
        => sf erase 0x0 0x400000
        => sf write $loadaddr 0x0 $filesize
        ```

        untuk mem-flash image baru (isi `$loadaddr` dapat diisi via TFTP atau USB, mis. `tftpboot` atau `usb start; fatload usb 0:$loadaddr newfw.bin`).
    *   **Baca/Tulis NAND**:

        ```
        => nand info
        => nand read $loadaddr 0x100000 0x600000   # baca 6MB dari offset 0x100000
        => nand erase 0x100000 0x600000 
        => nand write $loadaddr 0x100000 0x600000
        ```

        Perintah `nand read` dan `nand write` memindahkan blok data antara memori dan flash NAND.
    * **Protokol Jaringan**: Misalnya, `tftpboot 0x82000000 firmware.img` mengunduh file firmware.img dari server TFTP ke alamat memori, lalu `sf write 0x82000000 0x0 <ukuran>` mem-flash-nya.

    U-Boot generik menyediakan perintah tersebut di atas. Dengan kemampuan ini, perintah seperti `printenv`/`saveenv` mengelola lingkungan, sedangkan `sf/nand read/write` memungkinkan flashing _in situ_. Pada Realtek MIPS, skema partisi (seperti contoh di atas) dan perintah U-Boot serupa memudahkan proses upgrade dan recovery firmware.

### Ringkasan

U-Boot adalah bootloader modular dan sangat fleksibel untuk perangkat embedded. Arsitekturnya terbagi menjadi SPL/TPL dan U-Boot utama untuk mengatasi batasan memori awal. U-Boot menginisialisasi hardware, menyediakan lingkungan baris perintah dan pengaturan, memuat OS, serta mendukung flashing firmware dan debugging. Dengan variabel lingkungan yang configurable dan dukungan berbagai protokol, U-Boot memudahkan proses boot dan pemeliharaan sistem embedded. Pada arsitektur spesifik seperti MIPS Realtek, U-Boot juga mengelola partisi flash custom (bootloader, kernel, rootfs) dan memberikan utilitas `sf/nand` untuk dump dan flashing firmware.

**Referensi:** Dokumentasi resmi U-Boot dan sumber terkait yang dikutip di atas.
