---
description: Berisi tutorial dan pengenalan dasar-dasar docker.
cover: >-
  https://images.unsplash.com/photo-1560732488-6b0df240254a?crop=entropy&cs=srgb&fm=jpg&ixid=M3wxOTcwMjR8MHwxfHNlYXJjaHw2fHxzZXJ2ZXJ8ZW58MHx8fHwxNzA4NjYyNTY4fDA&ixlib=rb-4.0.3&q=85
coverY: 0
---

# ⛴️ Docker Dasar

## Pengenalan Container

Docker adalah platform terbuka untuk mengembangkan, mengirimkan, dan menjalankan aplikasi. Docker memungkinkan Anda memisahkan aplikasi Anda dari infrastruktur Anda sehingga Anda dapat mengirimkan perangkat lunak dengan lebih cepat. Dengan Docker, Anda dapat mengelola infrastruktur Anda dengan cara yang sama Anda mengelola aplikasi Anda. Dengan menggunakan kontainer Docker, Anda dapat secara signifikan meningkatkan kecepatan dan efisiensi proses pengembangan.

Container berbeda dengan Virtual Mesin. Virtual machine menjalakan beberapa system operasi dan aplikasi berjalan didalamnya. Sedangkan Container menjalan hanya satu system operasi dan aplikasi berjalan diatas satu system operasi tersebut namun secara terpisah, seolah-olah aplikasi berjalan di system operasi yang berbeda.

Docker merupakan teknologi yang relative baru, yang mulai diperkenalkan pada tahun 2013.

Docker merupakan aplikasi yang free dan open source.

Aplikasi yang terinstal di docker  disebut images.

Berikut beberapa istilah pada docker :&#x20;

### Virtual Machine (VM)

* Dalam dunia infrastruktur sudah tidak asing lagi dengan Vitrual Machine
* VM Berjalan di atas sebuah system operasi, dan di dalam VM berjalan system operasi pula.
* Masalah  yang sering muncul ketika menggunakan VM adalah proses yang lama ketika membuat VM baru dan membutuhkan waktu yang cukup lama ketika merestart VM.&#x20;

#### Diagram VM

<figure><img src="https://lh7-us.googleusercontent.com/H32Tj24jS7tKwSa0XSslFI2sWFT_EKOm4s-VWctd-MiWk0j1rvEw3tRihmyX_XhjSTMG5ZbqcvTXicw7Pkh98-N6PYUNou0jopv9YBPElYtIhTEuJlbggCjwqmzqBb3AsG9eh0ugE9Fg3b6GyvGVydXXhA=s2048" alt=""><figcaption></figcaption></figure>

### Container

* Berbeda dengan VM, Container sendiri berjalan di sisi aplikasi.
* Container sendiri sebenarnya berjalan di atas aplikasi container manager pada system operasi utama.
* Yang berbeda dengan VM adalah kita bisa mempackage aplikasi berserta depencinya tanpa harus menggabungkanya dari aplikasi. Aplikasi yang terinstal terpisah dari system operasi atau berdiri sendiri.
* Container bisasa menggunakan system operasi hots atau system operasi utamanya dimana container managernya berjalan. Oleh karena itu container akan lebih hemat resource dan lebih cepat jalanya karena tidak butuh system operasi sendiri.&#x20;
* Ukuran container biasanya lebih kecil dan biasnya hanya kisaran berapa MB, berbeda dengan VM yang ukuranya cukup besar sampai bergiga giga karena ada system operasi di dalam nya.

#### Diagram Container

<figure><img src="https://lh7-us.googleusercontent.com/jnaf6IuPJ5e_VNcZO9bRpNM1lWf81eKta3noSTMb1M0CN0AzJ_AbQroQTB2qHKvGjfWti372IzBRdsfrkwtYe4N639aS5eXupsdIms-KOFCusW4rhCLyVPHx0wLXf79oGBZ42KYCfJNRF2iBqRlIPto8cg=s2048" alt=""><figcaption></figcaption></figure>

## Arsitektur Docker

* Docker menggunakan  arsitektur client dan server
* Docker client berkomunikasi dengan dengan docker daemon (server)
* Saat kita menginstal docker, biasanya di dalamnya sudah terdapat Docker Client dan Docker Daemon.
* Docker Client dan Docker Daemon bisa berjalan disystem yang sama.
* Docker client dan docker daemon berkomunikasi menggunakan REST API.

#### Diagram Docker Arsitektur

<figure><img src="https://lh7-us.googleusercontent.com/y6jGcuv3x9YdS4O5qJWBNZFis2f6jIkU4CP6ACwb4ru51LvRXyaOSfwU7FqDqJNFSyAwuY4v6pD5UjuNy92piSCr_hLYq0xwQCTRSRUzukocoBzY6_y1Tq0Zk_DhlEceLF_dTal8-MPWMYWom2gb5h3e-w=s2048" alt=""><figcaption></figcaption></figure>

## Menginstal Docker

* Docker bisa di install di semua system operasi
* Untuk menginstall di Windows dan Mac, kita bisa menggunakan docker Desktop.
* Untuk linux bisa langsung menginstal dari repository masing-masing distro Linux

### Mengecek Docker

Untuk mengecek docker apakah sudah terinstal di system operasi dapat menggunakan perintah berikut :&#x20;

```bash
docker version
```

## Dokcer Registry

* Docker registry adalah tempat kita menyimpan docker image
* Dengan menggunakan docker registry, kita bisa menyimpan image yang kita buat dan bisa digunakan di docker daemon dimanapun selama bisa terkoneksi ke docker registry.

### Diagram

<figure><img src="https://lh7-us.googleusercontent.com/y6jGcuv3x9YdS4O5qJWBNZFis2f6jIkU4CP6ACwb4ru51LvRXyaOSfwU7FqDqJNFSyAwuY4v6pD5UjuNy92piSCr_hLYq0xwQCTRSRUzukocoBzY6_y1Tq0Zk_DhlEceLF_dTal8-MPWMYWom2gb5h3e-w=s2048" alt=""><figcaption></figcaption></figure>

### Contoh Docker Registry

* Docker Hub : [https://hub.docker.com/](https://hub.docker.com/)&#x20;
* Digital Ocean Container Registry : [https://www.digitalocean.com/products/container-registry/](https://www.digitalocean.com/products/container-registry/)&#x20;
* Google Cloud Container Registry : [https://cloud.google.com/container-registry](https://cloud.google.com/container-registry)&#x20;
* Amazon Elastic Container Registry : [https://aws.amazon.com/id/ecr/](https://aws.amazon.com/id/ecr/)&#x20;
* Azure Container Registry : [https://azure.microsoft.com/en-us/services/container-registry/](https://azure.microsoft.com/en-us/services/container-registry/)&#x20;



## Docker Image

* Docker image mirip seperti instaler aplikasi, dimana di dalam docker image terdapat apalikasi dan depedency dari aplikasi.
* Sebelum kita menjalankan aplikasi docker, kita perlu memastikan memiliki docker image dari aplikasi tersebut.

### Melihat Docker Image

Untuk menlihat docker image yang sudah ada dalam docker dapat menggunakan perintah berikut :&#x20;

```bash
docker image ls
```

### Donwload Docker Image

* Untuk mendownload docker image dari docker registry kita bisa menggunakan perintah :&#x20;

```bash
docker image pull [namaimage]:[tag]

#Example : 
docker image pull nginx:latest
```

* Kita juga dapat mendownload docker image langsung dari http://hub.docker.com

### Menghapus Docker Image

Untuk menghapus docker image yang sudah didownload dapat menggunkan perintah berikut :&#x20;

```bash
docker image rm [namaimage]:[tag]

#Example :
docker image rm nginx:latest
```

## Docker Container

* Jika sebuah image merupakan installer aplikasi, maka docker container adalah aplikasi yang sudah terinstal.&#x20;
* Satu docker image dapat membuat beberapa docker container asalkan namanya berbeda.&#x20;
* Ketika docker container dibuat makan docker image tidak dapat dihapus lagi karena docker container tidak mengcopy docker image namun menggunakan isinya saja.

### Status Container

* Ketika kita membuat kontainer, secara default container tersebut belum berjalan.
* Mirip seperti kita menginstal aplikasi, jika aplikasi tersebut tidak dijalankan maka aplikasi tersebut tidak akan berjalan begitu juga container.
* Oleh karena itu kita perlu menjalankan container setelah kita membuat kontainer agar bisa digunakan.&#x20;

### Melihat Container

Kita dapat melihat semua container yang sudah terpasang/terbuat. Melihat container yang aktif maupun tidak. Bisa menggunakan perintah berikut :&#x20;

#### Melihat Semua Container

```bash
docker container ls -a
```

#### Melihat Container yang sedang berjalan saja

```bash
docker container ls
```

### Membuat Container

Untuk membuat container baru dapat menggunakan perintah berikut :&#x20;

```bash
docker container create --name [namacontainer] [namaimage]:[tag]
```

### Menjalankan Container

Untuk menjalankan container yang sudah dibuat dapat menggunakan perintah berikut :&#x20;

```bash
docker container start [containerid/containername]
```

Setelah container sudah dijalankan dapat dicek dengan menggunakan perintah `docker container ls`

### Menghentikan Container

Untuk menghentikan docker yang sudah berjalan dapat menggunakan perintah berikut :&#x20;

```bash
docker container stop [dockername/dockerid]

#Example
docker container stop nginx
```

### Menghapus Container

Container yang sudah berhenti dapat dihapus dengan menggunakan perintah berikut :&#x20;

```bash
docker container rm [containername/containerid]
```

### Container Log

* Terkadang terjadi masalah pada aplikasi yang tedapat dalam container, maka kita perlu melihat log untuk mengatahui detail dan informasi tentang masala tersebut.
* Hal tersebut untuk membantu kita menganalisa dan memperbaiki masalah tersebut.

#### Melihat Container Log

Dapat menggunakan perintah berikut :&#x20;

```bash
docker container logs [containerid/containername]
```

#### Memantau Log secara real time

Untuk memantau log secara realtime kita dapat menggunakan perintah berikut :&#x20;

```bash
docker container logs -f [containername/containerid]
```

### Container Exec

* Saat kita membuat container, aplikasi yang terdapat dalam container hanya bisa diakses dari dalam container
* Oleh karena itu, ketika kita ingin menggunakan aplikasinya kadang kita harus masuk ke dalam container itu sendiri.
* Untuk masuk ke dalam container kita bisa menggunakan fitur Container Exec, dimana digunakan untuk mengeksekusi kode program yang terdapat  di dalam container.

### Masuk Ke Container

Untuk masuk container kita dapat mengeksuksi program bash script yang tedapat dalam container dengan menggunakan container exec. Perintah yang dapat digunakan adalah sebegai berikut :&#x20;

```bash
docker container exec -i -t [containername/containerid] /bin/bash

#Example : 
docker  container excec -i -t nginx /bin/bash
```

* `-i` :  adalah argument interaktif menjaga input tetap aktif
* `-t` : adalah argument untuk alokasi pseudo-TTY (terminal akses)
* `bin/bash` : contoh kode program yang terdapat dalam container

### Container Port

* Saat menjalankan container, container tersebut terisolasi di dalam docker.
* Artinya system host (misal Laptop kita), tida dapat mengakses aplikasi yang ada di dalam container.
* Biasanya, sebuah aplikasi berjalan pada port tertentu, misal saat kita menjalankan aplikasi redis, dia berjalan pada port 6379, kita bisa melihat port apa yang digunakan ketika melihat semua daftar kontainer.

### Cotainer Port Forwarding

* Docker memiliki kemampuan untuk melakukan port fordwarding, yaitu menerukan sebuah port yang terdapat di system host nya ke dalam docker container.
* Cara ini cocok jika kita ingin mengakses port yang terdapat dalam kontainer ke luar melalui system hostnya.&#x20;

### Container Envinrontment Variable

* Saat membuat aplikasi, menggunakan environtment variable adalah sesuatu teknik agar kongfigurasi aplikasi bisa diubah secara dinamis.
* Dengan menggunakan environtment variabel, kita bisa mengubah-ubah kongfigurasi aplikasi, tanpa harus mengubah code aplikasinya lagi.
* Docker container memiliki parameter yang bisa kita gunakan untuk mengirim variabel aplikasi yang terdapat pada container.

### Menambah Environment Variabel

* Untuk menambah environment variable dapat menggunakan perintah berikut :&#x20;

{% code overflow="wrap" %}
```bash
docker container create --name [containername] --env KEY="value" --env KEY2="value" [image]:[tag]

#Example : 
docker container create --name mongodb --publish 27017:27017 --env MONGO_INITDB_ROOT_USERNAME="fred" --env MONGO_INITDB_ROOT_PASSWORD=fred mongo:latest
```
{% endcode %}

### Container Stats

* Saat menjalankan beberapa container, di system host penggunaan resources seperti CPU dan Memory hanya terlihat digunakan oleh docker saja.
* Kadang kita ingin  melihat detail dari penggunaan resource untuk tiap container nya.
* Untungnya docker memiliki kemampuan untuk melihat penggunaan resources dari tiap container  yang sedang berjalan.
* Kita bisa gunakan perintah :&#x20;

```bash
docker container stats
```

### Container Resources Limit

* Saat membuat  container, secara default dia akan menggunakan semua CPU dan Memory yang diberikan ke Docker (Mac dan Windows )&#x20;
* Jika terjadi kesalahan, misal container terlalu banyak memakan CPU dan Memory maka bisa berdampak terhadap performa container lain, atau bahkan ke system host.
* Oleh karena itu, ada baiknya ketika kita membuat container, kita memberikan resources limit terhadap containernya.

#### Memory

* Saat kita membuat container, kita bisa menentukan jumlah memory yang bisa digunakan oleh container ini, dengan menggunakan perintah `--memory` diikuti dengan angka memory yang diperbolehkan untuk digunakan.
* Kita bisa menambahkan ukuran dalam bentuk b (bytes), k (kilo), m (mega), atau g ( giga  bytes), misal 100m artinya 100 mega bytes.

#### CPU

* Selain mengatur memory kita juga bisa mengatur CPU dengan menggunakan parameter `--cpus`
* Kita bisa set CPU menggunakan koma. Misalnya 1.5 artinya di hanya menggunakan 1.5 core.

Berikut adalah perintah yang bisa digunakan untuk melimit resource pada container :&#x20;

{% code overflow="wrap" %}
```bash
docker container create --name [containername]  --memory [memory] --cpus [cpucore] [image]:[tag]


#Example : 
docker container create --name smallnginx --publish 8080:80 --memory 100m --cpus 0.5 nginx:latest
```
{% endcode %}



### Bind Mounts

* Bind Mounts merupakan kemampuan melakukan mounting (sharing) file atau folder yang terdapat di system host ke container  yang terdapat di docker.
* Fitur ini sangat berguna ketika kita ingin mengirim kongfigurasi dari luar container atau menyimpan data yang dibuat di aplikasi  di dalam container ke dalam folder di system host.
* Jika file atau folder tidak ada di system host, secara otomatis akan dibuatkan oleh Docker.
* Untuk melakukan mouting, kita bisa menggunakan paramater `--mount` ketika kita membuat container
* Isi dari parameter `--mount` memiliki aturan sendiri

### Paramater Mount

| Paramater   | Keterangan                                                                         |
| ----------- | ---------------------------------------------------------------------------------- |
| type        | Tipe mount, **bind** dan **volume**                                                |
| source      | Lokasi file atau folder si system host                                             |
| destination | Lokasi file atau folder di container                                               |
| readonly    | Jika ada, maka file atau folder hanya bisa dibaca di container, tidak bisa ditulis |



### Melakukan Mouting

* Untuk melakukan mouting, kita bisa menggunakan perintah berikut :&#x20;

{% code overflow="wrap" %}
```bash
docker container create --name [conntainername] --mount "type=bind,source=folder,destination=folder,readonly" [image]:[tag]

#Example : 
docker contianer create --name mongodata --mount "type=bind,source=/home/ts/mongo-data,destination=/data/db" --publish 27018:27017 --env MONGO_INITDB_ROOT_USERNAME="fred" --env MONGO_INITDB_ROOT_PASSWORD=fred mongo:latest 
```
{% endcode %}



### Continer Port

* Saat menjalankan container, container tersebut terisolasi di dalam Docker.
* Artinya sistem Host ( misal Laptop kita ), tidak bisa mengakses aplikasi yang ada di dalam container secara langsung, salah satu caranya adalah harus menggunakan COntainer Exec untuk masuk ke dalam container.
* Biasanya sebuah aplikasi berjalan pada port tertetntu, misal saatt kita menjalankan aplikasi Redis, dia berjalan pada port 6379, kita bisa melihat port apa yang digunakan ketika melihat semua daftar container.

### Port Forwarding

* Docker memiliki kemampuan untuk melakukan port forwarding, yaitu meneruskan sebuah port yang terdapat di sistem host nya ke dalam docker container.
* Cara ini cocok jika kita ingin mengekspos port yang terdapat di container ke luar melalui system hostnya.

#### Melakukan Port Fordwarding

* Untuk melakukan port forwarding, kita bisa menggunakan perintah berikut ketika membuat containernya.

```bash
docker container create --name [containername/containerid] --publish [porthost]:[portcontainer] [image]:[tag]

#Example
docker container create --name nginx --publish 8080:8080 nginx:latest
```

* Jika kita ingin melakukan port forwarding lebih dari satu, kita bisa tambahkan dua kali parameter `--publish`
* `--publish` juga dapat disingkat menjadi `-p`
* Service port aplikasi yang ada di container akan di panggil atau diakses dari host dengan menggunakan port yang bisa kita set.&#x20;
* Port host boleh sama dengan port yang ada di container selama port tersebut belum aktif atau belum digunakan oleh host.

### Container Environment Variable

* Saat membuat apliklasi, menggunakan Envinronment Variable adalah salah satu teknik agar kongfigurasi aplikasi bisa diubah secara dinamis.
* Dengan menggunakan environtment variable, kita bisa mengubah-ubah kongfigurasi aplikasi tanpa harus mengubah kode aplikasinya lagi.
* Docker container memiliki parameter yang bisa kita gunakan untuk mengirim environ ment variable ke aplikasi yang terdapat di dalam container.

###

### Menambah Environment Variable

* Untuk menambah environtment variabel, kita bisa menggunakan perintah --env atau -e

{% code overflow="wrap" %}
```bash
docker container create --name [namacontainer] --env KEY="value" --env KEY2="value" [image]:[tag]

#Example

docker container create --name nginx --env environmet="development"

#Example
docker container create --name mongodb --publish 27017:27017 --env MONGO_INITDB_ROOT_USERNAME=fred --env MONGO_INITDB_ROOT_PASSWORD=eko mongo:latest
```
{% endcode %}

### Container Stats

* Saat menjalankan beberapa container, di sistem host penggunaan resource seperti CPU dan Memory hanya terlihat digunakan oleh docker saja.
* Kadang kita ingin melihat detail dari penggunaan resources untuk tiap containernya.
* Untungnya docker memiliki kemampuan untuk melihat penggunaan resource dari tiap container yang sedang berjalan.
* Kita bisa menggunakan perintah

```bash
docker container stats
```

### Container Resource Limit

* Saat membuat container, secara default dia akan menggunakan semua CPU dan memory yang diberikan ke Docker (Mac dan WIndows), dan akan menggunakan semua CPU dan memory yang tersedia di system host (Linux)
* Jika terjadi kesalahan, misal container terlalu banyak memakan CPU dan memory maka bisa berdampak terhadap performa container lain atau bahkan ke system operasi host.
* Oleh karena itu, ada baiknya ketika kita membuat container, kita memberikan resources limit terhadap containernya.

### Memory

* saat membuat container, kita bisa menentukan jumlah memory yang bisa digunakan oleh container ini, dengan menggunakan perintah `--memory` diikuti dengan angka memory yang diperbolehkan untuk digunakan.
* Kita bisa menambhkan ukuran dalam bentuk b(bytes), k (kilo bytes), m (mega bytes), tau g( giga bytes), contoh 100m srtinya 100 megabytes

### Container CPU

* Selain mengatur memory, kita juga bisa menentukan berapa jumlah CPU yang bisa digunakan oleh container dengan parameter --cpus
* Jika misal kita set dengan nilai 1.5, artinya container bisa menggunakan satu dan setengah CPU core.

#### Contoh&#x20;

Nginx

{% code overflow="wrap" %}
```bash
docker container create --name smallnginx --publish 8080:80 --memory 100m --cpus 0.5 nginx:latest
```
{% endcode %}

### Bind Mounts

* Bind Mounts merupakan kemampuan melakukan mounting (sharing) file atau folder yang terdapat di system host ke container yang terdapat di docker
* Fitur ini sangat berguna ketika misal kita ingin mengirim konfigurasi dari luar container atau misal menyimpan data yang dibuat di aplikasi di dalam container kedalam folder di system host.
* Jika file dan folder tidak ada di system host, secara otomatis akan dibuatkan oleh Docker
* Untuk melakukan mounting, kita bisa menggunakan parameter --mount ketika membuat container
* Isi parameter --mount memiliki aturan sendiri

### Parameter Mount



<table><thead><tr><th width="149">Parameter</th><th>Keterangan</th></tr></thead><tbody><tr><td>type</td><td>Tipe mount, bind atau volume</td></tr><tr><td>source</td><td>Lokasi file atau folder di system host</td></tr><tr><td>destination</td><td>Lokasi file atau folder di container</td></tr><tr><td>readonly</td><td>Jika ada, maka file atau folder hanya bisa dibaca di container, tidak bisa ditulis.</td></tr></tbody></table>

### Melakukan Mounting

* Untuk melakukan mounting, kita bisa menggunakan perintah berikut :&#x20;

{% code title="" overflow="wrap" fullWidth="false" %}
```bash
docker container create --name [containername] --mount "type=bind,source=[folder],destination=[folder],readonly" [image]:[tag]

#example
docker container create --name 
```
{% endcode %}

## Docker Volume

* Fitur Bind Mounts sudah ada sejak Docker versi awal, di versi terbaru direkomendasikan menggunakan Docker Volume.
* Docker Volume mirip dengan Bind Mounts, bedanya adalah terdapat mangamenet Volume dimana kita bisa membuat volume, melihat daftar Volume dan menghapus Volume.
* Volume sendiri bisa dianggap storage yang digunakan untuk menyimpan data, bedanya dengan Bind Mounts pada bind mounts data disimpan pada sistem host, sedangkan pada Volume data di manage oleh Docker.

### Melihat Docker Volume

* Saat kita membuat container, dimanakah data di dalam container itu disimpan, secara default semua container disimpan di dalam volume.
* Jika kita coba melihat docker volume, kita akan lihat bahwa ada banyak volume yang suddah terbuat walapun kita belum pernah membuatnya sama sekali.
* Kita bisa gunakan perintah berikut untuk melihat daftar volume.

```bash
docker volume ls
```

### Membuat Volume

* Untuk membuat volume, kita bisa menggunakan perintah :&#x20;

```bash
docker volume create [namavolume]

#Example : 
docker volume create mongovolume
```

### Menghapus Volume

* Volume yang tidak digunakan oleh container bisa kita hapus, tapi jika volume digunakan oleh container, maka tidak bisa kita hapus sampai containernya dihapus.
* Untuk menghapus volume, kita bisa gunakan perintah  berikut :&#x20;

```bash
docker volume rm [namavolume]

#Example : 
docker volume rm mongovolume
```

### Container Volume

* Volume yang sudah kita buat, bisa kita gunakan di container.
* Keuntungan menggunakan volume adalah, jika container kita hapus, data akan tetap aman di volume.
* Cara menggunakan volume di container sama dengan menggunakan bind mount, kita bisa menggunakan parameter --mount, namun dengan menggunakan type volume dan source nama volume.
* Berikut adalah perintah untuk membuat container volume&#x20;

{% code overflow="wrap" %}
```bash
docker container create --name [containername] --mount "type=volume,source=[namavolume],destination=/data/db" [images]:[tags]

#example :
docker container create --name mongodb --mount "type=volume,source=mongodata,destination=/data/db" --publish 27019:27019 --env MONGO_ROOT_USERNAME=fred --env MONGO_INITDB_ROOT_PASSWORD=fred mongo:latest
```
{% endcode %}

### Backup Volume

* Sayangnya sampai saat ini, tidak ada cara otomatis untuk melakukan backup volume yang sudah kita buat
* Namun kita bisa memanfaatkan container untuk melakukan backup data yang ada di dalam volume ke dalam archive seperti zip atau tar.gz

### Tahapan Melakukan Backup

* Matikan container yang menggunakan volume yang ingin kita backup
* Buat container baru dengan dua mount, volume yang ingin kita backu dan bind mount folder dari sistem host.
* Lakukan backup mengguanakn container dengancara mengarchive isi volume dan simpan di bind folder
* Isi file backup sekarang ada di folder di system host
* Delete container yang kita gunakan untuk melakukan backup
* Contoh Script Backup

{% code overflow="wrap" %}
```bash
dokcer container create --name nginxbackup --mount "type=bind,source=/home/fred/backup,destination=/backup" --mount "type=volume,source=mongodata,destination=/data" nginx:latest
```
{% endcode %}



### Menjalankan Container Secara Langsung

* Melakukan backup secara manual agak sedikti ribet karena kita harus start container terlebih dahulu, setelah backup hapus containernya lagi.
* Kita bisa menggunakan perintah run untuk menjalankan perintah di container dan digunakan parameter --rm untuk melakukan otomatis remove container setelah perintahnya selesai berjalan.
* Berikut contoh code untuk melakukan backup dengan container run :&#x20;

{% code overflow="wrap" %}
```bash
docker container run --rm --name ubuntu --mount "type=bind,source=/home/fred/backup,destination=/backup" --mount "type=volume,source=mongodata,destination=/data" ubuntu:latest tar cvf /"backup/backup.tar.gz /data
```
{% endcode %}

### Restore Volume

* Setelah melakukan backup volume ke dalam file archive, kita bisa menyimpan file archive backup tersebut ke tempat yang lebih aman, misal ke cloud storage.
* Sekarang kita akan coba melakukan restore data backup ke volume baru, untuk memastikan data backup yang kita lakukan tidak corrupt.

### Tahapan Melakukan Restore

* Buat volume baru unuk lokasi restore data backup
* Buat container baru dua mount, volume baru untuk restore backup dan bind mount folder dari system host yang berisi file backup.
* Isi file backup sekarang sudah di restore ke volume
* Delete container yang kita  gunakan untuk melakukan restore
* Volume baru yang berisi data backup siap digunakan oleh caontainer baru.
* Berikut contoh kode untuk melakukan restore backup :&#x20;

{% code overflow="wrap" %}
```bash
docker volume create mongodatabackup

docker container run --rm --name ubuntu --mount "type=bind,source=/home/fred/backup,destination=/backup" --mount "type=volume,source=mongodatabackup,destination=/data" ubuntu:latest bash -c "cd /data && tar xvf /backup/backup.tar.gz --strip 1"

```
{% endcode %}

## Docker Network

### Dokcer Network

* Saat kita mebuat container di docker, secara default container akan saling terisolasi stau sama lain, jadi jika kita mencoba memanggil antar container,bsia dipastikan bahwa kita tidak akan bisa melakukannya.
* Docker memiliki fitur Network yang bisa digunakan untuk membuat jaringan di dalam Docker&#x20;
* Dengan menggunakan Network, kita bisa mengkoneksikan container dengan container lain dalam satu network yang sama.
* Jika beberapa container terdapat pada satu network yang sama, maka secara otomatis container tersebut bisa saling berkomunikasi.

### Network Driver

* Saat kitamembuat Network di docker, kita perlu menentukan driver yang ingi kita gunakan, ada banyak driver yang bisa kita gunakan, tapi kadang ada syarat sebuah driver network bisa kita gunakan.
* Bridge, yaitu driver yang digunakan untuk mebuat network secara virtual yang memungkinan container yang terkoneksi di bridge network yang sama saling berkomunikasi.
* Host, yaitu driver yang digunakan untuk membuat network yang sama dengan sistem host. Host hanya jalan di Docker Linux, tidak bisa digunakan di mac atau windows.
* none, yaiut driver untuk membuat network yang tidak bisa berkomunikasi

### Melihat Network

untuk melihat network di docker, kita bisa gunakan perintah :&#x20;

```bash
docker network ls
```

### Membuat Network

* Untuk membuat network baru, kita bisa gunakan perintah :&#x20;

```bash
docker network create --driver [drivername] [networkname]

#Example : 

docker network create --driver bridge network1
```

### Menghapus Network

* Untuk menghapus network, kita bisa gunakan perintah  :&#x20;

```bash
docker container rm [networkname]

#Example :
docker container rm network1
```

* Network tidak bisa dihapus jika sudah digunakan oleh container. Kita harus menghapus containernya terlebih dahulu dari network.

### Membuat Container dengan Nework

* Untuk menambahkan container ke network, kita bisa menambahkan perintah --network ketika membuat container.
* Berikut contoh code untuk menambahkan network :&#x20;

{% code overflow="wrap" %}
```bash
docker container create --name [containername] --network [networkname] [image]:[tag]
```
{% endcode %}

Example :&#x20;

{% code overflow="wrap" %}
```bash
docker network create mongonetwork

docker container create --name mongodb --network mongonetwork --env MONGO_INITDB_ROOT_USERNAME=fred --env MONGO_INITDB_ROOT_PASSWORD=fred mongo:latest

docker container create --name mongodbexpress --network mongonetwork --publish 8081:8081 --env ME_CONFIG_MONGODB_URL="mongodb://fred:fred@mongodb:2701/" mongo-express:latest

docker container start mongodb

docker container start mongodbexpress

```
{% endcode %}

### Menghapus Container dari Network

* Jika diperlukan, kita juga bisa menghapus container dari network dengan perintah :&#x20;

```bash
docker network disconnect [networkname]
```

### Menghapus Container dari Network

* Jika diperlukan, kita juga bisa menghapus container dari network dengan perintah :&#x20;

```bash
docker container disconnect [networkname] [containername]
```

### Menghapus Container dari Network

```bash
docker network disconnect mongonetwork mongodb
```

### Menambahkan Container ke Network

* Jika containernya sudah terlanjut dibuat, kita juga bisa menambahkan container yang sudah dibuat ke netwrok dengan menggunakan perintah berikut :&#x20;

```bash
docker network connect [networkname] [containername]
```

Example :&#x20;

```bash
docker network connect mongonetwork mongodb
```

## Inspect

* Setelah kita mendownload image, atau membuat network, volume dan container. Kadang kita ingin melihat detail dari tiap hal tersebut.
* Misal kita ingni melihat detail dari image, perintah apa yang digunakan oleh image tersebut? Environtment variabel apa yang digunakan? Atau port apa yang digunakan?
* Misal kita juga ingin melihat detail container, Volume apa yang digunakan? Envinroment variabel apa yang digunakan ? Port forwarding apa yang digunakan? dan lain-lain.
* Docker memiliki fitur yang bernama inspect, yang bisa digunakan di image, container, volume dan network.
* Dengan fitur ini, kita bisa melihat detail dari tiap yang ada di Docker.

### Menggunakan Inspect

* Untuk melihat detail dari image, gunakan perintah berikut :&#x20;

```bash
docker image inspect [imagename]
```

* Untuk melihat detail dari container

```bash
docker container inspect [containername]
```

* Untuk melihat detail volume :&#x20;

```bash
docker volume inspect [volumename]
```

* Untuk melihat detail dari network, gunakan :&#x20;

```bash
docker network inspect [networkname]
```

* Contoh code menggunakan ispect :&#x20;

```bash
docker container inspect redist:latest
```

## Prune

* Saat kita menggunakan Docker, kadang kalanya kita ingin membersihkan hal-hal yang sudah tidak digunakan lagi di Docker, misal container yang sudah di stop, image yang tidak digunakan oleh container, atau volume yang tidak digunakan oleh container.
* Fitur untuk membersihkan secara otomatis di Docker bernama prune.
* Hampi di semua perintah di docker mendukung prune.

### Perintah Prune

* Perintah untuk menghapus semua container yang sudah stop :&#x20;

```bash
docker container prune
```

* Perintah untuk menghapus semua image yang tidak digunakan container :&#x20;

```bash
docker image prune
```

* Perintah untuk menghapus semua network yang tiidak digunakan oleh container :&#x20;

```bash
docker network prune
```

* Perintah untuk menghapus volume yang tidak digunakan container :&#x20;

```bash
docker volume prune
```

* Atau kita bisa menggunakan satu perintah untuk menghapus container,network, dan image yang sudah tidak digunakan menggunakan perintah :&#x20;

```bash
docker system prune
```
