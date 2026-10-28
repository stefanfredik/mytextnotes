# VPN

## Create IKEv2 IPSEC VPN

Tentu! Saya akan membantu Anda mengonfigurasi VPN IKEv2-PSK di MikroTik versi 6.48 agar bisa terhubung dengan perangkat Android.

### **Langkah-Langkah Konfigurasi IKEv2-PSK di MikroTik**

#### **1. Aktifkan IKEv2 di IPsec**

Buka MikroTik Anda melalui Winbox atau Terminal, lalu jalankan perintah berikut:

```sh
/ip ipsec profile add name=ikev2_profile hash-algorithm=sha256 enc-algorithm=aes-256,aes-128 dh-group=modp2048
/ip ipsec peer add name=ikev2_peer address=0.0.0.0/0 exchange-mode=ike2 passive=yes profile=ikev2_profile
/ip ipsec identity add peer=ikev2_peer auth-method=pre-shared-key secret="password-psk" generate-policy=port-override
```

Gantilah `"password-psk"` dengan kata sandi yang kuat.

#### **2. Konfigurasi Proposal & Policy**

Tambahkan proposal yang digunakan untuk enkripsi dan autentikasi:

```sh
/ip ipsec proposal add name=ikev2_proposal auth-algorithms=sha256 enc-algorithms=aes-256-cbc lifetime=30m pfs-group=modp2048
```

Tambahkan policy agar VPN bisa berjalan:

```sh
/ip ipsec policy add src-address=0.0.0.0/0 dst-address=0.0.0.0/0 sa-src-address=YOUR_ROUTER_PUBLIC_IP sa-dst-address=0.0.0.0 action=encrypt level=require proposal=ikev2_proposal
```

Gantilah `YOUR_ROUTER_PUBLIC_IP` dengan IP publik MikroTik Anda.

#### **3. Tambahkan Mode Config untuk Alokasi IP Client**

Mode Config digunakan untuk memberikan IP ke client VPN:

```sh
/ip ipsec mode-config add name=ikev2_mode config address-pool=ikev2_pool system-dns=yes
```

Tambahkan pool untuk memberikan IP ke client:

```sh
/ip pool add name=ikev2_pool ranges=192.168.100.10-192.168.100.100
```

#### **4. Tambahkan Firewall NAT agar Client Bisa Akses Internet**

Jalankan perintah ini untuk mengizinkan client VPN mengakses internet:

```sh
/ip firewall nat add chain=srcnat src-address=192.168.100.0/24 action=masquerade
```

#### **5. Konfigurasi di Android**

1. Buka **Pengaturan > Jaringan & Internet > VPN**.
2. Pilih **Tambahkan VPN**.
3. Masukkan:
   * **Nama**: (Bebas)
   * **Tipe**: IKEv2/IPSec PSK
   * **Server**: (IP Publik MikroTik Anda)
   * **IPSec Prasyarat Kunci**: (Gunakan password PSK yang sudah dikonfigurasi)
   * **ID IPSec**: (Bisa dikosongkan atau gunakan IP router)
4. Simpan dan sambungkan.

#### **6. Uji Koneksi**

Coba hubungkan perangkat Android Anda ke VPN yang sudah dibuat. Jika mengalami masalah, cek log di MikroTik dengan:

```sh
/log print where message~"ipsec"
```

Jika ada kesalahan atau ingin tambahan fitur seperti user-based authentication, beri tahu saya! 😊

