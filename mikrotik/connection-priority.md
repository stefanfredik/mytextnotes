---
description: >-
  Dengan menggunakan Connection Priority kita dapat memprioritaskan koneksi pada
  aplikasi atau situs web tertentu.
---

# Connection Priority

### Zoom Priority

Pada contoh saat ini kita akan menggunakan aplikasi zoom. Kita akan memprioritaskan koneksi zoom dari pada koneksi lainya.

#### Pengumpulan IP dan Port Aplikasi

```bash
/ip firewall address-list
add address=3.7.35.0/25 list=zoom_ip
add address=3.21.137.128/25 list=zoom_ip
add address=3.22.11.0/24 list=zoom_ip
add address=3.23.93.0/24 list=zoom_ip
add address=3.25.41.128/25 list=zoom_ip
add address=3.25.42.0/25 list=zoom_ip
add address=3.25.49.0/24 list=zoom_ip
add address=3.80.20.128/25 list=zoom_ip
add address=3.96.19.0/24 list=zoom_ip
add address=3.101.32.128/25 list=zoom_ip
add address=3.101.52.0/25 list=zoom_ip
add address=3.104.34.128/25 list=zoom_ip
add address=3.120.121.0/25 list=zoom_ip
add address=3.127.194.128/25 list=zoom_ip
add address=3.208.72.0/25 list=zoom_ip
add address=3.211.241.0/25 list=zoom_ip
add address=3.235.69.0/25 list=zoom_ip
add address=3.235.71.128/25 list=zoom_ip
add address=3.235.72.128/25 list=zoom_ip
add address=3.235.73.0/25 list=zoom_ip
add address=3.235.82.0/23 list=zoom_ip
add address=3.235.96.0/23 list=zoom_ip
add address=4.34.125.128/25 list=zoom_ip
add address=4.35.64.128/25 list=zoom_ip
add address=8.5.128.0/23 list=zoom_ip
add address=13.52.6.128/25 list=zoom_ip
add address=13.52.146.0/25 list=zoom_ip
add address=15.220.80.0/24 list=zoom_ip
add address=15.220.81.0/25 list=zoom_ip
add address=18.157.88.0/24 list=zoom_ip
add address=18.205.93.128/25 list=zoom_ip
add address=18.254.23.128/25 list=zoom_ip
add address=18.254.61.0/25 list=zoom_ip
add address=20.203.158.80/28 list=zoom_ip
add address=20.203.190.192/26 list=zoom_ip
add address=50.239.202.0/23 list=zoom_ip
add address=50.239.204.0/24 list=zoom_ip
add address=52.61.100.128/25 list=zoom_ip
add address=52.202.62.192/26 list=zoom_ip
add address=52.215.168.0/25 list=zoom_ip
add address=64.125.62.0/24 list=zoom_ip
add address=64.211.144.0/24 list=zoom_ip
add address=64.224.32.0/19 list=zoom_ip
add address=65.39.152.0/24 list=zoom_ip
add address=69.174.57.0/24 list=zoom_ip
add address=69.174.108.0/22 list=zoom_ip
add address=99.79.20.0/25 list=zoom_ip
add address=101.36.167.0/24 list=zoom_ip
add address=101.36.170.0/23 list=zoom_ip
add address=103.122.166.0/23 list=zoom_ip
add address=111.33.115.0/25 list=zoom_ip
add address=111.33.181.0/25 list=zoom_ip
add address=115.110.154.192/26 list=zoom_ip
add address=115.114.56.192/26 list=zoom_ip
add address=115.114.115.0/26 list=zoom_ip
add address=115.114.131.0/26 list=zoom_ip
add address=120.29.148.0/24 list=zoom_ip
add address=129.151.1.128/27 list=zoom_ip
add address=129.151.1.192/27 list=zoom_ip
add address=129.151.2.0/27 list=zoom_ip
add address=129.151.3.160/27 list=zoom_ip
add address=129.151.7.96/27 list=zoom_ip
add address=129.151.11.64/27 list=zoom_ip
add address=129.151.11.128/27 list=zoom_ip
add address=129.151.12.0/27 list=zoom_ip
add address=129.151.13.64/27 list=zoom_ip
add address=129.151.15.224/27 list=zoom_ip
add address=129.151.16.0/27 list=zoom_ip
add address=129.151.31.224/27 list=zoom_ip
add address=129.151.40.0/25 list=zoom_ip
add address=129.151.40.160/27 list=zoom_ip
add address=129.151.40.192/27 list=zoom_ip
add address=129.151.41.0/25 list=zoom_ip
add address=129.151.41.192/26 list=zoom_ip
add address=129.151.42.0/27 list=zoom_ip
add address=129.151.42.64/27 list=zoom_ip
add address=129.151.42.128/26 list=zoom_ip
add address=129.151.42.224/27 list=zoom_ip
add address=129.151.43.0/27 list=zoom_ip
add address=129.151.43.64/26 list=zoom_ip
add address=129.151.48.0/27 list=zoom_ip
add address=129.151.48.160/27 list=zoom_ip
add address=129.151.49.0/26 list=zoom_ip
add address=129.151.49.96/27 list=zoom_ip
add address=129.151.49.128/27 list=zoom_ip
add address=129.151.49.192/26 list=zoom_ip
add address=129.151.50.0/27 list=zoom_ip
add address=129.151.50.64/27 list=zoom_ip
add address=129.151.52.128/26 list=zoom_ip
add address=129.151.53.32/27 list=zoom_ip
add address=129.151.53.224/27 list=zoom_ip
add address=129.151.55.32/27 list=zoom_ip
add address=129.151.56.32/27 list=zoom_ip
add address=129.151.57.32/27 list=zoom_ip
add address=129.151.60.192/27 list=zoom_ip
add address=129.159.2.32/27 list=zoom_ip
add address=129.159.2.192/27 list=zoom_ip
add address=129.159.3.0/24 list=zoom_ip
add address=129.159.4.0/23 list=zoom_ip
add address=129.159.6.0/27 list=zoom_ip
add address=129.159.6.96/27 list=zoom_ip
add address=129.159.6.128/26 list=zoom_ip
add address=129.159.6.192/27 list=zoom_ip
add address=129.159.160.0/26 list=zoom_ip
add address=129.159.160.64/27 list=zoom_ip
add address=129.159.163.0/26 list=zoom_ip
add address=129.159.163.160/27 list=zoom_ip
add address=129.159.208.0/21 list=zoom_ip
add address=129.159.216.0/26 list=zoom_ip
add address=129.159.216.64/27 list=zoom_ip
add address=129.159.216.128/26 list=zoom_ip
add address=130.61.164.0/22 list=zoom_ip
add address=132.226.176.0/25 list=zoom_ip
add address=132.226.176.128/26 list=zoom_ip
add address=132.226.177.96/27 list=zoom_ip
add address=132.226.177.128/25 list=zoom_ip
add address=132.226.178.0/27 list=zoom_ip
add address=132.226.178.128/27 list=zoom_ip
add address=132.226.178.224/27 list=zoom_ip
add address=132.226.179.0/27 list=zoom_ip
add address=132.226.179.64/27 list=zoom_ip
add address=132.226.180.128/27 list=zoom_ip
add address=132.226.183.160/27 list=zoom_ip
add address=132.226.185.192/27 list=zoom_ip
add address=134.224.0.0/16 list=zoom_ip
add address=140.238.128.0/24 list=zoom_ip
add address=140.238.232.0/22 list=zoom_ip
add address=144.195.0.0/16 list=zoom_ip
add address=147.124.96.0/19 list=zoom_ip
add address=149.137.0.0/17 list=zoom_ip
add address=150.230.224.0/25 list=zoom_ip
add address=150.230.224.128/26 list=zoom_ip
add address=150.230.224.224/27 list=zoom_ip
add address=152.67.20.0/24 list=zoom_ip
add address=152.67.118.0/24 list=zoom_ip
add address=152.67.168.0/22 list=zoom_ip
add address=152.67.180.0/24 list=zoom_ip
add address=152.67.184.32/27 list=zoom_ip
add address=152.67.240.0/21 list=zoom_ip
add address=152.70.0.0/25 list=zoom_ip
add address=152.70.0.128/26 list=zoom_ip
add address=152.70.0.224/27 list=zoom_ip
add address=152.70.1.0/25 list=zoom_ip
add address=152.70.1.128/26 list=zoom_ip
add address=152.70.1.192/27 list=zoom_ip
add address=152.70.2.0/26 list=zoom_ip
add address=152.70.7.192/27 list=zoom_ip
add address=152.70.10.32/27 list=zoom_ip
add address=152.70.224.32/27 list=zoom_ip
add address=152.70.224.64/26 list=zoom_ip
add address=152.70.224.160/27 list=zoom_ip
add address=152.70.224.192/27 list=zoom_ip
add address=152.70.225.0/25 list=zoom_ip
add address=152.70.225.160/27 list=zoom_ip
add address=152.70.225.192/27 list=zoom_ip
add address=152.70.226.0/27 list=zoom_ip
add address=152.70.227.96/27 list=zoom_ip
add address=152.70.227.192/27 list=zoom_ip
add address=152.70.228.0/27 list=zoom_ip
add address=152.70.228.64/27 list=zoom_ip
add address=152.70.228.128/27 list=zoom_ip
add address=156.45.0.0/17 list=zoom_ip
add address=158.101.64.0/24 list=zoom_ip
add address=158.101.184.0/23 list=zoom_ip
add address=158.101.186.0/25 list=zoom_ip
add address=158.101.186.128/27 list=zoom_ip
add address=158.101.186.192/26 list=zoom_ip
add address=158.101.187.0/25 list=zoom_ip
add address=158.101.187.160/27 list=zoom_ip
add address=158.101.187.192/26 list=zoom_ip
add address=159.124.0.0/16 list=zoom_ip
add address=160.1.56.128/25 list=zoom_ip
add address=161.199.136.0/22 list=zoom_ip
add address=162.12.232.0/22 list=zoom_ip
add address=162.255.36.0/22 list=zoom_ip
add address=165.254.88.0/23 list=zoom_ip
add address=166.108.64.0/18 list=zoom_ip
add address=168.138.16.0/22 list=zoom_ip
add address=168.138.48.0/24 list=zoom_ip
add address=168.138.56.0/21 list=zoom_ip
add address=168.138.72.0/24 list=zoom_ip
add address=168.138.74.0/25 list=zoom_ip
add address=168.138.80.0/25 list=zoom_ip
add address=168.138.80.128/26 list=zoom_ip
add address=168.138.80.224/27 list=zoom_ip
add address=168.138.81.0/24 list=zoom_ip
add address=168.138.82.0/23 list=zoom_ip
add address=168.138.84.0/25 list=zoom_ip
add address=168.138.84.128/27 list=zoom_ip
add address=168.138.84.192/26 list=zoom_ip
add address=168.138.85.0/24 list=zoom_ip
add address=168.138.86.0/23 list=zoom_ip
add address=168.138.96.0/22 list=zoom_ip
add address=168.138.116.0/27 list=zoom_ip
add address=168.138.116.64/27 list=zoom_ip
add address=168.138.116.128/27 list=zoom_ip
add address=168.138.116.224/27 list=zoom_ip
add address=168.138.117.0/27 list=zoom_ip
add address=168.138.117.96/27 list=zoom_ip
add address=168.138.117.128/27 list=zoom_ip
add address=168.138.118.0/27 list=zoom_ip
add address=168.138.118.160/27 list=zoom_ip
add address=168.138.118.224/27 list=zoom_ip
add address=168.138.119.0/27 list=zoom_ip
add address=168.138.119.128/27 list=zoom_ip
add address=168.138.244.0/24 list=zoom_ip
add address=170.114.0.0/16 list=zoom_ip
add address=173.231.80.0/20 list=zoom_ip
add address=192.204.12.0/22 list=zoom_ip
add address=193.122.16.0/25 list=zoom_ip
add address=193.122.16.192/27 list=zoom_ip
add address=193.122.17.0/26 list=zoom_ip
add address=193.122.17.64/27 list=zoom_ip
add address=193.122.17.224/27 list=zoom_ip
add address=193.122.18.32/27 list=zoom_ip
add address=193.122.18.64/26 list=zoom_ip
add address=193.122.18.160/27 list=zoom_ip
add address=193.122.18.192/27 list=zoom_ip
add address=193.122.19.0/27 list=zoom_ip
add address=193.122.19.160/27 list=zoom_ip
add address=193.122.19.192/27 list=zoom_ip
add address=193.122.20.224/27 list=zoom_ip
add address=193.122.21.96/27 list=zoom_ip
add address=193.122.32.0/21 list=zoom_ip
add address=193.122.40.0/22 list=zoom_ip
add address=193.122.44.0/24 list=zoom_ip
add address=193.122.45.32/27 list=zoom_ip
add address=193.122.45.64/26 list=zoom_ip
add address=193.122.45.128/25 list=zoom_ip
add address=193.122.46.0/23 list=zoom_ip
add address=193.122.208.96/27 list=zoom_ip
add address=193.122.216.32/27 list=zoom_ip
add address=193.122.222.0/27 list=zoom_ip
add address=193.122.223.128/27 list=zoom_ip
add address=193.122.226.160/27 list=zoom_ip
add address=193.122.231.192/27 list=zoom_ip
add address=193.122.232.160/27 list=zoom_ip
add address=193.122.237.64/27 list=zoom_ip
add address=193.122.244.160/27 list=zoom_ip
add address=193.122.244.224/27 list=zoom_ip
add address=193.122.245.0/27 list=zoom_ip
add address=193.122.247.96/27 list=zoom_ip
add address=193.122.252.192/27 list=zoom_ip
add address=193.123.0.0/19 list=zoom_ip
add address=193.123.40.0/21 list=zoom_ip
add address=193.123.128.0/19 list=zoom_ip
add address=193.123.168.0/21 list=zoom_ip
add address=193.123.192.224/27 list=zoom_ip
add address=193.123.193.0/27 list=zoom_ip
add address=193.123.193.96/27 list=zoom_ip
add address=193.123.194.96/27 list=zoom_ip
add address=193.123.194.128/27 list=zoom_ip
add address=193.123.194.224/27 list=zoom_ip
add address=193.123.195.0/27 list=zoom_ip
add address=193.123.196.0/27 list=zoom_ip
add address=193.123.196.192/27 list=zoom_ip
add address=193.123.197.0/27 list=zoom_ip
add address=193.123.197.64/27 list=zoom_ip
add address=193.123.198.64/27 list=zoom_ip
add address=193.123.198.160/27 list=zoom_ip
add address=193.123.199.64/27 list=zoom_ip
add address=193.123.200.128/27 list=zoom_ip
add address=193.123.201.32/27 list=zoom_ip
add address=193.123.201.224/27 list=zoom_ip
add address=193.123.202.64/27 list=zoom_ip
add address=193.123.202.128/26 list=zoom_ip
add address=193.123.203.0/27 list=zoom_ip
add address=193.123.203.160/27 list=zoom_ip
add address=193.123.203.192/27 list=zoom_ip
add address=193.123.204.0/27 list=zoom_ip
add address=193.123.204.64/27 list=zoom_ip
add address=193.123.205.64/26 list=zoom_ip
add address=193.123.205.128/27 list=zoom_ip
add address=193.123.206.32/27 list=zoom_ip
add address=193.123.206.128/27 list=zoom_ip
add address=193.123.207.32/27 list=zoom_ip
add address=193.123.208.160/27 list=zoom_ip
add address=193.123.209.0/27 list=zoom_ip
add address=193.123.209.96/27 list=zoom_ip
add address=193.123.210.64/27 list=zoom_ip
add address=193.123.211.224/27 list=zoom_ip
add address=193.123.212.128/27 list=zoom_ip
add address=193.123.215.192/26 list=zoom_ip
add address=193.123.216.64/27 list=zoom_ip
add address=193.123.216.128/27 list=zoom_ip
add address=193.123.217.160/27 list=zoom_ip
add address=193.123.219.64/27 list=zoom_ip
add address=193.123.220.224/27 list=zoom_ip
add address=193.123.222.64/27 list=zoom_ip
add address=193.123.222.224/27 list=zoom_ip
add address=198.251.128.0/17 list=zoom_ip
add address=202.177.207.128/27 list=zoom_ip
add address=204.80.104.0/21 list=zoom_ip
add address=204.141.28.0/22 list=zoom_ip
add address=206.247.0.0/16 list=zoom_ip
add address=207.226.132.0/24 list=zoom_ip
add address=209.9.211.0/24 list=zoom_ip
add address=209.9.215.0/24 list=zoom_ip
add address=213.19.144.0/24 list=zoom_ip
add address=213.19.153.0/24 list=zoom_ip
add address=213.244.140.0/24 list=zoom_ip
add address=221.122.63.0/24 list=zoom_ip
add address=221.122.64.0/24 list=zoom_ip
add address=221.122.88.64/27 list=zoom_ip
add address=221.122.88.128/25 list=zoom_ip
add address=221.122.89.128/25 list=zoom_ip
add address=221.123.139.192/27 list=zoom_ip

```

#### Menambahkan Address List pada Firewall Mikrotik

Copy dan paste list tersebut di dalam terminal ROS.

### Kongfigurasi Mangle&#x20;

#### Add address list zoom

Berfungsi untuke menangkap IP atau alamat zoom yang diakses oleh user.

Buat mangle baru dengan ketentuan sebagai berikut :&#x20;

* chain : prerouting
* protocol : tcp
* dst-port&#x20;
  * Port yang digunakan oleh zoom
  * Port :  3478,3479,5090,5091,8801-8810
* Pada Tab Action :&#x20;
  * Action : add dst to address list
  * address-lit : zoom-ip

Kemudian buatkan kembali mangle yang kedua persis seperti diatas namun :&#x20;

Ganti protocol mejadi : udp

#### Menandai Packet Zoom

Tambah mangle baru sesuai dengan ketentuan berikut :&#x20;

* Tab General :&#x20;
  * chain : prerouting
  * protocol : tcp
  * dst-port : port aplikasi zoom
* Tab Advanced :&#x20;
  * dst -add-list : zoom-ip
* Tab Action :&#x20;
  * action : mark connection
  * new connection mark : koneksi-zoom

Kemudian selanjutnya untuk protocol udp, lakukan langkah yang sama seperti diatas kemudian ganti protocolnya menjadi udp.





