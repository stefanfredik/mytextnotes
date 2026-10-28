# Routing

## **Policy-Based Routing (PBR)**

***

### 🎯 **Mengatur agar IP client tertentu (misalnya 192.168.88.10) atau netwrk tertentu diarahkan ke gateway tertentu (misalnya 192.168.100.2), namun tetap bisa berkomunikasi normal dengan jaringan lokal (LAN)**

***

### 🧱 ILUSTRASI TOPOLOGI

```
                             INTERNET
                                ↑
                            2 Gateway
                        +------------+
                        | MikroTik   |
                        |            |
        LAN <---------->| ether2: 192.168.88.1/24
                        | ether1: 192.168.100.1 (GW-A, default)
                        | ether3: 192.168.100.2 (GW-B, khusus)
                        +------------+

Client: 192.168.88.10 → harus keluar lewat GW-B
```

***

### 🧠 MASALAH YANG DIHINDARI

Jika kita **ganti default gateway client ke 192.168.100.2**, maka client **tidak bisa akses LAN lainnya**.

**Solusi terbaik:** Gunakan **Policy Based Routing (PBR)** di MikroTik untuk **hanya arahkan trafik internet keluar**, tanpa mengganggu komunikasi LAN.

***

### ✅ LANGKAH-LANGKAH SETUP PBR UNTUK IP TERTENTU

#### 1️⃣ Buat `routing-mark` via Mangle untuk IP Client

```bash
/ip firewall mangle
add chain=prerouting src-address=192.168.88.10 dst-address=!192.168.88.0/24 \
    action=mark-routing new-routing-mark=use_gw_b passthrough=yes
```

#### 📌 Penjelasan:

| Field                          | Fungsi                                                     |
| ------------------------------ | ---------------------------------------------------------- |
| `src-address=192.168.88.10`    | Client yang diatur                                         |
| `dst-address=!192.168.88.0/24` | Artinya **kecuali** jaringan LAN → supaya LAN tetap normal |
| `mark-routing=use_gw_b`        | Tandai trafik yang nanti diarahkan ke route khusus         |
| `passthrough=yes`              | Biarkan rule mangle lainnya tetap berjalan                 |

***

#### 2️⃣ Tambahkan Route Khusus Berdasarkan Routing-Mark

```bash
/ip route
add dst-address=0.0.0.0/0 gateway=192.168.100.2 routing-mark=use_gw_b \
    comment="Routing untuk IP client via GW-B"
```

> Hanya trafik dengan `routing-mark=use_gw_b` yang akan pakai gateway ini.

***

#### 3️⃣ Default Route Tetap Ada

Pastikan kamu tetap punya default route normal untuk klien lainnya:

```bash
/ip route
add dst-address=0.0.0.0/0 gateway=192.168.100.1 comment="Default GW untuk umum"
```

***

### 🔍 VERIFIKASI

Dari MikroTik:

```bash
/tool traceroute src-address=192.168.88.10 8.8.8.8
```

Dari client:

* Buka `https://whatismyipaddress.com` dan pastikan keluar lewat IP gateway B.
* Coba ping ke IP lain di LAN seperti `192.168.88.1` → harus tetap bisa.

***

### 📌 CATATAN PENTING

| Tips                                   | Keterangan                                                      |
| -------------------------------------- | --------------------------------------------------------------- |
| `connection-tracking` aktif            | Cek via: `/ip firewall connection tracking`                     |
| Jangan ubah default gateway client     | Biarkan pakai `192.168.88.1`, biar router yang atur arah keluar |
| dst-address=!192.168.88.0/24           | Ini yang memastikan komunikasi LAN **tidak dialihkan**          |
| Bisa diterapkan per user, subnet, VLAN | Cukup ganti `src-address` atau pakai `address-list`             |

***

### 🧠 BONUS: Untuk Banyak IP Client

Gunakan address-list:

```bash
/ip firewall address-list
add address=192.168.88.10 list=via_gw_b
add address=192.168.88.20 list=via_gw_b

/ip firewall mangle
add chain=prerouting src-address-list=via_gw_b dst-address=!192.168.88.0/24 \
    action=mark-routing new-routing-mark=use_gw_b passthrough=yes
```

### Masih Bisa Diakses dari Network Local

Pertanyaan sangat penting, Fred — dan ini sering menjadi **jebakan umum saat menerapkan PBR (Policy-Based Routing)** di MikroTik.

***

### 🎯 Masalah

Setelah kita **mengarahkan IP client tertentu keluar lewat gateway khusus (GW-B)**, kadang:

> ❌ Klien lain di LAN/VLAN yang berbeda **tidak bisa komunikasi** ke klien yang diarahkan (mis. `192.168.88.10`)\
> ✅ Tapi `192.168.88.10` bisa mengakses klien lain

***

### 🧠 Penyebab

Masalahnya adalah **asymmetric routing**:

1. Klien A (`192.168.88.20`) kirim paket ke `192.168.88.10` → **via MikroTik**.
2. MikroTik terima dan teruskan ke `192.168.88.10` → OK.
3. Tapi **balasan dari `192.168.88.10` diarahkan keluar lewat GW-B** (karena kena rule PBR).
4. Jadi, koneksi dianggap **tidak sah atau timeout**.

> Ini karena mangle `routing-mark` dibuat terlalu **agresif**, sehingga semua paket keluar (termasuk balasan komunikasi internal) diarahkan ke internet.

***

### ✅ SOLUSI

#### 🎯 **Batasi mangle rule hanya untuk trafik yang benar-benar keluar WAN**

***

#### 🔧 LANGKAH PERBAIKAN

#### 1️⃣ Gunakan `out-interface` untuk hanya tandai trafik keluar WAN

```bash
/ip firewall mangle
add chain=prerouting src-address=192.168.88.10 dst-address=!192.168.88.0/24 \
    out-interface=ether1 action=mark-routing new-routing-mark=use_gw_b passthrough=yes
```

> Ganti `ether1` dengan interface WAN kamu yang mengarah ke gateway B.

***

#### 2️⃣ ATAU Lebih Aman Lagi: Gunakan `dst-address-type=local` untuk mengecualikan komunikasi internal

```bash
/ip firewall mangle
add chain=prerouting src-address=192.168.88.10 dst-address-type=!local \
    action=mark-routing new-routing-mark=use_gw_b passthrough=yes
```

> Ini akan mengecualikan semua trafik ke **interface lokal router**, termasuk LAN & VLAN lain, dan hanya tandai trafik yang benar-benar keluar.

***

#### 3️⃣ Pastikan tidak ada NAT berlebihan untuk trafik lokal

Cek rule NAT kamu:

```bash
/ip firewall nat
```

Pastikan **tidak ada masquerade atau src-nat** yang memaksa antar subnet LAN.

***

#### 4️⃣ Gunakan traceroute dan connection tracking untuk debug

```bash
/tool traceroute 192.168.88.10
/ip firewall connection print where dst-address~"192.168.88.10"
```

> Pastikan koneksi antar internal **tidak tertandai sebagai koneksi dengan routing-mark**.

***

### 🧪 TESTING

1. Dari klien biasa (`192.168.88.20`), ping ke `192.168.88.10` → ✅ harus bisa.
2. Dari `192.168.88.10`, ping ke 8.8.8.8 → ✅ keluar lewat gateway B.
3. Pastikan `192.168.88.10` tetap bisa komunikasi LAN.

***

### 🧠 KESIMPULAN

| Masalah Umum                       | Solusi                                                                   |
| ---------------------------------- | ------------------------------------------------------------------------ |
| Client tidak bisa diakses dari LAN | Perbaiki rule mangle agar hanya tandai trafik keluar (bukan semua paket) |
| Asymmetric routing                 | Hindari menandai semua trafik, cukup tandai trafik keluar WAN saja       |
| Tidak bisa komunikasi VLAN lain    | Gunakan `dst-address-type=!local` agar hanya internet yang ditandai      |

***

Kalau kamu pakai VLAN, VRF, atau PPPoE antar pelanggan di CHR Proxmox, pendekatan ini bisa diadaptasi dengan sangat presisi.

Mau saya bantu buat skrip otomatisasi untuk scenario multi-client multi-gateway per VLAN/PPPoE?
