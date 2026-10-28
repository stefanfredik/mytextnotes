# Script Docker

Berikut daftar lengkap perintah-perintah Docker beserta parameter, fungsi, dan contoh penggunaannya:

***

### **1. Perintah Dasar Docker**

#### **1.1 `docker --version`**

* **Fungsi**: Menampilkan versi Docker yang terinstal.
*   **Contoh**:

    ```sh
    docker --version
    ```

#### **1.2 `docker info`**

* **Fungsi**: Menampilkan informasi tentang sistem Docker yang sedang berjalan.
*   **Contoh**:

    ```sh
    docker info
    ```

#### **1.3 `docker help`**

* **Fungsi**: Menampilkan daftar perintah Docker atau bantuan terkait perintah tertentu.
*   **Contoh**:

    ```sh
    docker help
    docker help run
    ```

***

### **2. Perintah Manajemen Container**

#### **2.1 `docker run`**

* **Fungsi**: Menjalankan container baru berdasarkan image.
* **Parameter**:
  * `-d` → Jalankan container di latar belakang.
  * `-it` → Menjalankan container dalam mode interaktif dengan terminal.
  * `--name <nama_container>` → Memberi nama pada container.
  * `-p <host_port>:<container_port>` → Melakukan port forwarding.
  * `-v <host_dir>:<container_dir>` → Mount volume dari host ke container.
*   **Contoh**:

    ```sh
    docker run -d -p 8080:80 --name webserver nginx
    docker run -it --rm ubuntu bash
    ```

#### **2.2 `docker ps`**

* **Fungsi**: Menampilkan daftar container yang sedang berjalan.
* **Parameter**:
  * `-a` → Menampilkan semua container (baik yang berjalan maupun yang berhenti).
*   **Contoh**:

    ```sh
    docker ps
    docker ps -a
    ```

#### **2.3 `docker stop`**

* **Fungsi**: Menghentikan container yang sedang berjalan.
*   **Contoh**:

    ```sh
    docker stop webserver
    ```

#### **2.4 `docker start`**

* **Fungsi**: Menjalankan kembali container yang telah dihentikan.
*   **Contoh**:

    ```sh
    docker start webserver
    ```

#### **2.5 `docker restart`**

* **Fungsi**: Merestart container.
*   **Contoh**:

    ```sh
    docker restart webserver
    ```

#### **2.6 `docker rm`**

* **Fungsi**: Menghapus container.
* **Parameter**:
  * `-f` → Memaksa penghapusan container yang sedang berjalan.
*   **Contoh**:

    ```sh
    docker rm webserver
    docker rm -f webserver
    ```

***

### **3. Perintah Manajemen Image**

#### **3.1 `docker images`**

* **Fungsi**: Menampilkan daftar image yang tersedia di sistem lokal.
*   **Contoh**:

    ```sh
    docker images
    ```

#### **3.2 `docker pull`**

* **Fungsi**: Mengunduh image dari Docker Hub.
*   **Contoh**:

    ```sh
    docker pull nginx
    docker pull ubuntu:latest
    ```

#### **3.3 `docker rmi`**

* **Fungsi**: Menghapus image.
* **Parameter**:
  * `-f` → Memaksa penghapusan image yang sedang digunakan oleh container.
*   **Contoh**:

    ```sh
    docker rmi nginx
    docker rmi -f nginx
    ```

#### **3.4 `docker build`**

* **Fungsi**: Membangun image baru dari Dockerfile.
* **Parameter**:
  * `-t` → Memberi nama dan tag pada image.
*   **Contoh**:

    ```sh
    docker build -t myapp:v1 .
    ```

***

### **4. Perintah Manajemen Volume**

#### **4.1 `docker volume create`**

* **Fungsi**: Membuat volume baru.
*   **Contoh**:

    ```sh
    docker volume create myvolume
    ```

#### **4.2 `docker volume ls`**

* **Fungsi**: Menampilkan daftar volume yang ada.
*   **Contoh**:

    ```sh
    docker volume ls
    ```

#### **4.3 `docker volume inspect`**

* **Fungsi**: Melihat detail informasi volume.
*   **Contoh**:

    ```sh
    docker volume inspect myvolume
    ```

#### **4.4 `docker volume rm`**

* **Fungsi**: Menghapus volume.
*   **Contoh**:

    ```sh
    docker volume rm myvolume
    ```

***

### **5. Perintah Manajemen Jaringan**

#### **5.1 `docker network create`**

* **Fungsi**: Membuat jaringan baru di Docker.
*   **Contoh**:

    ```sh
    docker network create mynetwork
    ```

#### **5.2 `docker network ls`**

* **Fungsi**: Menampilkan daftar jaringan Docker.
*   **Contoh**:

    ```sh
    docker network ls
    ```

#### **5.3 `docker network inspect`**

* **Fungsi**: Melihat detail informasi jaringan tertentu.
*   **Contoh**:

    ```sh
    docker network inspect mynetwork
    ```

#### **5.4 `docker network rm`**

* **Fungsi**: Menghapus jaringan.
*   **Contoh**:

    ```sh
    docker network rm mynetwork
    ```

***

### **6. Perintah Logging dan Monitoring**

#### **6.1 `docker logs`**

* **Fungsi**: Melihat log dari container.
* **Parameter**:
  * `-f` → Mengikuti log secara real-time.
*   **Contoh**:

    ```sh
    docker logs webserver
    docker logs -f webserver
    ```

#### **6.2 `docker stats`**

* **Fungsi**: Menampilkan penggunaan sumber daya container secara real-time.
*   **Contoh**:

    ```sh
    docker stats
    ```

#### **6.3 `docker top`**

* **Fungsi**: Melihat proses yang sedang berjalan dalam container.
*   **Contoh**:

    ```sh
    docker top webserver
    ```

***

### **7. Perintah Eksekusi dalam Container**

#### **7.1 `docker exec`**

* **Fungsi**: Menjalankan perintah dalam container yang sedang berjalan.
* **Parameter**:
  * `-it` → Menjalankan dalam mode interaktif.
*   **Contoh**:

    ```sh
    docker exec -it webserver bash
    docker exec webserver ls /var/www/html
    ```

#### **7.2 `docker attach`**

* **Fungsi**: Menghubungkan terminal ke container yang sedang berjalan.
*   **Contoh**:

    ```sh
    docker attach webserver
    ```

***

### **8. Perintah Save dan Load Image**

#### **8.1 `docker save`**

* **Fungsi**: Menyimpan image dalam bentuk file tar.
*   **Contoh**:

    ```sh
    docker save -o nginx.tar nginx
    ```

#### **8.2 `docker load`**

* **Fungsi**: Memuat image dari file tar.
*   **Contoh**:

    ```sh
    docker load -i nginx.tar
    ```

***

### **9. Perintah Export dan Import Container**

#### **9.1 `docker export`**

* **Fungsi**: Mengekspor container dalam bentuk file tar.
*   **Contoh**:

    ```sh
    docker export webserver -o webserver.tar
    ```

#### **9.2 `docker import`**

* **Fungsi**: Mengimpor file tar menjadi image Docker.
*   **Contoh**:

    ```sh
    cat webserver.tar | docker import - mywebserver:v1
    ```

***

Itulah daftar lengkap perintah Docker beserta fungsi, parameter, dan contoh penggunaannya. 🚀
