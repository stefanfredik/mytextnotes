---
description: Berisi catatan catatan singkat yang berhubungan dengan IP Address.
---

# 🔢 All About IP Address

## IP Range

### Private

IP private (alamat IP yang tidak bisa diakses langsung dari internet) didefinisikan dalam beberapa blok alamat IP sesuai dengan standar yang ditentukan oleh **RFC 1918**. Berikut adalah daftar range IP private berdasarkan CIDR:

#### 1. Kelas A&#x20;

* #### CIDR: `10.0.0.0/8`
* **Range IP**: `10.0.0.0 - 10.255.255.255`
* **Jumlah IP**: 16.777.216 IP

#### 2. Kelas B&#x20;

* #### CIDR: `172.16.0.0/12`
* **Range IP**: `172.16.0.0 - 172.31.255.255`
* **Jumlah IP**: 1.048.576 IP

#### 3. Kelas C

* #### CIDR: `192.168.0.0/16`
* **Range IP**: `192.168.0.0 - 192.168.255.255`
* **Jumlah IP**: 65.536 IP

#### Detail IP Private:

* **10.0.0.0/8**: Seluruh subnet mulai dari 10.0.0.0 hingga 10.255.255.255.
* **172.16.0.0/12**: Seluruh subnet mulai dari 172.16.0.0 hingga 172.31.255.255.
* **192.168.0.0/16**: Seluruh subnet mulai dari 192.168.0.0 hingga 192.168.255.255.



### IP Public

```

1.0.0.0-9.255.255.255
11.0.0.0-100.63.255.255
100.128.0.0-126.255.255.255
128.0.0.0-169.253.255.255
169.255.0.0-172.15.255.255
172.32.0.0-191.255.255.255
192.0.1.0/24
192.0.3.0-192.88.98.255
192.88.100.0-192.167.255.255
192.169.0.0-198.17.255.255
198.20.0.0-198.51.99.255
198.51.101.0-203.0.112.255
203.0.114.0-223.255.255.255

```

## Ip Address Trouble Shoot

Ip address yang digunakan untuk melakukan traceroute untuk kebutuhan troubleshoot.

Beberapa IP publik yang sering digunakan untuk melakukan traceroute adalah:

1. **Google Public DNS**: 8.8.8.8
2. **Cloudflare DNS**: 1.1.1.1
3. **OpenDNS**: 208.67.222.222 dan 208.67.220.220
4. **Facebook**: 31.13.71.36
5. **Netflix**: 13.226.52.184
