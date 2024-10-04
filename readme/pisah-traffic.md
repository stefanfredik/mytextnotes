---
description: >-
  Berikut adalah cara memisahkan traffic seusai kebutuhan pada mikrotik dan
  mebatasi limitasi bandwith setiap traffic yang sudah dipisahkan.
---

# Pisah Traffic

## Via Winbox

### Address List

IP - Firewall - Address List

Tambah address List

* Name : IP Local
*   Address :&#x20;

    * 192.168.0.0/16
    * 10.10.0.0/16



### Menangkap IP Content menggunakan RAW

IP - FIrewall - RAW

#### Content

Untuk list kontent bisa dilihat pada linnk berikut :&#x20;

{% content-ref url="content.md" %}
[content.md](content.md)
{% endcontent-ref %}

***

**Tab General**

* Chain : prerouting

**Tab Advanced**

* Source Address List : IP-Local
* Destination Address List : (negasi) !IP-Local
* Content : googlevideo.com

**Tab Action**

* Action : add destination to address list
* Timeout : 00:10:00&#x20;

***

Ulangi langkah di atas satu-satu  dengan mengganti content sesuai content yang ada list.&#x20;





## Shortcut - Via Script

Untuk mempersingkat setingan bisa menggunakan script berikut

### Address list

Menambahkan IP  List Local

```bash
/ip firewall address-list
add address=192.168.0.0/16 list=IP-Local
add address=10.10.0.0/16 list=IP-Local
```

### Raw

Menambah IP List menggunakan Raw firewall&#x20;

#### Youtube

```bash
/ip firewall raw
add action=add-dst-to-address-list address-list=IP-Youtube address-list-timeout=\
    10m chain=prerouting content=googlevideo.com dst-address-list=\
    !IP-Local src-address-list=IP-Local
    
add action=add-dst-to-address-list address-list=IP-Youtube address-list-timeout=\
    10m chain=prerouting content=ytimg.com dst-address-list=!IP-Local \
    src-address-list=IP-Local
    
add action=add-dst-to-address-list address-list=IP-Youtube address-list-timeout=\
    10m chain=prerouting content=youtube.com dst-address-list=!IP-Local \
    src-address-list=IP-Local
```

#### Tiktok

Tiktok Content

```bash
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

```bash
/ip firewall raw
add comment=IP-Tiktok action=add-dst-to-address-list address-list=IP-Tiktok \
    address-list-timeout=1h chain=prerouting content=\
    .tiktok.com dst-address-list=!IP-Local src-address-list=IP-Local
    
add action=add-dst-to-address-list address-list=IP-Tiktok \
    address-list-timeout=1h chain=prerouting content=\
    .tiktokv.com dst-address-list=!IP-Local src-address-list=IP-Local
    
add action=add-dst-to-address-list address-list=IP-Tiktok \
    address-list-timeout=1h chain=prerouting content=\
    .tiktokcdn.com dst-address-list=!IP-Local src-address-list=IP-Local
    
add action=add-dst-to-address-list address-list=IP-Tiktok \
    address-list-timeout=1h chain=prerouting content=\
    .byteoversea.com dst-address-list=!IP-Local src-address-list=IP-Local
    
add action=add-dst-to-address-list address-list=IP-Tiktok \
    address-list-timeout=1h chain=prerouting content=\
    .ibyteimg.com dst-address-list=!IP-Local src-address-list=IP-Local
    
add action=add-dst-to-address-list address-list=IP-Tiktok \
    address-list-timeout=1h chain=prerouting content=\
    .ibytedtos.com dst-address-list=!IP-Local src-address-list=IP-Local
    
add action=add-dst-to-address-list address-list=IP-Tiktok \
    address-list-timeout=1h chain=prerouting content=\
    .ttwstatic.com dst-address-list=!IP-Local src-address-list=IP-Local
    
add action=add-dst-to-address-list address-list=IP-Tiktok \
    address-list-timeout=1h chain=prerouting content=\
    .ttlivecdn.com dst-address-list=!IP-Local src-address-list=IP-Local
    
add action=add-dst-to-address-list address-list=IP-Tiktok \
    address-list-timeout=1h chain=prerouting content=\
    .tiktokcdn-us.com dst-address-list=!IP-Local src-address-list=IP-Local
    
add action=add-dst-to-address-list address-list=IP-Tiktok \
    address-list-timeout=1h chain=prerouting content=\
    .tiktokcdn-in.com dst-address-list=!IP-Local src-address-list=IP-Local
    
add action=add-dst-to-address-list address-list=IP-Tiktok \
    address-list-timeout=1h chain=prerouting content=\
    bytedance.com dst-address-list=!IP-Local src-address-list=IP-Local
```

### Mangle

#### Game

```bash
/ip firewall mangle
add action=mark-connection chain=prerouting comment=GAME dst-port="!0-1023,119\
    4,1723,1935,2083,3478,5050-5061,6666,8777,8000-8081,35915,39397" \
    new-connection-mark=GAME passthrough=yes protocol=tcp src-address-list=\
    IP-Local
    
add action=mark-connection chain=prerouting dst-port=!0-1023,1701,5060,5061 \
    new-connection-mark=GAME passthrough=yes protocol=udp src-address-list=\
    IP-Local
    
add action=mark-packet chain=prerouting connection-mark=GAME new-packet-mark=\
    "GAME PAKET" passthrough=yes
    
```

#### Youtube

```bash
add action=mark-connection chain=prerouting comment=youtube dst-address-list=\
    IP-Youtube new-connection-mark=Youtube-conn passthrough=yes protocol=!icmp \
    src-address-list=IP-Local
    
add action=mark-packet chain=prerouting connection-mark=Youtube-conn \
    new-packet-mark="Youtube-Packet" passthrough=yes

```

#### Tiktok

```bash
add action=mark-connection chain=prerouting comment=youtube dst-address-list=\
    IP-Tiktok new-connection-mark=Tiktok-conn passthrough=yes protocol=!icmp \
    src-address-list=IP-Local
    
add action=mark-packet chain=prerouting connection-mark=Tiktok-conn \
    new-packet-mark="Tiktok-packet" passthrough=yes

```

#### Umum

```bash
add action=mark-connection chain=prerouting comment=umum connection-mark=\
    no-mark new-connection-mark=Common-conn passthrough=yes
    
add action=mark-packet chain=prerouting connection-mark=Common-conn new-packet-mark=\
    "Common-packet" passthrough=yes
```

### Simple Queue

```bash
#Global
/queue simple add name=Global target=192.168.0.0/16,10.10.0.0/16

#Game
add name=Game packet-marks="Game-packet" parent=Global target=\
    192.168.0.0/16

#Youtube
add name=Youtube packet-marks="Youtube-packet" parent=Global target=\
    192.168.0.0/16

#Tiktok    
add name=Tiktok packet-marks="Tiktok-packet" parent=Global target=\
    192.168.0.0/16

#Umum    
add name=Common packet-marks="Common-packet" parent=Global target=\
    192.168.0.0/16
```

### Hotspot Profile

```bash
ON LOGIN
/queue simple add name="game-$address" targ=$address max-limit=1M/1M comment=$user place-befo=GAME parent=GAME;
/queue simple add name="youtube-$address" targ=$address max-limit=2M/2M comment=$user place-befo=YOUTUBE parent=YOUTUBE;

ON LOGOUT
/queue simple remove [find comment=$user];
```
