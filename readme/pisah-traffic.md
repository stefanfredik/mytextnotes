# Pisah Traffic

## Shortcut

Untuk mempersingkat setingan bisa menggunakan script berikut

### Address list

Menambahkan IP  List Local

```
/ip firewall address-list
add address=192.168.0.0/16 list=private-lokal
add address=10.10.0.0/16 list=private-lokal
```

### Raw

Menambah IP List menggunakan Raw firewall&#x20;

#### Youtube

```bash
/ip firewall raw
add action=add-dst-to-address-list address-list=YOUTUBE address-list-timeout=\
    10m chain=prerouting content=googlevideo.com dst-address-list=\
    !private-lokal src-address-list=private-lokal
    
add action=add-dst-to-address-list address-list=YOUTUBE address-list-timeout=\
    10m chain=prerouting content=ytimg.com dst-address-list=!private-lokal \
    src-address-list=private-lokal
    
add action=add-dst-to-address-list address-list=YOUTUBE address-list-timeout=\
    10m chain=prerouting content=youtube.com dst-address-list=!private-lokal \
    src-address-list=private-lokal
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

### Mangle

#### Game

```bash
/ip firewall mangle
add action=mark-connection chain=prerouting comment=GAME dst-port="!0-1023,119\
    4,1723,1935,2083,3478,5050-5061,6666,8777,8000-8081,35915,39397" \
    new-connection-mark=GAME passthrough=yes protocol=tcp src-address-list=\
    private-lokal
    
add action=mark-connection chain=prerouting dst-port=!0-1023,1701,5060,5061 \
    new-connection-mark=GAME passthrough=yes protocol=udp src-address-list=\
    private-lokal
    
add action=mark-packet chain=prerouting connection-mark=GAME new-packet-mark=\
    "GAME PAKET" passthrough=yes
    

```

#### Youtube

```bash
add action=mark-connection chain=prerouting comment=youtube dst-address-list=\
    YOUTUBE new-connection-mark=YOUTUBE passthrough=yes protocol=!icmp \
    src-address-list=private-lokal
    
add action=mark-packet chain=prerouting connection-mark=YOUTUBE \
    new-packet-mark="YOUTUBE PAKET" passthrough=yes
    
add action=mark-connection chain=prerouting comment=umum connection-mark=\
    no-mark new-connection-mark=UMUM passthrough=yes
    
add action=mark-packet chain=prerouting connection-mark=UMUM new-packet-mark=\
    "UMUM PAKET" passthrough=yes
```

#### Tiktok

```bash
add action=mark-connection chain=prerouting comment=youtube dst-address-list=\
    YOUTUBE new-connection-mark=TIKTOK passthrough=yes protocol=!icmp \
    src-address-list=private-lokal
    
add action=mark-packet chain=prerouting connection-mark=YOUTUBE \
    new-packet-mark="TIKTOK PAKET" passthrough=yes

```

#### Umum

```bash
add action=mark-connection chain=prerouting comment=umum connection-mark=\
    no-mark new-connection-mark=UMUM passthrough=yes
    
add action=mark-packet chain=prerouting connection-mark=UMUM new-packet-mark=\
    "UMUM PAKET" passthrough=yes
```

### Simple Queue



```bash
#Global
/queue simple add name=GLOBAL target=192.168.0.0/16,10.10.0.0/16

#Game
add name=GAME packet-marks="GAME PAKET" parent=GLOBAL target=\
    192.168.0.0/16
    
add name=YOUTUBE packet-marks="YOUTUBE PAKET" parent=GLOBAL target=\
    192.168.0.0/16
    
add name=YOUTUBE packet-marks="TIKTOK PAKET" parent=GLOBAL target=\
    192.168.0.0/16
    
add name=UMUM packet-marks="UMUM PAKET" parent=GLOBAL target=\
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
