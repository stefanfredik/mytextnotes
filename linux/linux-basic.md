---
description: All About Basic of Linux
---

# Linux Basic

## Linux Basic

### User

#### Root User

Root user adalah user paling tinggi yang memiliki akses penuh pada system linux.

Root user memiliki directory home dengan nama /root

```bash
root@kopi-flores#
```

#### User Biasa / Regular User

User biasa adalah user yang digunakan untuk kebutuhan standar di dalam Linux. Akses reguler user memiliki keterbatasan dan tidak memiliki askses penuh ke dalam system.

Reguler user memiliki directory home dengan nama user **`/home/nama_user`**

#### System User

Merupakan user yang dibuat oleh aplikasi tertentu untuk kebutuhan aplikasi tersebut.

System user tidak memiliki directory home. Id system user biasanya 1-999.

Tipe user ini tidak dapat digunakan oleh pengguna biasa. Sehingga kita tidak dapat login ke user tersebut.

Contoh : _`www-data`_   digunakan oleh aplikasi Apache2.



#### Cek Info User

Mengecek informasi id dari user

```bash
id [namauser]
```

### System

#### Command

```bash
losetup
```

Merupakan sebah perintah untuk melihat list loop device

```bash
lsbk
```

Melihat informasi disk pada device linux.

#### fsdisk

Merupakan perintah untuk mengelolah disk pada system linux.

#### parted

Merupakan perintah alternatif dari fsdisk

#### mkfs

Perintah yang digunakn untuk mengelolah file system, termasuk menformat partisi.

#### df

Merupakan perintah yang digunakan untuk melihat informasi ruang penyimpanan disk

```bash
bash f -h    #untuk menampilakan list informasi ruang penyimpanan disk yang format human (mudah dibaca)
```

#### file

Digunakan untuk menampilkan/ mengidentifikasi jenis file.

```bash
file [namafile]
```

#### stat

Perintah stat memberikan informasi seperti ukuran file, izin akses, ID user dan ID group, waktu akses, serta waktu lahir file. Perintah stat memiliki fitur lain yang juga dapat memberikan informasi filesystem. Perintah ini baik digunakan ketika kita menginginkan informasi dari file apa pun.

```bash
stat [namafile]
```



### Swith LTS to Normal Mode

Open File

```bash
sudo nano /etc/update-manager/release-upgrades

#change 
Prompt=lts

#to
Prompt=normal
```

Upgrade System

```
sudo do-release-upgrade -c
sudo do-release-upgrade
```

## Checking Your Login With Whoami

Di dalam linux, root adalah user yang sangat powerfull, dengan user root kita bisa menambahkan banyak user, ganti paswword, merubah hak akses, dan masih banyak lagi. Tentu anda tidak ingin ada user lain yang memiliki akses dengan kemampun user root. Sebagai seorang hacker, kita tentu ingin mendapatkan semua akses tersebut. Kita bisa menggunakan perintah whoami untuk mengetahu user yang ada saat ini.

```bash
whoami
```

## Navigasi System Linux

Untuk berpindah atau berganti direktori pada terminal linux kita dapat menggunakan perintah change directory atau cd

#### Pindah Folder/ Direktory

```bash
#Masuk ke folder tertentu
cd [namafolder]
```

### Berpindah ke folder sebelumnya

Untuk berpindah ke beberapa tingkat folder sebelumnya dapat menggunakan perintah berikut :&#x20;

```bash
#Kembali ke tingkat folder sebelumnya
cd ..

#Kembali ke 2 tingkat folder sebelumnya
cd .. ..

#Kembali ke 4 tingkat folder sebelumnya
cd .. ... ..
```

## Menampilkan list file atau folder menggunaakan ls

Dengan menggunakan perintah ls kita dapat menampilkan daftar/list file dan folder yang ingin kita tampilkan.

Berikut cara penggunaan perintah ls berseta opsinya

### Menampilkan folder dan file pada lokasi saat ini.

Kita dapat menggunakan perintah berikut untuk menampilkan daftar file dan folder pada lokasi kita saat ini.

```bash
ls
```

### Menampilkan folder dan File pada Folder Tertertu

Dari lokasi saat ini, kita juga bisa menampilkan file dan folder yang ada di dalam folder atau lokasi tertentu. Berikut adalah contoh perintah yang dapat digunakan.

```bash
#menampilkan listing pada satu folder yang ada di dalam lokasi saat ini.
ls [namafolder]

#menampilkan isi file dan folder pada lokasi tertentu

ls [lokasi/path]
#Example : 
ls /home/fred/documents/

```

### Opsi&#x20;

Kita dapat menggunakan perintah ls dengan berabagai opsi sesuai kebutuhan kita. Beberapa opsi yang digunakan dalam perintah ls adalah sebagai berikut :&#x20;

* **-a** : Menampilkan semua file dan folder, termasuk yang tersembunyi. Contoh: `ls -a`
* **-l** : Menampilkan file dan folder dengan format panjang, termasuk izin, jumlah link, pemilik, grup, ukuran, dan tanggal modifikasi. Contoh: `ls -l`
* **-h** : Saat digunakan dengan opsi -l, menampilkan ukuran file dalam format yang lebih mudah dibaca manusia (misalnya, 1K, 234M, 2G). Contoh: `ls -lh`
* **-r** : Membalik urutan hasil, yang berguna saat ingin melihat file terbaru atau terlama terlebih dahulu. Contoh: `ls -lr`
* **-t** : Mengurutkan file dan folder berdasarkan waktu modifikasi terakhir, dengan file yang paling baru muncul pertama kali. Contoh: `ls -lt`

## Mempelajari perintah menggunakan manual page

Di dalam linux kita bisa mempelajari manual atau cara menggunakan perintah beserta opsinya menggunakan perintah man. Dengan man kita bisa melihat penjelasan, fungsi dan cara penggunaan perintah yang ingin kita gunakan. Berikut contoh menggunakan man.

```bash
man [namaperintah]

#Example : menampilkan manual pages dari ls
man ls
```

## Fitur Pencarian

Di dalam linux kita kadang pusing saat ingin menemukan dimana letak perintah atau aplikasi atau file system yang ingin kita gunakan. Berikut adalah beberapa cara atau perintah yang bisa digunakan untuk menemukan atau mencari file, direktory, binary atau aplikasi pada linux menggunakan command line.

### Mencari berdasarkan lokasi

Perintah yang paling mudah kita gunakan adalah perintah locate. Kita dapat menggunakan perintah locate untuk menemukan lokasi dari file/aplikasi yang ingin kita cari di dalam keseluruhan system.

```bash
locate [katakunci]

#Example : 
locate aircrack-ng
```

Perintah tersebut kadang memberikan terlalu banyak informasi yang berlebihan dan tidak kita butuhkan, karena perintah locate akan menemukan file dan folder apapun yang memiliki kata kunci sama sesai yang kita cari.

### Mencari Binary dengan menggunakan whereis

Kita bisa mencari file binary menggunakan perintah whereis. Binary file adalah file binary yang bisa dieksekusi atau dengan kata lain adalah perintah/aplikasi dari linux itu sendiri.

Untuk mencari lokasi dari file biner suatu program, Anda bisa menggunakan perintah `whereis`. Ini sangat berguna untuk mengetahui di mana program tersebut diinstal di sistem Linux Anda. Berikut adalah format umum dari penggunaan perintah ini:

```bash
whereis [nama_program]
# Contoh:
whereis nginx
```

Perintah ini akan mengembalikan path lokasi dari binary, source, dan man page (jika ada) dari program yang Anda cari. Ini jauh lebih spesifik dibanding menggunakan perintah `locate` karena `whereis` dikhususkan untuk mencari binary files, source files, dan manual pages saja.

### Mencari Binary berdasarkan lokasi/path menggunakan perintah which

Setelah memahami cara menggunakan perintah `whereis` untuk menemukan file biner, source, dan man pages, kita dapat mempelajari cara mencari binary berdasarkan lokasi/path menggunakan perintah `which`. Perintah `which` digunakan untuk menemukan lokasi eksekusi dari perintah yang kita gunakan dalam shell. Ini hanya mengembalikan lokasi dari binary file saja.

Format penggunaan perintah ini cukup sederhana:

```bash
which [nama_program]
# Contoh:
which nginx
```

Perintah ini akan mengembalikan path lokasi binary dari program yang Anda cari. Sangat berguna ketika Anda ingin mengetahui versi executable mana yang dipanggil saat Anda menjalankan suatu program dari terminal.

Jika Anda menjalankan beberapa versi dari program yang sama, perintah `which` akan menunjukkan path dari versi yang akan dipanggil pertama kali berdasarkan `$PATH` environment variable Anda. Ini membantu dalam memastikan bahwa Anda menggunakan versi program yang tepat.



### Mencari File menggunakan Find

Perintah find adalah perintah utilitas yang sangat powerful dan flexible. Kita bisa menemukan apa saja dengan find menggunakan berbagai parameter yang berbeda seperti menggunakan nama file, tanggal atau waktu pembuatan atau modifikasi file, user owner, group user, permission, dan ukuran.

Berikut adalah perintah dasar menggunakan find :&#x20;

```bash
find [namadirektory] option expression
```

Sebagai contoh saya ingin mencari nama apache2 dengan titik pencarian awal adalah pada folder root. Maka perintah yang bisa digunakan adalah sebagai berikut :&#x20;

```bash
find / -type f -name apache2
```

Perintah ini mencari mulai dari direktori root (`/`) untuk file (`-type f`) dengan nama `apache2`. Perintah tersebut akan melalui semua direktori di bawah root, sehingga mungkin memerlukan waktu tergantung pada ukuran sistem file.

Opsi dan ekspresi dapat digabungkan untuk pencarian yang lebih spesifik. Misalnya, untuk menemukan file yang dimodifikasi dalam 7 hari terakhir:

```bash
find / -type f -name apache2 -mtime -7
```

Opsi `-mtime -7` membatasi pencarian pada file yang dimodifikasi dalam 7 hari terakhir.

### Filtering menggunkan grep

Ketika menggunakan command line, kita kadang ingin mencari keyword tertentu. Kita bisa menggunakan perintah grep untuk melakukan hal tersebut.

Perintah grep sering digunakan ketika outputnya dapat di salurkan/digabungkan dengan perintah lainya. Didalam linux ada namanya pipiing. Piping adalah proses menggabungkan lebih dari satu perintah dan menghasilkan satu output. Untuk menggunakan piping kita dapat menggunakan karakter | sebagai pemisah dari 2 atau lebih perintah yang berbeda. Kita bisa menggunakan grep untuk dikombinasikan dengan perintah lainya.

Ps adalah perintah yang digunakan untuk menampilkan proses yang terdapat pada system operasi linux.&#x20;

```bash
pas aux
```

Perintah diatas akan menampilkan semua proses yang ada pada system. Tapi bagaimana caranya agar menampilkan proses-proses yang kita ingin tampilkan saja ? Kita bisa menggunakan kombinasi grep.

Berikut adalah perintah yang bisa digunakan :&#x20;

Selain itu, untuk lebih memperluas pencarian Anda atau mengurangi hasil yang ditampilkan, Anda dapat menggunakan wildcard atau regex dalam grep. Misalnya, jika Anda ingin mencari semua proses yang berhubungan dengan nginx, Anda bisa menggunakan:

```bash
ps aux | grep nginx
```

Atau jika Anda ingin mencari semua proses yang dimulai dengan kata 'http', Anda dapat menggunakan:

```bash
ps aux | grep "^http"
```

Dengan menggunakan karakter `^` dalam contoh terakhir, Anda memberi tahu grep untuk mencocokkan ekspresi reguler yang dimulai dengan "http", memastikan bahwa pencarian Anda lebih spesifik.

Grep dan pipelining adalah alat yang sangat ampuh dalam lingkungan Unix/Linux, memungkinkan pembuatan rangkaian operasi pencarian dan penyaringan output yang kompleks dengan mudah.

## Mengelolah File dan Folder

Ketika kita sudah mendapatkan file yang ingin kita cari, tentu kita ingin memodifikasi file atau direktori tersebut. Seperti merubah nama, hak askes, copy, pindah atau hapus file/folder tersebut.

### Membuat File Baru

Begitu banyak cara untuk membuat file baru pada Linux, namun kita bisa menggunakan beberapa cara yang sederhana.

Yang pertama adalah cat. Fungsi utama cat adalah menampilkan isi sebuah file. Namun kita juga bisa membuat file menggunakan perintah cat, biasanya file-file yang berukuran kecil atau file text. Untuk file yang lebih besar kita bisa menggunakan text editor.

#### Penggabungan menggunakan Cat

Perintah cat lalu diikuti namafile biasanya menampilkan isi atau text dari file text tersebut. Kita bisa tambahkan simbol > dan diikuti dengan nama file maka otomatis akan membuat file baru.

Berikut adalah contoh membuat file baru menggunakan cat :&#x20;

```bash
cat > namafilebaru.txt
```

Setelah mengetik perintah di atas, tekan `Enter`. Anda akan memasuki mode penulisan. Ketik teks yang ingin Anda simpan di dalam file, kemudian tekan `Ctrl` + `D` untuk menyimpan dan keluar.

Jika kita menggunakan double simbol >> pada file sebelumnya, maka text yang kita input tidak akan menimpa text atau isi file sebelumnya. Cat akan otomatis menambahkan text baru yang kita ketik pada baris terakhit. Berikut adalah contohnya :&#x20;

To view the contents of a file without making any changes, use the simple `cat` command followed by the file name:

```bash
cat namafile.txt
```

This command will display the contents of `namafile.txt` on your screen, allowing you to quickly inspect its contents without opening it in a text editor. This is particularly useful for checking file contents on the fly in a command line environment.

### Membuat file baru dengan touch

Selain menggunakan cat, kita juga bisa membuat file baru menggunakan perintah touch dengan simpel.

Berikut cara menggunakan touch :&#x20;

```bash
touch namabaru.txt
```

Jika namafile belum ada, maka touch akan otomasis membuat file baru sesuai dengan nama yang kita tentukan.

### Membuat folder baru

Kita dengan gampang membuat folder baru dengan menggunakan perintah dir.

Berikut cara menggunakan perintah dir :&#x20;

```bash
mkdir namafolderbaru
```

Perintah ini akan membuat folder baru dengan nama yang Anda tentukan, dalam hal ini adalah `namafolderbaru`. Sangat sederhana dan mudah digunakan untuk organisasi file dan direktori Anda.

Kita juga dapat membuat folder lebih dari satu sekaligus dengan satu baris perintah&#x20;

Untuk membuat beberapa folder sekaligus, Anda bisa menambahkan semua nama folder yang ingin dibuat dalam satu perintah `mkdir` dengan memisahkan setiap nama dengan spasi. Berikut adalah contohnya:

```bash
mkdir folder1 folder2 folder3
```

Perintah di atas akan membuat tiga folder baru dalam direktori saat ini dengan nama `folder1`, `folder2`, dan `folder3`. Ini adalah cara yang efisien untuk membuat banyak folder sekaligus tanpa perlu menjalankan perintah `mkdir` berulang kali.

#### Mengganti nama file atau folder

Untuk mengganti nama file atau folder, Anda dapat menggunakan perintah `mv`. Cara menggunakan perintah ini cukup sederhana, berikut contohnya:

```bash
mv namalama.txt namabaru.txt
```

Perintah di atas akan mengganti nama file `namalama.txt` menjadi `namabaru.txt`. Sama halnya dengan file, Anda juga bisa mengganti nama folder menggunakan perintah yang sama:

```bash
mv namafolderlama namafolderbaru
```

Perlu diingat bahwa perintah `mv` tidak hanya digunakan untuk mengganti nama, tapi juga dapat digunakan untuk memindahkan file atau folder ke lokasi lain.

### Copy File

Kita bisa dengan mudah mengcopy file atau folder pada linux dengan menggunakan perintah cp.

Berikut cara penggunaan cp pada linux :&#x20;

Untuk mengcopy sebuah file dari satu lokasi ke lokasi lainnya, gunakan perintah berikut:

```bash
cp namafileasal lokasitujuan
```

Jika Anda ingin mengcopy sebuah folder beserta seluruh isinya, Anda perlu menambahkan opsi `-r` (yang berarti recursive) seperti ini:

```bash
cp -r namafolderasal lokasitujuan
```

Perintah ini akan mengcopy folder `namafolderasal` dan semua file serta subfolder di dalamnya ke `lokasitujuan`.

Contoh :&#x20;

```bash
cp filebaru.txt /home/fred/Documents
```

### Mengubah  nama file

Untuk mengubah file kita bisa menggunakan perintah mv.

Perintah mv pada dasarnya berfungsi untuk memindahkan file atau folder. Namun dengan perintah mv kita juga bisa mengubah nama file atau folder. Berikut cara penggunaan mv untuk mengubah nama atau folder.

```bash
mv [nama file/folder awal] [nama file/folder tujuan]

#Example : 
mv filebaru.txt filerevisi.txt

mv Doc Documents
```

### Memindah file atau folder

Untuk memindah file dan folder kita bisa menggunakan perintah yang sama untuk mengubah nama yaitu perintah mv.

Berikut cara penggunaan mv untuk memindah file atau folder :&#x20;

```bash
mv [nama file/folder] [lokasi tujuan]

#Example 1 :
mv document.pdf /home/fred/Documents

#Example 2 : 
mv /home/fred/project /home/fred/Documents
```

### Menghapus File

Untuk menghapus file pada Linux kita bisa menggunakan perintah rm

```bash
rm [namafile]

#Example
rm documents.pdf
```

Kita juga bisa menghapus folder dengan perintah yang sama. Namun untuk menghapus folder kita bisa menambah sedikit opsi -r yang berrati recursive agar bisa menghapus isi dan folder yang terdapat didalamnya :&#x20;

```bash
rm -r [namafolder]

#Example : 
rm -r Project
```



## Manipulasi Text

Di dalam linux, kebanyakan file system, aplikasi dan kongfigurasi berupa text. Jadi sangat penting kita mengetahui bagaimana memanipulasi text.

### Menampilkan isi file

Sebelumnya kita sudah mengetahui cara menampilkan file dengan menggunakan cat.

Dengan menggunakan cat kita bisa melihat semua isi dari sebuah file dan akan di tampilkan dalam bentuk text.

```bash
cat [namafile]

#Example
cat /etc/snort/snort.conf
```

### Menampilkan bagian header file text

Untuk menampilkan sebagian header dari sebuah text atau file kita bisa menggunakan perintah head. Secara default perintah tersebut hanya menampilkan 10 baris dari isi text atau file.

```bash
head [namafile]

#Example
head /etc/passwd
```

### Menampilkan beberapa baris text dengan tail

Jika menggunakan head kita hanya bisa menampilkan 10 baris text teratas, dengan tail kita bisa menampilkan jumlah baris sesuai dengan yang kita inginkan.

Berikut cara penggunaan tail

```bash
tail -[baris] [text/file]

#Example
tail -15 /etc/passwd
```

### Menampilkan nomor baris pada text

Kita bisa menampilkan nomor/urutan baris  pada text atau file dengan menggunakan perintah ln. Dengan menggunakan ln kita bisa menampilkan nomor baris dari setiap text yang ingin kita tampilkan.

```bash
nl [namafile]

#Example
nl /etc/passwd
```

### Menfilter text dengan grep

Perintah grep memiliki fungsi yang sangat powerfull dalam memanipulasi text. Kita bisa menfilter konten dari sebuah file untuk ditampilkan.&#x20;

Berikut contoh cara menggunakan grep dalam menfilter text.

```bash
cat /etc/snort/snort.conf | grep output
```

Perintah tersebut pertama kali akan mengambil isi atau konten dari file snort.conf dan menggunakan simbol pipi ( | ) untuk dikirimkan ke ke grep sebagai input dengan keyword output dan akan menamilkan baris yang mengandung kata output. Perintah grep sangat berguna dan powerfull ketika kita menggunakan linux, karena kita bisa menghemat waktu dalam mencari baris text dalam sebuah file dengan menggunakan kata kunci tertentu. &#x20;

### Tantangan menjadi hacker : Menggunakan grep, tail, nl, dan head.

Ada beberapa cara untuk menampilkan beberapa baris text.

Cara 1

```bash
nl /etc/snort.conf | grep output
```

Cara 2 :&#x20;

```bash
tail -n+507 /etc/snort/snort.conf | head -n 6
```

Diantara cara-cara untuk menampilkan beberapa baris teks, dua metode telah dijelaskan di atas. Berikut penjelasannya:

#### Cara 1

Perintah:

```bash
nl /etc/snort.conf | grep output
```

* `nl`: Menambahkan nomor baris pada setiap baris dari output file `/etc/snort.conf`.
* `|`: Pipa, digunakan untuk mengirimkan output dari perintah sebelumnya (`nl`) sebagai input ke perintah berikutnya.
* `grep output`: Mencari baris yang mengandung kata `output`. Hanya baris yang cocok yang ditampilkan.

#### Cara 2

Perintah:

```bash
tail -n+507 /etc/snort/snort.conf | head -n 6
```

* `tail -n+507`: Menampilkan semua baris dari file `/etc/snort/snort.conf` dimulai dari baris ke-507 hingga akhir file.
* `head -n 6`: Dari output `tail`, perintah ini kemudian mengambil 6 baris pertama untuk ditampilkan.

Kedua cara tersebut dapat digunakan tergantung pada kebutuhan spesifik Anda; Cara 1 lebih bermanfaat untuk mencari teks tertentu, sedangkan Cara 2 bagus untuk menampilkan potongan teks dari posisi tertentu dalam file.

### Menggunakan perintah sed sebagai find and replace

Perinta sed akan mencari text dengan mengguanakan pola tertentu dan akan menimpanya dengan text baru. Istilah tersebut dinamakan sebagai kontraksi dari stream editor, karena mengikuti konsep stream editor. Pada dasarnya, sed bekerja seperti find and replace pada text editor windows.

Berikut adalah contoh penggunaa sed.&#x20;

Mencari kata mysql pada file snort.conf menggunakan perintah grep :&#x20;

```bash
cat /etc/snort/snort.conf | grep mysql
```

Kita akan akan menemukan 2 baris teks yang berhubungan dengan mysql.

Mari kita coba menimpa setiap kata yang mengandung mysql dengan kata MySQL ( Ingat linux menganut case sensitif) dan menyimpannya pada file baru dengan nama snort2.conf. Kita bisa menggunakan perintah berikut :&#x20;

```bash
sed /s/mysql/MySQL/g /etc/snort/snort.conf > snort2.conf
```

Perintah `sed` yang digunakan dalam contoh di atas adalah untuk menemukan dan mengganti setiap kemunculan kata "mysql" dengan "MySQL" dalam file `snort.conf` yang berada pada direktori `/etc/snort/`. Penggantian ini bersifat case-insensitive, yang berarti tidak memperhatikan huruf besar atau kecil saat mencari kata "mysql". Hasil penggantian tersebut kemudian disimpan dalam file baru bernama `snort2.conf`.

Singkatnya, perintah ini mengambil isi dari file `snort.conf`, mencari kata "mysql", menggantinya dengan "MySQL", dan menyimpan outputnya ke dalam file baru yang bernama `snort2.conf`.

Jika ingin menimpa text mysql pertama saja tanpa harus menimpa keseluruhan text, kita bisa menggunakan perintah berikut :&#x20;

```bash
sed s/mysql/MySQL/ /etc/snort/snort.conf > snort2.conf
```

Kita juga bisa menggunakan perintah tersebut dengan menggunakan kondisi kondisi tertentu seperti spesifik pada baris baris tertentu saja. Contoh. Kita hanya ingin menimpa mysql pada baris atau urutan kedua saja kita bisa gunakan perintah berikut :&#x20;

```bash
sed s/mysql/MySQL/2 /etc/snort/snort.conf > snort2.conf
```

Perintah tersebut hanya akan menimpa text mysql pada baris atau urutan kedua saja.

### Menampilakan file dengan more dan less

Walaupun cat merupakan perintah yang cukup bagus untuk menampilkan dan membuat file baru, sebenranya dia memiliki batasan ketika menampilkan file atau text yang cukup besar ukuranya. Ketika kita mencoba menampilkan file snort.conf dengan perintah cat, maka terminal kita akan terus melakukan scroling sampai text terbaca dengan semuanya. Tentu sangat tidak efisien jika kita hanya ingin menampilkan bagian tertentu saja. Oleh karena itu kita bisa menggunakan more dan less untuk menampilkan file lebih atraktif.

### Mengontrol tampilan text dengan more

Dengan menggunakan perintah more, maka file atau text  hanya akan ditampilkan sebagian saja dari total keseluruhan text atau file. File akan ditampilkan per halaman sesuai dengan ukuran terminal yang kita gunakan. Kita dapat menggunaan enter untuk melanjutkan ke halaman atau bagian berikutnya. Untuk menggunakan more sama seperti menggunakan perintah cat.

```bash
more [namafile]

#Example 
more /etc/snort/snort.conf
```

Perintah more hanya akan menampilkan halaman pertamanya sajal lalu text akan berhenti, dan pada akhir text akan ditampilkan informasi berapa persen isi file tersebut sudah ditampilkan pada layar. Untuk melanjutkan menampilkan isi text dapat menggunakan tombol enter dan menekan tombol q untuk exit atau kembali.

### Memfilter dan menampilkan text dengan less

Perintah less sangat mirip dengan perintah more, tapi memiliki fungsi tamabahan. Ketika mmenggunakan perintah less, maka terminal akan menampilkan promp interaktif. Ketika kita mengetik tanda slash ( / ) pada promp interkatif lalu diikuti text, makan terminal akan melakukan pencarian dan menampilkan hasl pencarian langsung pada text dengan cara hightligh text. Contoh kita mengetikan /output, maka terminal akan menghighlight text output yang terdapat pada text atau file tersebut.





## Menganalisa dan Mengatur Jaringan

Memahami jaringan sangat penting bagi siapa saa yang ingin menjadi hacker. Dalam banyak situasi kita akan melakukan proses hacking melalui jaringan dan seorang hacker yang baik perlu mengetahui cara terhubung dan berinteraksi dengan jaringan. Sebagai contoh, kita mungkin ingin menghubngkan komputer dengan IP Address tanpa terdeteksi, atau mungkin kita mengalihkan sebuah DNS target pada system kita. Itu merupakan aktifitasi yang sangat sederhana namun kita perlu mengetahui pengetahuan dasar tentang jaringan bagaimana jaringan bekerja. Berikutnya kita akan mempelajari tools linux untuk menganalisa dan mengatur jaringan dalam proses perjalan meretas sebuah jaringan.

### Analisa jaringan menggunakan ifconfig

Ifconfig merupkan salah satu perintah dasar untuk memeriksa dan berinterkasi dengan interface jaringan yang ada pada perangkat komputer kita. Kita bisa menggunakanya secara sederhana dengan langsung mengetik ifconfig pada terminal.

```bash
ifconfig
```

Perintah tersebut akan emanmpilkan semua informasi interface jaringan yang ada pada komputer kita.&#x20;

### Mengecek perangkat wireless dengan perintah iwconfig

Jika kita memiliki perangkat wireless yang tersambung pada komputer kita, kita bisa menggunakan perintah iwconfig untuk menampilkan informasi semua perangkat wireless yang sudah terpasang pada komputer kita.

```bash
iwconfig
```

### Mengubah Informasi jaringan

Mampu mengganti IP address dan berbagai informasi jaringan merupakan skill yang sangat berguna karena akan membantu kita untuk mengakses jaringan yang berbeda dan hair sebagai perangkat yang dipercaya pada jaringan tersebut. Sebagai contoh, dalam serangan DoS (Denial of Service) kita bisa mengakali/menyembunyikan IP kita sebagai penyerang dan akan menghindari ketika ada investigasi jika terdapat analisis forensic dari sisi korban. Ini merupakan konsep dasar yang bisa diatasi dengan iinfconfig.

### Mengganti IP Address

Untuk mengganti ip address kita bbisa menggunakan perintah ifconfig lalu diikuti ip address  pada interface yang ingin kita set.

Kita bisa menggunakan perintah berikut :

```bash
ifconfig [interface] [ipaddress]

#Example
ifconfig eth0 192.168.1.1
```

Jika setelah mengeksekusi perintah tersebut tidak ada pesan error, artinya kita sudah berhasil mengganti ip pada interface tersebut. Dan kita bisa melihat ip address sudah berubah.

### Mengubah Network Mask dan IP Broadcast

Kita juga bisa mengubah network mask (netmask) dan broadcast address dengan menggunakan perintah ifconfig.

Berikut adalah perintah yang bisa kita gunakan :&#x20;

```bash
ifconfig [interface] [ipaddress] netmask [netmask] broadcast [broadcast]

#Example
ifconfig eth0 192.168.181.115 netmask 255.255.255.0 broadcast 192.168.1.255
```

Jika tidak terdapat error, maka kita sukses mengganti ip, netmask dan broadcast address pada interface tersebut.&#x20;

Dan kita bisa memastikanya kembali dengan perintah ifconfig.

### Memalsukan Mac Address

Kita juga bisa menggunakan ifconfig untuk mengganti Mac Address tau hardware address. Mac Address biasnaya unik dan tidak akan pernah sama dengan dengan mac address lainya. Mengganti mac address dapat digunakan untuk menyumbunyikan identitas device yang kita gunakan untuk agar lebih aman dan lebih sulit untuk terdeteksi.&#x20;

Untuk mengganti mac address terlebih dahulu kita disable atau takedown interface yang ingin kita ganti mac addressnya.

Berikut adalah contoh mengganti mac address :&#x20;

```bash
ifconfig eth0 down
ifconfig eth0 hw enther 00:11:22:33:44:55
ifconfig eth0 up
```

Jika sudah dieksekusi kita bisa mengeksekusi perintah ifconfig untuk memastikan apakah sudah berhasil terganti.

### Mendapatkan IP Address dari DHCP Server

Linux memiliki DHCP Server dan berjalan sebagai service, service tersebut berjalan di backgroud dengan nama dhcpd atau dhcp daemon.&#x20;

Biasanya, untuk menghubungkan ke jaringan LAN, kita perlu mendapatkan IP Address dengan menggunakan ip dhcp client. Jika sebelumnya kita setting mengguanakn IP static dan ingin mendapatkan ip dhcp biasnya kita melakukan restart terlebih dahulu.

Namun kita bisa mendapatkan ip dhcp tanpa harus merestart komputer kita. Kita bisa menggunakan perintah dhclient.&#x20;

Berikut cara menggunakanya :&#x20;

```bash
dhclient [interface]

#Example
dhclient eth0
```

Perintah tersebut akan mengitik sebuah request dhcpdiscover dari interface tersebut. Lalu akan diterima oleh DHCP server sebagai DHCPOFFER dan akan dikonfirmasi atau dberikan kuota ip oleh DHCP Server yang terdapat dalam jaringan.

### Memanipulasi DNS

Hacker bisa menemukan sebuah informasi harta karun pada target melalu DNS. DNS merupakan komponen yang kritikal dari internet, digunakan untuk menerjemahkan nama domain menjadi IP Address, hacker bisa menggunakanya untuk mendapatkan banyak informasi.

### Mendapatkan informasi DNS menggunakan dig

DNS merupakan service yang menerjemahkan nama domain seperti google.com menjadi IP Address. Itulah mengapa perangkat kita bisa mengaksesnya.&#x20;

Tanpa DNS kita perlu menghafalkan ribuan ip address dari wesbite faforite kita.&#x20;

Salah satu perntah yang paling banyak digunakan oleh hacker aadalah dig, dimana kita bisa menggali informasi terkait DNS.&#x20;

Berikut cara menggunakan dig :&#x20;

```bash
dig [namadomain]

#Example
dig kompas.com
```

Berikut adalah hasil eksekusi perintah tersebut :&#x20;

```bash
; <<>> DiG 9.18.18-0ubuntu2.1-Ubuntu <<>> kompas.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 8009
;; flags: qr rd ra; QUERY: 1, ANSWER: 4, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 65494
;; QUESTION SECTION:
;kompas.com.			IN	A

;; ANSWER SECTION:
kompas.com.		60	IN	A	52.84.229.92
kompas.com.		60	IN	A	52.84.229.57
kompas.com.		60	IN	A	52.84.229.37
kompas.com.		60	IN	A	52.84.229.119

;; Query time: 44 msec
;; SERVER: 127.0.0.53#53(127.0.0.53) (UDP)
;; WHEN: Sun Mar 03 21:39:09 WITA 2024
;; MSG SIZE  rcvd: 103

```

Dari hasil eksekusi perintah `dig` untuk domain `kompas.com`, kita dapat memahami beberapa detail penting mengenai DNS (Domain Name System) dari domain tersebut:

* **QUESTION SECTION**: Bagian ini menunjukkan permintaan yang diajukan, yakni mencari record `A` (alamat IP) untuk domain `kompas.com`.
* **ANSWER SECTION**: Bagian ini menampilkan jawaban dari permintaan tersebut. Ada 4 alamat IP yang dikembalikan, yaitu `52.84.229.92`, `52.84.229.57`, `52.84.229.37`, dan `52.84.229.119`. Setiap record memiliki TTL (Time To Live) 60 detik, menunjukkan waktu setelahnya DNS cache harus diperbarui.
* **SERVER**: Menunjukkan server DNS lokal yang digunakan untuk query, yaitu `127.0.0.53`.
* **Query time**: Waktu yang diperlukan untuk mendapatkan jawaban, dalam kasus ini adalah 44 milidetik.
* **DATE**: Menampilkan waktu ketika query dijalankan, yaitu `Sun Mar 03 21:39:09 WITA 2024`.

Informasi tersebut berguna, terutama bagi administrator jaringan atau pengembang web, untuk memahami bagaimana nama domain mereka diselesaikan dan mengidentifikasi potensi masalah dalam konfigurasi DNS.

### Mengganti IP DNS Server

Dalam kasus tertentu, kita mungkin ingin menggunakan DNS Server tertentu. Untuk melakukanya kita perlu mengedit file berikut  /etc/resolv.conf pada system kita.

Kita bisa mengeditnya menggunakan text editor seperti nano,vim  atau gedit.

```bash
nano /etc/resolv.conf
```

Kita bisa mencari baris nameserver dan mengisi ip dns server di belakangnya.

Contoh :&#x20;

```bash
nameserver 8.8.8.8
```

Pada contoh diatas saya menggunakan DNS dari google.

Jiika ingin lebih mudah kita bisa menggunakan perintah echo. Kita hanya perlu mengeksekusi satu baris perintah untuk mengganti DNS server pada file tersebut.

Berikut adalah contoh perintah yang bisa digunakan :&#x20;

```bash
echo "nameserver 8.8.8.8" > /etc/resolv.conf
```

Sebagai catatan, jika kita menggunakan DHCP client, ketika kita assign ip address baru dari DHCP Server makan maka IP yang kita settign akan ditimpa oleh IP DNS Server dari DHCP Server.
