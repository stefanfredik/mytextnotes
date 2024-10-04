---
description: Memprioritaskan Koneksi Tiktok dari koneksi lainya
---

# Tiktok Priority

## Tangkap IP Tiktok

### List Content

Berikut adalah list content yang biasa digunakan TIktok untuk mendistribusikan konten dan websitenya.

```
.tiktok.com
.tiktokv.com
.tiktokcdn.com
.byteoversea.com
.ibyteimg.com
.ibytedtos.com
.ttwstatic.com
.ttlivecdn.com
.tiktokcdn-us.com
.tiktokcdn-in.com
bytedance.com
```

### Tangkap IP Tiktok menggunakan Firewall Raw

* Tab General :&#x20;
  * Chain : prerouting
  * Src. Add. : network local
  * Dst. Add : (negasi) !network local
* Tab Advanced :&#x20;
  * content : isi dengan list content
* Tab Action :&#x20;
  * Action : add dst to addd list
  * add list : IP Tiktok
  * Timeout : 01:00:00

Bisa disingkat menggunakan script berikut secara berulang :&#x20;

```bash
/ip firewall raw
add comment=IP-Tiktok action=add-dst-to-address-list address-list=IP-Tiktok \
    address-list-timeout=1h chain=prerouting content=\
    .tiktok.com dst-address-list=!IP-Lokal src-address-list=IP-Lokal
add action=add-dst-to-address-list address-list=IP-Tiktok \
    address-list-timeout=1h chain=prerouting content=\
    .tiktokv.com dst-address-list=!IP-Lokal src-address-list=IP-Lokal
add action=add-dst-to-address-list address-list=IP-Tiktok \
    address-list-timeout=1h chain=prerouting content=\
    .tiktokcdn.com dst-address-list=!IP-Lokal src-address-list=IP-Lokal
add action=add-dst-to-address-list address-list=IP-Tiktok \
    address-list-timeout=1h chain=prerouting content=\
    .byteoversea.com dst-address-list=!IP-Lokal src-address-list=IP-Lokal
add action=add-dst-to-address-list address-list=IP-Tiktok \
    address-list-timeout=1h chain=prerouting content=\
    .ibyteimg.com dst-address-list=!IP-Lokal src-address-list=IP-Lokal
add action=add-dst-to-address-list address-list=IP-Tiktok \
    address-list-timeout=1h chain=prerouting content=\
    .ibytedtos.com dst-address-list=!IP-Lokal src-address-list=IP-Lokal
add action=add-dst-to-address-list address-list=IP-Tiktok \
    address-list-timeout=1h chain=prerouting content=\
    .ttwstatic.com dst-address-list=!IP-Lokal src-address-list=IP-Lokal
add action=add-dst-to-address-list address-list=IP-Tiktok \
    address-list-timeout=1h chain=prerouting content=\
    .ttlivecdn.com dst-address-list=!IP-Lokal src-address-list=IP-Lokal
add action=add-dst-to-address-list address-list=IP-Tiktok \
    address-list-timeout=1h chain=prerouting content=\
    .tiktokcdn-us.com dst-address-list=!IP-Lokal src-address-list=IP-Lokal
add action=add-dst-to-address-list address-list=IP-Tiktok \
    address-list-timeout=1h chain=prerouting content=\
    .tiktokcdn-in.com dst-address-list=!IP-Lokal src-address-list=IP-Lokal
add action=add-dst-to-address-list address-list=IP-Tiktok \
    address-list-timeout=1h chain=prerouting content=\
    bytedance.com dst-address-list=!IP-Lokal src-address-list=IP-Lokal
```



## Membuat Connection Mark Tiktok

Masuk ke menu IP Firewall - Mangle

```git
Tab General
    - Chain: prerouting
Tab Advanced
    - Src. Address List: IP-Local
    - Dst. Address List: IP-Tiktok
Tab Action
    - Action: mark-connection
    - New Connection Mark: Koneksi-Tiktok
    - Passthrough: checklist
```



## Membuat Packet Mark

IP - Firewall - Mangle

```markup
Tab General
    - Chain: forward
    - Connection Mark: Koneksi-Tiktok
Tab Action
    - Action: mark-packet
    - New Packet Mark: Paket-Tiktok
    - Passthrough: unchecklist
```



## Mengarahkan Trafik Tiktok ke ISP Lain (Routing)

Routing merupakan teknik untuk mengarahkan trafik tertentu ke salah satu isp, dalam kesempatan kali ini kita akan mencoba untuk me-routing trafik tiktok ke isp lain.

Sebelum itu, pastikan teman-teman sudah membuat rule **Menangkap IP Tiktok dengan Content RAW Domain**

Masuk ke Menu **IP > Firewall > Mangle** > (+) Add Rule

```git
Tab General
- Chain: prerouting
Tab Advanced
- Src. Address List: IP-Local
- Dst. Address List: IP-Tiktok
Tab Action
- Action: mark-routing
- New Routing Mark: Trafik-Tiktok
- Passthrough: checklist
Apply > OK
```

Setelah rule diatas. Masuk ke Menu **IP > Routes** > (+) Add Rule

```git
Tab General
- Dst. Address: 0.0.0.0/0
- Gateway: isi dengan IP Address ISP, misal 192.168.2.1
- Check Gateway: ping
- Distance: 2
- Routing Mark: Trafik-Tiktok
Apply > OK
```

#### Kesimpulan <a href="#kesimpulan" id="kesimpulan"></a>

Sekian artikel mengenai RAW Content Tiktok untuk Pisah Trafik Mikrotik, Anda bisa menerapkan raw content tersebut untuk keperluan pisah trafik maupun routing ke salah satu isp. Jika kesulitan mempraktikan tutorial ini, ayo pake aja yang simpel! gunakan [Jasa Setting Mikrotik by Mikrotiknesia](https://mikrotiknesia.com/jasa-setting-mikrotik/). Terima beres pokoknya hehehe
