---
description: >-
  Berisi catatan perintah-perintah yang sering digunakan pada OLT ZTE C320 dalam
  melakukan pengecekan dan kongfigurasi jaringan.
---

# ZTE C320 OLT

### Show Config

#### Show Bandwith Profile

```bash
sh gpon profile traffic
```

#### Show Tcon  Profile



#### Show **Utilisasi Bandwith Link ONT Real Time**

```bash
show interface gpon-onu_[port]
```



#### Show **Mac Address Perangkat yang terkoneksi ke ONT**

```bash
sho mac gpon onu gpon-onu_[port onu]
```



#### Show Ont Version

```bash
sho gpon remote-onu equip gpon-onu_[port]
```

#### Show ONT IP

```bash
sho gpon remote-onu ip-host gpon-onu_1/4/1:2
```

#### Show Ont Port Status

```bash
sho gpon remote-onu interface eth gpon-onu_[port]
```



### ZTE C320 Config

**Cek Detail Config di Interface ke arah ONT**

```bash
show run interface gpon-onu_1/2/5:4
```

#### **Cek Status Interface ONT**

```bash
show gpon onu state gpon-olt_1/2/5
```



**Cek Nilai Optik ONT**

```bash
show pon power attenuation gpon-onu_1/2/5:4      

```

**Cek Utilisasi Bandwith Link ONT Real Time**

```bash
show interface gpon-onu_1/2/5:4
```

**Cek ONT Uptime dan History**

```bash
OLT-ZTE_C320#show gpon onu detail-info gpon-onu_1/2/5:5
```

\
**Cek Mac Address Perangkat yang terkoneksi ke ONT**

```bash
show mac gpon onu gpon-onu_1/2/5:5
```

**Cek Status Port di ONT**

```bash
show gpon remote-onu interface eth gpon-onu_1/2/5:5
```

#### Reboot/Restart ONT

```bash
config t
Enter configuration commands, one per line.  End with CTRL/Z.
pon-onu
pon-onu-mng gpon-onu_1/2/5:5
reboot
```

**Shutdown Port 1 di ONT**

```bash
config t
pon-onu
pon-onu-mng gpon-onu_1/2/5:5
interface eth eth_0/1 state lock
```

**Cek Konfigurasi PON ONU**

```bash
show onu running config gpon-onu_1/2/11:8

pon-onu-mng gpon-onu_1/2/11:8
  service Internet-Customer1 gemport 1 vlan 671
  service OAM gemport 2 vlan 1450
  service Internet-Customer2 gemport 3 vlan 1490
  vlan port eth_0/1 mode tag vlan 671
  vlan port eth_0/3 mode tag vlan 1490
  
```

#### Check OLT Uptime

```bash
show system-group

System Description: ZX Version V4.
Started before: 47484 days, 5 hours, 25 minutes
```

#### Check OLT Board

```bash
GTGOE = 8 Port (O=Octal)
GTGH = 16 Port (H=Hexa
```

```bash
sho card
```

#### Check OLT Processor dan Memory

```bash
show processor
```

#### Check OLT Temperature

```bash
show temperature
```

#### CHECKING OLT PER SLOT

```bash
show card slotno 12
```

#### Finding Ont by Serial Number

```bash
sho gpon onu by sn ABCDEFGHIJ
```

#### Checking ONT status on OLT Port

```bash
sho gpon onu sta gpon-olt_1/4/1 2
```



CHECKING ONT UPTIME AND ONT HISTORY LOG

```bash
sho gpon onu detail-info gpon-onu_1/4/1:2
```

#### Checking Ont Mac Address

```bash
sho mac gpon onu gpon-onu_1/4/1:2
```



#### Checking ONT Power

```bash
sho pon power attenuation gpon-onu_1/4/1:2
```



#### Checking Ont Version

```bash
sho gpon remote-onu equip gpon-onu_1/4/1:2
```

#### Checking Ont IP (Routing Mode)

```bash
sho gpon remote-onu equip gpon-onu_1/4/1:2
```

```bash
sho gpon remote-onu ip-host gpon-onu_1/4/1:2
```

#### Checking ONT Port Status

```bash
sho gpon remote-onu interface eth gpon-onu_1/4/1:2
```

#### Checking ONT Configuration

```bash
sho run int gpon-onu_1/4/1:2
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

Config 2

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

#### Reboot ONT

```bash
config t
OL01(config)#pon-onu
OL01(config)#pon-onu-mng gpon-onu_1/4/1:2
OL01(gpon-onu-mng)#reboot
```

#### Reset ONT

```bash
OLT-ZTE_C320#config t
OLT-ZTE_C320(config)#pon-onu
OLT-ZTE_C320(config)#pon-onu-mng gpon-onu_1/4/1:2
OLT-ZTE_C320(gpon-onu-mng)#restore factory
```

#### How to lock / shutdon  third party port ONT

```bash
OLT-ZTE_C320#config t
Enter configuration commands, one per line.  End with CTRL/Z.
OLT-ZTE_C320(config)#pon-onu
OLT-ZTE_C320(config)#pon-onu-mng gpon-onu_1/4/1:2
OLT-ZTE_C320(gpon-onu-mng)#interface eth eth_0/3 state lock
```

#### Unlock Port ONT

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

#### RELEASE RENEW ONT AS DHCP CLIENT

```bash
OLT-ZTE_C320#config t
OLT-ZTE_C320(config)#pon-onu
OLT-ZTE_C320(config)#pon-onu-mng gpon-onu_1/4/1:2
OLT-ZTE_C320(gpon-onu-mng)#ip-host 1 dhcp-enable false ping-response false traceroute-response false
OLT-ZTE_C320(gpon-onu-mng)#ip-host 1 dhcp-enable true ping-response true traceroute-response true
```

#### CHECKING RX LEVEL FOR OLT UPLINK

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

#### RESET SLOT

```bash
OLT-ZTE_C320#reset-card slotno 12
```

#### Swap

Use This for First Level Handling when you get Anomaly Process.

sometimes it can help you, but sometime isn’t.

```bash
OLT-ZTE_C320#swap
```

#### CHECKING VLAN SUMMARY

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
