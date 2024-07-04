---
description: >-
  Berisi catatan perintah-perintah yang sering digunakan pada OLT ZTE C320 dalam
  melakukan pengecekan dan kongfigurasi jaringan.
---

# 🖥️ ZTE C320 OLT

Untuk panduan lengkapnya bisa di download di ebook berikut.&#x20;

{% file src="../.gitbook/assets/ZTE C320 OLT Manual Book.pdf" %}

## Show Config

### Show Bandwith Profile

Menampilakan profil bandwith yang sudah di set sebelumnya.

```bash
sh gpon profile traffic
```

### Show uncofig ONU&#x20;

Mengecek ONU yang belum terdaftar.

```bash
show gpon onu uncfg
```

### Show **Utilisasi Bandwith Link ONT Real Time**

```bash
show interface gpon-onu_[port]
```

### Show **Mac Address Perangkat yang terkoneksi ke ONT**

```bash
sho mac gpon onu gpon-onu_[port onu]
```

### Show Ont Version

Menampilkan informasi detail  versi dari ONT.

```bash
sho gpon remote-onu equip gpon-onu_[port]
```

### Show ONT IP

Menampilkan IP  host yang terdapat di ONT.

```bash
sho gpon remote-onu ip-host gpon-onu_[port]
```

### Show Ont Port Status

Menampilkan informasi detail PORT ONT.

```bash
sho gpon remote-onu interface eth gpon-onu_[port]
```

### **Cek Detail Config di Interface ke arah ONT**

```bash
show run interface gpon-onu_[port]
```

### **Cek Status Interface ONT**

```bash
show gpon onu state gpon-olt_1/2/5
```

### **Cek Nilai Optik ONT**

```bash
show pon power attenuation gpon-onu_1/2/5:4      

```

### **Cek Utilisasi Bandwith Link ONT Real Time**

```bash
show interface gpon-onu_1/2/5:4
```

### **Cek ONT Uptime dan History**

Menampilkan informasi detail tentang log , history serta dan  status ONT.

```bash
OLT-ZTE_C320#show gpon onu detail-info gpon-onu_1/2/5:5
```

### **Cek Mac Address Perangkat yang terkoneksi ke ONT**

Menampilkan mac address pada dari ONT.

```bash
show mac gpon onu gpon-onu_[port]
```

### **Cek Status Port di ONT**

Menampilkan informasi detail tentang port Lan pada ONT.

```bash
show gpon remote-onu interface eth gpon-onu_[port]
```

### Reboot/Restart ONT

Restart ONT dari OLT

```bash
config t
pon-onu
pon-onu-mng gpon-onu_[port]
reboot
```

### **Shutdown Port 1 di ONT**

Disable port lan pada ONT.

```bash
config t
pon-onu
pon-onu-mng gpon-onu_1/2/5:5
interface eth eth_0/1 state lock
```

### **Cek Konfigurasi PON ONU**

Menampilkan kongfigurasi ONT

```bash
show onu running config gpon-onu_1/2/11:8
```

Hasil

```bash
pon-onu-mng gpon-onu_1/2/11:8
  service Internet-Customer1 gemport 1 vlan 671
  service OAM gemport 2 vlan 1450
  service Internet-Customer2 gemport 3 vlan 1490
  vlan port eth_0/1 mode tag vlan 671
  vlan port eth_0/3 mode tag vlan 1490
```

### Check OLT Uptime

Mengecek uptime dari OLT.

```bash
show system-group

System Description: ZX Version V4.
Started before: 47484 days, 5 hours, 25 minutes
```

### Check OLT Board/ Card

Menampilkan informasi board/ card yang terpasang pada OLT.

```bash
sho card
```

```bash
GTGOE = 8 Port (O=Octal)
GTGH = 16 Port (H=Hexa
```

### Check OLT Processor dan Memory

Menampilkan informasi CPU dan memory yang sedang digunakan.

```bash
show processor
```

### Check OLT Temperature

```bash
show temperature
```

### Checking OLT per Slot

Menampilkan informasi detail dari Card/Slot pada OLT.

```bash
show card slotno 12
```

### Finding Ont by Serial Number

Mencari port onu dari ONT menggunakan serial number.

```bash
sho gpon onu by sn ABCDEFGHIJ
```

### Checking ONT status on OLT Port

Menampilkan status ONT per ONT

```bash
sho gpon onu state gpon-olt_[port] 2
```

Menampilkan semua status ONT pada&#x20;

```bash
sho gpon onu state gpon-olt_[port]
```

### Checking Ont Mac Address

Menampilkan mac ONT pada setiap WAN network atau Vlan

```bash
sho mac gpon onu gpon-onu_[port]
```

### Checking ONT Power/ Redaman

Menampilkan informasi redaman kabel pada ONT.

```bash
sho pon power attenuation gpon-onu_[port]
```

### Checking Ont Version

Menampilkan detail informasi versi dari ONT.

```bash
sho gpon remote-onu equip gpon-onu_[port]
```

### Checking Ont IP (Routing Mode)

Menampilkan informasi IP  yang terdapat pada setingan ONT.

```bash
sho gpon remote-onu ip-host gpon-onu_[port]
```

### Checking ONT Port Status

```bash
sho gpon remote-onu interface eth gpon-onu_[port]
```

### Checking ONT Configuration

#### Config 1

```bash
sho run int gpon-onu_1/4/1:
```

```bash
Building configuration…
!
interface gpon-onu_1/4/1:2
descriptionIRONMAN
tcont 1 profile share
gemport 1 name ngenet unicast tcont 1 dir both
gemport 1 traffic-limit downstream 300m
gemport 2 name nontonfelm unicast tcont 1 dir both
gemport 2 traffic-limit downstream 300m
gemport 3 name tilpun unicast tcont 1 dir both
gemport 3 traffic-limit downstream 300m
switchport mode hybrid vport 1
switchport vlan 212  tag vport 1
switchport mode hybrid vport 2
switchport vlan 121  tag vport 2
switchport mode hybrid vport 3
switchport vlan 122  tag vport 3
!
end
```

#### Config 2

```bash
sho onu run con gpon-onu_1/4/1:2
```

```bash
pon-onu-mng gpon-onu_1/4/1:2
flow 2 switch switch_0/1
flow 3 switch switch_0/1
flow mode 1 tag-filter vid-filter untag-filter discard
flow mode 2 tag-filter vid-filter untag-filter discard
flow mode 3 tag-filter vid-filter untag-filter discard
flow 1 priority 0 vid 212
flow 2 priority 0 vid 121
flow 3 priority 0 vid 122
gemport 1 flow 1
gemport 2 flow 2
gemport 3 flow 3
switchport-bind switch_0/1 iphost 1
ip-host 1 dhcp-enable true ping-response true traceroute-response true
vlan-filter-mode iphost 1 tag-filter vid-filter untag-filter discard
vlan-filter iphost 1 priority 0 vid 212
vlan port eth_0/2 mode tag vlan 121
vlan port eth_0/3 mode tag vlan 122
dhcp-ip ethuni eth_0/2 from-internet –> bridging mode
dhcp-ip ethuni eth_0/3 from-internet –> bridging mode
```

## Configuration

### Reboot ONT

Restart ONT dari OLT.

```bash
config t
OLT(config)#pon-onu
OLT(config)#pon-onu-mng gpon-onu_1/4/1:2
OLT(gpon-onu-mng)#reboot
```

### Reset ONT

Reset ONT dari OLT.

```bash
OLT-ZTE_C320#config t
OLT-ZTE_C320(config)#pon-onu
OLT-ZTE_C320(config)#pon-onu-mng gpon-onu_1/4/1:2
OLT-ZTE_C320(gpon-onu-mng)#restore factory
```

#### How to lock / shutdon  third party port ONT

```bash
OLT-ZTE_C320#config t
OLT-ZTE_C320(config)#pon-onu
OLT-ZTE_C320(config)#pon-onu-mng gpon-onu_1/4/1:2
OLT-ZTE_C320(gpon-onu-mng)#interface eth eth_0/3 state lock
```

### Unlock Port ONT

Lock

```bash
OLT-ZTE_C320#config t
OLT-ZTE_C320(config)#pon-onu
OLT-ZTE_C320(config)#pon-onu-mng gpon-onu_1/4/1:2
OLT-ZTE_C320(gpon-onu-mng)#interface eth eth_0/3 state lock
```

Unlock

```bash
OLT-ZTE_C320#config t
Enter configuration commands, one per line.  End with CTRL/Z.
OLT-ZTE_C320(config)#pon-onu
OLT-ZTE_C320(config)#pon-onu-mng gpon-onu_1/4/1:2
OLT-ZTE_C320(gpon-onu-mng)#interface eth eth_0/3 state unlock
```

### Release New ONT as DHCP Client

```bash
OLT-ZTE_C320#config t
OLT-ZTE_C320(config)#pon-onu
OLT-ZTE_C320(config)#pon-onu-mng gpon-onu_1/4/1:2
OLT-ZTE_C320(gpon-onu-mng)#ip-host 1 dhcp-enable false ping-response false traceroute-response false
OLT-ZTE_C320(gpon-onu-mng)#ip-host 1 dhcp-enable true ping-response true traceroute-response true
```

### CHECKING RX LEVEL FOR OLT UPLINK

Make Sure , There is no CRC Counting at your Uplink Interface :p

```bash
OLT-ZTE_C320#sho int optical-module-info xgei_1/21/1
Optical module information:xgei_1/21/1
Basic-info:
Vendor-Name    : SOU          Vendor-Pn      : SPP1
Vendor-Sn      : D9               Version-Lev    : 10
Production-Date: 13                   Module-Type    : 10GBASE-LR
Wavelength     : 1310      (nm)           Connector      : LC
Diagnostic-info:
RxPower        : -10.1(dbm)          TxPower      : -1.7(dbm)
Bias-Current   : 25.724    (mA)           Laser-Rate   : 103(100Mb/s)
Temperature    : 24.184    (c)            Supply-Vol   : 3.324(v)
Alarm-thresh: –> Threshold
RxPower-Upper    : 3  (dbm)               RxPower-Lower    : -34(dbm)
TxPower-Upper    : 9  (dbm)               TxPower-Lower    : -14(dbm)
Bias-Upper       : 131(mA)                Bias-Lower       : 0  (mA)
Voltage-Upper    : 7  (v)                 Voltage-Lower    : 0  (v)
Temperature-Upper: 90 (c)                 Temperature-Lower: -45(c)
OLT-ZTE_C320#
```

### RESET SLOT

```bash
OLT-ZTE_C320#reset-card slotno 12
```

### Swap

Use This for First Level Handling when you get Anomaly Process.

sometimes it can help you, but sometime isn’t.

```bash
OLT-ZTE_C320#swap
```

### CHECKING VLAN SUMMARY

```bash
OLT-ZTE_C320#sho vlan sum 
```

\
All created vlan num: 3\
Details are following:\
111, 222 , 333

```bash
OLT-ZTE_C320#sho vlan 111
```

```bash
vlanid          :111
name            :VLAN00111
description     :N/A
multicast-packet:flood-unknown
tpid:0x8100
port(untagged):
port(tagged):
gpon-onu_1/2/8:128:1
xgei_1/22/1
sg1
OLT-ZTE_C320#
```



### Basic Config

```bash
Untuk Mikrotik di colok ke 10/100 untuk remote OLT.
Mikrotik yg mengarah ke 10/100 tersebut diberi IP 136.1.1.1/24 lalu coba ping ke 136.1.1.100 ( ip OLT )

Agar dapat terhubung dengan Netnumen maka OLT harus disetting sebagai berikut ( telnet ke 136.1.1.100 dan paste config dibawah ini )

############## Config agar OLT dapat diremote ###########
conf t
line telnet idle-timeout 1000
snmp-server community public view allview rw
snmp-server host 136.1.1.100 trap version 2c public enable NOTIFICATIONS server-index 1 udp-port 162
snmp-server enable trap
ip route 0.0.0.0 0.0.0.0 136.1.1.1
end
write

############## END Config  ###########

############## Dijalankan jika habis direset ##############

conf t
line telnet idle-timeout 1000
snmp-server community public view allview rw
snmp-server host 136.1.1.100 trap version 2c public enable NOTIFICATIONS server-index 1 udp-port 162
snmp-server enable trap
ip route 0.0.0.0 0.0.0.0 136.1.1.1
add-rack rackno 1 racktype C320Rack
add-shelf shelfno 1 shelftype C320_SHELF
add-card slotno 3 PRAM
add-card slotno 1 GTGO
add-subcard slotno 4 subcardno 1 UCDC/1
set-pnp enable
fan control temp_level 30 35 40 50
fan high-threshold 65
gpon
  profile tcont 100M type 1 fixed 100000
  profile traffic UP100M sir 100000 pir 120000
  profile traffic DW100M sir 100000 pir 120000
exit
gpon
  onu profile vlan PPPoE tag-mode tag cvlan 600
  onu profile vlan HOTSPOT tag-mode tag cvlan 500
exit
pon
onu-type ZTE-F660 gpon description 4ETH,2POTS,WIFI service-mgmt-via-non-omci wifi enable
onu-type ZTE-F660 gpon max-tcont 7 max-gemport 32 max-switch-perslot 8 max-flow-perswitch 32 max-iphost 5
onu-type-if ZTE-F660 eth_0/1-4
onu-type-if ZTE-F660 pots_0/1-2
onu-type-if ZTE-F660 wifi_0/1-4
onu-type ZTE-F609 gpon description 4ETH,2POTS,WIFI service-mgmt-via-non-omci wifi enable
onu-type ZTE-F609 gpon max-tcont 7 max-gemport 32 max-switch-perslot 8 max-flow-perswitch 32 max-iphost 5
onu-type-if ZTE-F609 eth_0/1-4
onu-type-if ZTE-F609 pots_0/1-2
onu-type-if ZTE-F609 wifi_0/1-4
exit
interface gei_1/4/3
switchport mode trunk
switchport vlan 600 tag
exit


####### Untuk melihat rack

show rack
show shelf
show card
show subcard
show running-config
show gpon onu uncfg
show gpon onu state

############ Untuk mengadopsi / meregister modem yg baru dipasang


show gpon onu uncfg #
interface gpon-olt_1/1/1
onu 1 type ZTE-F609 sn ZTEGC86CCB88
exit


interface gpon-onu_1/1/1:1
  name PEMDA
  description 1$$PEMDA$$
  sn-bind enable sn
  tcont 1 name HSI profile 100M
  tcont 2 name HOT profile 100M
  gemport 1 name HSI unicast tcont 1 dir both
  gemport 1 traffic-limit upstream UP100M downstream DW100M
  gemport 2 name HOT unicast tcont 2 dir both
  gemport 2 traffic-limit upstream UP100M downstream DW100M
  switchport mode hybrid vport 1
  switchport mode hybrid vport 2
  service-port 1 vport 1 user-vlan 600 vlan 600
  service-port 2 vport 2 user-vlan 500 vlan 500
  pppoe-plus enable sport 1
  pppoe-plus trust true replace sport 1
exit


pon-onu-mng gpon-onu_1/1/1:1
  service HSI type internet gemport 1 cos 0 vlan 600
  service HOT type internet gemport 2 cos 0 vlan 500
  wan-ip 1 mode pppoe username testing123@babel password 123456 vlan-profile PPPoE host 1
  wan-ip 1 ping-response enable traceroute-response enable
  interface wifi wifi_0/1 state lock
  interface wifi wifi_0/4 state lock
  wifi enable channel 1
  ssid auth wep wifi_0/1 open-system
  ssid auth wep wifi_0/2 open-system
  ssid auth wep wifi_0/3 open-system
  ssid auth wep wifi_0/4 open-system
  ssid ctrl wifi_0/1 name 1
  ssid ctrl wifi_0/2 name My-WIFI
  ssid ctrl wifi_0/3 name HOTSPOTKU
  ssid ctrl wifi_0/4 name 2
  wan 1 ethuni 1,2,3 ssid 2 service internet host 1
  vlan port eth_0/4 mode tag vlan 500
  vlan port wifi_0/3 mode tag vlan 500
  dhcp-ip ethuni eth_0/4 from-internet
exit
```



### Enable WAN Remote

```bash
conf t
pon-onu-mng gpon_"TARGET INTERFACE"

#enable
security-mng 2 ingress-type wan mode permit state enable protocol web
#disable
security-mng 2 ingress-type wan mode permit state disable protocol web
```

### Gpon Profile

```bash
#Registrasi Onu dengan bandwith Ratio up to dw 1:1
#Profile TCON ALL (MIXED)
#Profile Bandwith 10:XMbps
#UPDATE 09-jun-2021

interface gpon-onu_INTERFACE_GPON_ONU
  name JDN_[IDPEL]_[NAMAPEL]
  description zone_btu_junrejo_torongrejo_descr_Jl._aji_mustofa\&_[PHONENUMBER]_extid_[GPONSN]_authd_20210605
  tcont 1 profile SMARTOLT-10M-UP
  gemport 1 unicast tcont 1 dir both
  gemport 1 traffic-limit downstream SMARTOLT-10M-DOWN
  switchport mode hybrid vport 1
  switchport vlan 1200  tag vport 1
!

pon-onu-mng gpon-onu_INTERFACE_ONU
  flow mode 1 tag-filter vid-filter untag-filter discard
  flow 1 priority 0 vid 1200
  gemport 1 flow 1
  switchport-bind switch_0/1 iphost 1
  switchport-bind switch_0/1 veip 1
  pppoe 1 nat enable user [PPPOEUSERNAME] password [PPPOEPASSWORD]
  vlan-filter-mode iphost 1 tag-filter vid-filter untag-filter discard
  vlan-filter iphost 1 priority 0 vid 1200
  dhcp-ip ethuni eth_0/1 from-onu
  dhcp-ip ethuni eth_0/2 from-onu
  dhcp-ip ethuni eth_0/3 from-onu
  dhcp-ip ethuni eth_0/4 from-onu
  security-mng 1 state enable mode permit protocol web https
  security-mng 1 start-src-ip 10.115.0.1 end-src-ip 10.115.0.255
  security-mng 998 state enable mode permit ingress-type lan protocol web https
  security-mng 999 state enable ingress-type lan protocol ftp telnet ssh snmp tr069
```

### Ganti GPON Serial Number

```bash
GPON00-D-KL(config)#inte
GPON00-D-KL(config)#interface gp
GPON00-D-KL(config)#interface gpon-on
GPON00-D-KL(config)#interface gpon-onu_0/10/4:1
GPON00-D-KL(config-if)#registration-method sn  ZTEGXxXxX
```
