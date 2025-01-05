# Upgrade Linux LTS

Berikut langkah-langkah untuk meng-upgrade Ubuntu 22.04 LTS ke Ubuntu 24.04 LTS:

***

#### **Persiapan Sebelum Upgrade**

1. **Backup Data**\
   Pastikan semua data penting telah dibackup untuk menghindari kehilangan data jika terjadi masalah selama proses upgrade.
2.  **Update Semua Paket di Ubuntu 22.04 LTS**\
    Jalankan perintah berikut untuk memastikan sistem Anda memiliki semua pembaruan terbaru:

    ```bash
    sudo apt update && sudo apt upgrade -y
    sudo apt dist-upgrade -y
    ```

    Pastikan juga semua paket yang tidak dibutuhkan telah dihapus:

    ```bash
    sudo apt autoremove --purge -y
    sudo apt autoclean
    ```
3.  **Pastikan Paket `update-manager-core` Terinstal**\
    Periksa dan instal paket `update-manager-core` jika belum ada:

    ```bash
    sudo apt install update-manager-core
    ```
4.  **Nonaktifkan Repository Pihak Ketiga**\
    Nonaktifkan PPA (Personal Package Archives) atau repository pihak ketiga dengan mengedit file:

    ```bash
    sudo nano /etc/apt/sources.list.d/<nama-ppa>.list
    ```

    Tambahkan tanda komentar `#` di depan setiap baris yang berasal dari PPA.

***

#### **Upgrade ke Ubuntu 24.04 LTS**

1.  **Ubah Preferensi Upgrade ke Rilis LTS**\
    Pastikan file konfigurasi `/etc/update-manager/release-upgrades` diatur ke `lts`:

    ```bash
    sudo nano /etc/update-manager/release-upgrades
    ```

    Ubah bagian `Prompt` menjadi:

    ```
    Prompt=lts
    ```
2.  **Jalankan Proses Upgrade**\
    Jalankan perintah berikut untuk memulai upgrade:

    ```bash
    sudo do-release-upgrade
    ```

    Jika upgrade tidak tersedia langsung, gunakan flag `-d`:

    ```bash
    sudo do-release-upgrade -d
    ```
3. **Ikuti Petunjuk Selama Proses**
   * Proses upgrade akan memakan waktu cukup lama tergantung pada koneksi internet dan spesifikasi komputer Anda.
   * Jika ada prompt mengenai penghapusan paket atau penimpaan konfigurasi, baca dengan cermat sebelum memilih.
4.  **Restart Sistem**\
    Setelah proses upgrade selesai, sistem akan meminta Anda untuk melakukan restart:

    ```bash
    sudo reboot
    ```

***

#### **Setelah Upgrade**

1.  **Periksa Versi Ubuntu**\
    Pastikan sistem Anda telah berhasil di-upgrade ke Ubuntu 24.04 LTS:

    ```bash
    lsb_release -a
    ```

    Atau:

    ```bash
    cat /etc/os-release
    ```
2.  **Aktifkan Kembali Repository Pihak Ketiga (Jika Diperlukan)**\
    Jika Anda menonaktifkan repository pihak ketiga, aktifkan kembali dengan menghapus tanda komentar di file `sources.list.d/<nama-ppa>.list` lalu update:

    ```bash
    sudo apt update
    ```
3. **Periksa Aplikasi dan Layanan**\
   Pastikan semua aplikasi dan layanan berjalan dengan normal.

***

Jika ada masalah selama proses upgrade, beri tahu saya untuk bantuan lebih lanjut! 😊
