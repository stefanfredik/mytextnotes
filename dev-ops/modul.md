# Modul

## Modul Belajar Docker: Panduan Komprehensif

### **1. Pendahuluan**

#### **1.1 Apa itu Docker?**

Docker adalah platform open-source yang memungkinkan pengembang untuk mengotomatisasi pengembangan, pengiriman, dan penerapan aplikasi dalam lingkungan yang terisolasi yang disebut **container**. Container memungkinkan aplikasi untuk berjalan dengan konsisten di berbagai lingkungan, mulai dari development, testing, hingga production.

#### **1.2 Keuntungan Menggunakan Docker**

* **Konsistensi**: Aplikasi berjalan dengan cara yang sama di semua lingkungan.
* **Isolasi**: Setiap aplikasi berjalan dalam container terpisah, mengurangi konflik dependensi.
* **Portabilitas**: Container dapat dijalankan di mana saja, baik di lokal, cloud, atau server fisik.
* **Efisiensi**: Container lebih ringan daripada virtual machine (VM) karena berbagi kernel host.

#### **1.3 Konsep Dasar Docker**

* **Image**: Template read-only yang berisi aplikasi dan semua dependensinya.
* **Container**: Instance yang dapat dijalankan dari sebuah image.
* **Dockerfile**: File teks yang berisi instruksi untuk membuat image.
* **Docker Hub**: Repositori publik untuk berbagi dan menemukan Docker image.

***

### **2. Instalasi Docker**

#### **2.1 Instalasi di Linux**

1.  Update package manager:

    ```bash
    sudo apt-get update
    ```
2.  Install paket yang diperlukan:

    ```bash
    sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
    ```
3.  Tambahkan Docker GPG key:

    ```bash
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    ```
4.  Tambahkan Docker repository:

    ```bash
    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
    ```
5.  Install Docker:

    ```bash
    sudo apt-get update
    sudo apt-get install docker-ce
    ```
6.  Verifikasi instalasi:

    ```bash
    sudo docker --version
    ```

#### **2.2 Instalasi di Windows dan macOS**

* Unduh Docker Desktop dari [situs resmi Docker](https://www.docker.com/products/docker-desktop).
* Ikuti petunjuk instalasi.
*   Setelah instalasi selesai, buka terminal dan verifikasi:

    ```bash
    docker --version
    ```

***

### **3. Dasar-Dasar Docker**

#### **3.1 Menjalankan Container**

*   Jalankan container dari image:

    ```bash
    docker run hello-world
    ```
*   Jalankan container dengan interaktif shell:

    ```bash
    docker run -it ubuntu /bin/bash
    ```

#### **3.2 Melihat Container yang Sedang Berjalan**

*   List semua container yang sedang berjalan:

    ```bash
    docker ps
    ```
*   List semua container (termasuk yang tidak berjalan):

    ```bash
    docker ps -a
    ```

#### **3.3 Menghentikan dan Menghapus Container**

*   Hentikan container:

    ```bash
    docker stop <container_id>
    ```
*   Hapus container:

    ```bash
    docker rm <container_id>
    ```

#### **3.4 Mengelola Docker Images**

*   List semua images:

    ```bash
    docker images
    ```
*   Hapus image:

    ```bash
    docker rmi <image_id>
    ```

***

### **4. Membuat Dockerfile**

#### **4.1 Struktur Dockerfile**

Dockerfile adalah file teks yang berisi serangkaian instruksi untuk membuat Docker image. Contoh sederhana:

```dockerfile
# Gunakan image base
FROM ubuntu:20.04

# Update package manager dan install aplikasi
RUN apt-get update && apt-get install -y python3

# Set working directory
WORKDIR /app

# Copy file aplikasi ke container
COPY . /app

# Expose port
EXPOSE 80

# Command yang dijalankan saat container start
CMD ["python3", "app.py"]
```

#### **4.2 Membangun Image dari Dockerfile**

*   Build image:

    ```bash
    docker build -t nama-image:tag .
    ```
*   Jalankan container dari image yang baru dibuat:

    ```bash
    docker run -d -p 80:80 nama-image:tag
    ```

***

### **5. Docker Compose**

#### **5.1 Apa itu Docker Compose?**

Docker Compose adalah alat untuk mendefinisikan dan menjalankan multi-container Docker aplikasi. Dengan menggunakan file `docker-compose.yml`, Anda dapat mengonfigurasi dan menjalankan beberapa container sekaligus.

#### **5.2 Contoh `docker-compose.yml`**

```yaml
version: '3'
services:
  web:
    image: nginx
    ports:
      - "80:80"
  db:
    image: mysql
    environment:
      MYSQL_ROOT_PASSWORD: example
```

#### **5.3 Menjalankan Docker Compose**

*   Jalankan semua service:

    ```bash
    docker-compose up
    ```
*   Hentikan dan hapus container:

    ```bash
    docker-compose down
    ```

***

### **6. Networking dan Volumes**

#### **6.1 Docker Networking**

*   Buat network:

    ```bash
    docker network create nama-network
    ```
*   Jalankan container dengan network tertentu:

    ```bash
    docker run --network nama-network nama-image
    ```

#### **6.2 Docker Volumes**

*   Buat volume:

    ```bash
    docker volume create nama-volume
    ```
*   Gunakan volume di container:

    ```bash
    docker run -v nama-volume:/path/di/container nama-image
    ```

***

### **7. Use Case: Deploy Aplikasi Web Sederhana**

#### **7.1 Deskripsi Use Case**

Kita akan membuat aplikasi web sederhana menggunakan Python Flask dan MySQL. Aplikasi ini akan di-deploy menggunakan Docker dan Docker Compose.

#### **7.2 Langkah-Langkah**

1. **Buat File Aplikasi Flask**:
   *   `app.py`:

       ```python
       from flask import Flask
       import mysql.connector

       app = Flask(__name__)

       @app.route('/')
       def hello():
           return "Hello, Docker!"

       if __name__ == '__main__':
           app.run(host='0.0.0.0')
       ```
2.  **Buat Dockerfile**:

    ```dockerfile
    FROM python:3.8-slim
    WORKDIR /app
    COPY . /app
    RUN pip install flask mysql-connector-python
    EXPOSE 5000
    CMD ["python", "app.py"]
    ```
3.  **Buat `docker-compose.yml`**:

    ```yaml
    version: '3'
    services:
      web:
        build: .
        ports:
          - "5000:5000"
        depends_on:
          - db
      db:
        image: mysql:5.7
        environment:
          MYSQL_ROOT_PASSWORD: root
          MYSQL_DATABASE: mydb
    ```
4.  **Jalankan Aplikasi**:

    ```bash
    docker-compose up
    ```
5. **Akses Aplikasi**: Buka browser dan akses `http://localhost:5000`.

***

### **8. Kesimpulan**

Docker adalah alat yang sangat powerful untuk mengelola dan mengotomatisasi pengembangan dan deployment aplikasi. Dengan memahami konsep dasar Docker, membuat Dockerfile, dan menggunakan Docker Compose, Anda dapat membangun dan menjalankan aplikasi dengan lebih efisien dan konsisten.

***

### **9. Contoh Use Case Lanjutan**

#### **9.1 Use Case: Deploy Aplikasi Microservices**

Anda dapat mencoba membuat aplikasi microservices dengan beberapa service seperti:

* **Service 1**: Aplikasi web (Flask/Node.js)
* **Service 2**: Database (MySQL/PostgreSQL)
* **Service 3**: Cache (Redis)
* **Service 4**: Load Balancer (Nginx)

Gunakan Docker Compose untuk mengatur dan menjalankan semua service ini secara bersamaan.

#### **9.2 Use Case: CI/CD dengan Docker**

Integrasikan Docker dengan pipeline CI/CD seperti Jenkins, GitLab CI, atau GitHub Actions untuk mengotomatisasi proses build, test, dan deployment aplikasi.

***

Dengan mempelajari modul ini, Anda telah memiliki dasar yang kuat untuk mulai menggunakan Docker dalam proyek-proyek Anda. Selamat mencoba!
