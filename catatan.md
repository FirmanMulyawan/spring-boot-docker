Siap 👍 Kita mulai dari **LEVEL 1 — Konsep Dasar Docker**. Saya akan jelaskan **pelan-pelan dengan bahasa sederhana**, tidak langsung dijejali banyak command.

Anggap saja kita sedang belajar Docker dari **nol banget**.

# 🐳 LEVEL 1 — Konsep Dasar Docker

Sebelum belajar command seperti `docker run`, `docker ps`, dan `docker compose`, kita harus memahami **5 benda utama**:

```text
Docker
Image
Container
Dockerfile
Registry
```

Kita bahas satu-satu.

---

## 1. Apa itu Docker?

Bayangkan kamu punya aplikasi.

Misalnya aplikasi Spring Boot kamu membutuhkan:

```text
Java 21
Spring Boot
PostgreSQL
Redis
```

Kalau kamu menjalankannya langsung di komputer, kamu harus memastikan semuanya sudah terinstall dan versinya cocok.

Masalahnya:

```text
Laptop saya      → berhasil
Laptop teman     → error
Server           → error
```

😵

Docker membantu membuat lingkungan aplikasi menjadi lebih **konsisten**.

Sederhananya:

> **Docker adalah alat untuk menjalankan aplikasi di dalam lingkungan yang terisolasi yang disebut container.**

---

# 2. Apa itu Container?

Ini bagian paling penting.

Bayangkan kamu punya **kotak** 📦.

Di dalam kotak itu ada aplikasi yang ingin kamu jalankan.

```text
┌──────────────────────┐
│      CONTAINER       │
│                      │
│   Aplikasi           │
│   + kebutuhan        │
│                      │
└──────────────────────┘
```

Kotak tersebut adalah **container**.

Misalnya:

```text
┌──────────────────────┐
│ PostgreSQL Container │
│                      │
│ PostgreSQL           │
└──────────────────────┘
```

Atau:

```text
┌──────────────────────┐
│ Redis Container      │
│                      │
│ Redis                │
└──────────────────────┘
```

Jadi:

> **Container = tempat aplikasi berjalan.**

---

# 3. Apa itu Image?

Nah, sekarang kita punya container.

Pertanyaannya:

> "Container itu dibuat dari mana?"

Jawabannya:

**Image.**

Bayangkan:

```text
IMAGE = cetakan
CONTAINER = hasil cetakan
```

Misalnya kamu punya cetakan kue 🍪.

```text
      CETAKAN
        ↓
      🍪
      🍪
      🍪
```

Sama dengan Docker:

```text
      IMAGE
        ↓
   ┌────┴────┐
   ↓         ↓
Container  Container
```

Contoh:

```text
Redis Image
     ↓
     ├── Redis Container 1
     ├── Redis Container 2
     └── Redis Container 3
```

Jadi:

> **Image adalah template/cetakan untuk membuat container.**

---

# 4. Image dan Container jangan tertukar

Ini sangat penting.

### Image

```text
IMAGE
↓
Template
↓
Cetakan
↓
Belum tentu sedang berjalan
```

### Container

```text
CONTAINER
↓
Dibuat dari Image
↓
Bisa dijalankan
↓
Bisa dihentikan
```

Analogi rumah 🏠:

```text
Denah rumah
     ↓
   IMAGE

Rumah yang dibangun
     ↓
 CONTAINER
```

---

# 5. Contoh nyata

Misalnya kita punya:

```text
nginx
```

Nginx bisa kita ambil dalam bentuk **Docker Image**.

Kemudian image tersebut digunakan untuk membuat container.

```text
nginx IMAGE
     ↓
nginx CONTAINER
```

Container tersebut kemudian menjalankan Nginx.

Jadi alurnya:

```text
Docker Image
     ↓
buat container
     ↓
Container berjalan
     ↓
Nginx berjalan
```

---

# 6. Apa itu Dockerfile?

Sekarang muncul pertanyaan baru:

> "Kalau saya ingin membuat Image sendiri bagaimana?"

Nah, kita menggunakan:

**Dockerfile.**

Dockerfile adalah file yang berisi **instruksi untuk membuat Docker Image**.

Misalnya:

```text
Dockerfile
    ↓
"Gunakan Java"
"Copy aplikasi"
"Jalankan aplikasi"
    ↓
Docker Image
```

Jadi:

```text
Dockerfile
     ↓
 docker build
     ↓
Docker Image
     ↓
 docker run
     ↓
Container
```

Ini adalah salah satu alur terpenting dalam Docker.

---

# 7. Apa itu Registry?

Sekarang kita punya Image.

Pertanyaannya:

> "Kalau saya punya Image di laptop, bagaimana saya menyimpannya atau mengambil Image milik orang lain?"

Gunakan **Registry**.

Registry adalah tempat menyimpan Docker Image.

Contoh paling terkenal:

**Docker Hub**

Bayangkan seperti:

```text
GitHub
↓
menyimpan source code

Docker Hub
↓
menyimpan Docker Image
```

Misalnya:

```text
Docker Hub
│
├── nginx
├── redis
├── postgres
├── mysql
└── dll
```

Kita bisa mengambil image dari registry.

```text
Docker Hub
     ↓
  nginx image
     ↓
    Laptop
```

---

# 8. Jadi hubungan semuanya bagaimana?

Sekarang gabungkan semuanya.

```text
                 DOCKER HUB
                     │
                     │ download
                     ↓
                  IMAGE
                     │
                     │ create
                     ↓
                CONTAINER
                     │
                     ↓
                 APLIKASI
```

Kalau kita membuat image sendiri:

```text
              Dockerfile
                  │
                  │ build
                  ↓
                IMAGE
                  │
                  │ run
                  ↓
              CONTAINER
                  │
                  ↓
               APLIKASI
```

---

# 9. Contoh dengan Spring Boot

Misalnya kamu punya:

```text
DigiAsk Backend
```

yang dibuat menggunakan:

```text
Java
Spring Boot
```

Kita bisa membuat:

```text
Dockerfile
    ↓
Spring Boot Image
    ↓
Spring Boot Container
    ↓
Spring Boot berjalan
```

Kemudian database:

```text
PostgreSQL Image
       ↓
PostgreSQL Container
       ↓
Database berjalan
```

Redis:

```text
Redis Image
       ↓
Redis Container
       ↓
Redis berjalan
```

Akhirnya:

```text
Docker
│
├── Spring Boot Container
│
├── PostgreSQL Container
│
└── Redis Container
```

Ini nanti akan sangat dekat dengan project backend yang kamu kerjakan.

---

# 10. Kenapa tidak satu container saja?

Misalnya:

```text
❌ 1 Container
   ├── Spring Boot
   ├── PostgreSQL
   └── Redis
```

Biasanya Docker digunakan dengan pendekatan:

```text
✅ Spring Boot Container
        +
   PostgreSQL Container
        +
      Redis Container
```

Masing-masing punya tugas sendiri.

```text
Spring Boot
    ↓
mengurus aplikasi/API

PostgreSQL
    ↓
mengurus database

Redis
    ↓
mengurus cache
```

Nanti container-container tersebut bisa saling berkomunikasi melalui **Docker Network**.

---

# 🧠 11. Lima istilah yang harus kamu ingat

Untuk hari ini, cukup hafalkan ini:

### Docker

> Alat untuk menjalankan aplikasi menggunakan container.

### Container

> Tempat aplikasi berjalan.

### Image

> Cetakan/template untuk membuat container.

### Dockerfile

> Instruksi untuk membuat Image.

### Registry

> Tempat menyimpan Docker Image.

---

# ⭐ Rumus paling gampang

Kalau lupa semuanya, ingat ini:

```text
Dockerfile
    ↓
  IMAGE
    ↓
CONTAINER
    ↓
 APLIKASI
```

Sedangkan Image bisa berasal dari registry:

```text
Docker Hub
    ↓
  IMAGE
    ↓
CONTAINER
```

---

# 🎯 Tes kecil sebelum LEVEL 2

Coba jawab **tanpa melihat penjelasan di atas**:

**1.** Kalau `Image` adalah cetakan, maka `Container` adalah apa?

**2.** Kalau ingin membuat Image sendiri, kita menggunakan file apa?

**3.** Docker Hub digunakan untuk apa?

**4.** Mana yang benar?

```text
A. Container → Image → Dockerfile

B. Dockerfile → Image → Container
```

**5.** Kalau kita punya:

```text
Redis Image
```

apakah kita bisa membuat **2 Redis Container** dari image tersebut?

Jawab 1–5 saja. Setelah itu saya akan cek jawabanmu, lalu kita lanjut **LEVEL 2 — Docker CLI** dengan praktik pertama `docker run` secara pelan-pelan.

---

Betul semua! 🎉 **5/5 benar.** Berarti konsep dasar Docker sudah mulai masuk.

Kita lanjut ke **LEVEL 2 — Docker CLI**, tapi tetap pelan-pelan.

# 🐳 LEVEL 2 — Docker CLI

**CLI** adalah singkatan dari:

> **Command Line Interface**

Bahasa sederhananya:

> **CLI = cara kita menyuruh Docker menggunakan terminal.**

Misalnya kita mengetik:

```powershell
docker ps
```

Kita sedang **memberi perintah kepada Docker**.

---

## 1. Sebelum praktik: pastikan Docker hidup

Karena sebelumnya kamu mendapatkan error:

```text
failed to connect to the docker API
dockerDesktopLinuxEngine
```

pastikan **Docker Desktop sudah dibuka dan Docker Engine sudah running**.

Kemudian di PowerShell:

```powershell
docker --version
```

Kalau berhasil, akan muncul kira-kira:

```text
Docker version 28.x.x, build xxxxx
```

Artinya:

> Docker CLI sudah dikenali oleh Windows.

---

# 2. Command pertama: `docker ps`

Sekarang jalankan:

```powershell
docker ps
```

Apa artinya?

`ps` bisa kita anggap sebagai:

> **lihat container yang sedang berjalan**

Kalau belum ada container, kemungkinan hasilnya seperti:

```text
CONTAINER ID   IMAGE   COMMAND   CREATED   STATUS   PORTS   NAMES
```

Kosong tidak masalah.

Artinya:

> Docker hidup, tetapi saat ini belum ada container yang berjalan.

---

# 3. `docker ps -a`

Sekarang:

```powershell
docker ps -a
```

Bedanya:

```text
docker ps
    ↓
container yang sedang berjalan
```

sedangkan:

```text
docker ps -a
    ↓
semua container
```

Termasuk container yang sudah berhenti.

---

# 4. Analogi gampang

Bayangkan kamu punya garasi 🚗.

```text
docker ps
```

seperti:

> "Tampilkan mobil yang **sedang ada di jalan**."

Sedangkan:

```text
docker ps -a
```

seperti:

> "Tampilkan **semua mobil**, termasuk yang sedang parkir."

---

# 5. Command berikutnya: `docker images`

Sekarang:

```powershell
docker images
```

Ini digunakan untuk melihat:

> **Image apa saja yang sudah ada di komputer kita.**

Misalnya:

```text
REPOSITORY   TAG       IMAGE ID
nginx        latest    xxxxx
redis        7         xxxxx
postgres     16        xxxxx
```

Jadi:

```text
docker ps
```

→ melihat **container**

```text
docker images
```

→ melihat **image**

Ini harus kamu bedakan.

---

# 🧠 Ingat 3 command ini dulu

```text
docker ps
     ↓
lihat container yang berjalan

docker ps -a
     ↓
lihat semua container

docker images
     ↓
lihat image
```

Jangan belajar 20 command sekaligus.

Kita kuasai **3 ini dulu**.

---

# 🧪 Praktik pertama

Sekarang buka **PowerShell** dan jalankan satu per satu:

```powershell
docker --version
```

kemudian:

```powershell
docker ps
```

kemudian:

```powershell
docker ps -a
```

kemudian:

```powershell
docker images
```

**Kirim hasilnya ke saya.**

Setelah itu kita akan melakukan hal yang lebih menarik:

```text
Docker Hub
    ↓
download Nginx Image
    ↓
buat Container
    ↓
jalankan Nginx
    ↓
buka http://localhost:8080
```

Itu akan menjadi **praktik Docker pertama kamu**. 🚀

---

Mantap 👍 **Docker kamu sudah berjalan dengan normal.** Bahkan environment Docker kamu sudah lumayan lengkap. 😄

Mari kita **jangan buru-buru masuk ke command berikutnya**. Kita gunakan hasil terminal kamu untuk memahami konsep LEVEL 2.

## 1. `docker --version` ✅

Kamu mendapatkan:

```text
Docker version 29.7.2
```

Artinya:

> Docker CLI sudah terinstall dan bisa digunakan.

---

## 2. `docker ps` ✅

Kamu punya **2 container yang sedang berjalan**:

```text
postgres
redis
```

Perhatikan bagian:

```text
IMAGE
postgres:16-alpine
redis:6.0.8
```

Ini menghubungkan dengan pelajaran LEVEL 1 kita.

```text
postgres:16-alpine
        ↓
    PostgreSQL
        ↓
   postgres container
```

dan:

```text
redis:6.0.8
        ↓
      Redis
        ↓
   redis container
```

Jadi sekarang kamu sudah melihat sendiri:

> **Image → Container**

---

# 3. Mari baca `docker ps`

Contoh milikmu:

```text
CONTAINER ID   IMAGE                STATUS       PORTS       NAMES
976323b33abd   postgres:16-alpine   Up 2 hours   ...         postgres
b454c3c2085b   redis:6.0.8          Up 2 hours   ...         redis
```

Ada beberapa kolom penting.

### `CONTAINER ID`

Contohnya:

```text
976323b33abd
```

Ini adalah **ID unik container**.

Anggap seperti nomor identitas container.

---

### `IMAGE`

Contohnya:

```text
postgres:16-alpine
```

Ini adalah **image yang digunakan untuk membuat container tersebut**.

Jadi:

```text
postgres:16-alpine
       ↓
   container
       ↓
    postgres
```

---

### `STATUS`

```text
Up 2 hours
```

Artinya container tersebut **sedang berjalan**.

Kalau:

```text
Exited
```

artinya container tersebut **sudah berhenti**.

---

### `PORTS`

PostgreSQL kamu:

```text
0.0.0.0:5432->5432/tcp
```

Jangan khawatir dulu kalau ini terlihat rumit.

Sederhananya:

```text
Windows : 5432
    ↓
Docker PostgreSQL : 5432
```

Artinya PostgreSQL di dalam container bisa diakses melalui port `5432` dari komputer kamu.

Redis:

```text
0.0.0.0:6379->6379/tcp
```

Artinya:

```text
Windows : 6379
    ↓
Docker Redis : 6379
```

Nanti kita pelajari **port mapping** lebih dalam.

---

### `NAMES`

Kamu punya:

```text
postgres
redis
```

Ini nama container.

Jadi kamu bisa menjalankan command menggunakan nama:

```powershell
docker logs postgres
```

atau:

```powershell
docker logs redis
```

Tidak perlu menghafalkan Container ID.

---

# 4. Sekarang lihat `docker ps -a`

Ini menarik.

Kamu punya:

```text
ubuntu
ubuntu
postgres
logstash
kafka
kibana
elasticsearch
zookeeper
stirling-pdf
redis
gotenberg
```

Tetapi hanya:

```text
postgres
redis
```

yang statusnya:

```text
Up
```

Yang lainnya:

```text
Exited
```

Jadi sekarang kamu sudah melihat perbedaan:

```text
docker ps
      ↓
yang sedang berjalan

docker ps -a
      ↓
SEMUA container
      ↓
yang berjalan + yang berhenti
```

---

# 5. `Exited (137)` itu apa?

Kamu punya Kafka:

```text
kafka
Exited (137)
```

Belum perlu kita bahas mendalam.

Untuk sementara cukup tahu:

> `Exited` = container berhenti.

Nanti ketika kita masuk ke **debugging Docker**, baru kita pelajari kenapa container bisa berhenti dan bagaimana melihat penyebabnya menggunakan:

```powershell
docker logs
```

---

# 6. Sekarang `docker images`

Ini bagian yang sangat bagus untuk belajar.

Kamu memiliki banyak image:

```text
postgres:16-alpine
redis:6.0.8
postgres:15
postgres:17
kafka
elasticsearch
kibana
...
```

Ingat konsep kita:

```text
IMAGE
  ↓
membuat
  ↓
CONTAINER
```

Misalnya:

```text
postgres:16-alpine
        ↓
     postgres
```

Dan:

```text
redis:6.0.8
        ↓
       redis
```

---

# 7. Ada hal menarik di sini

Kamu memiliki banyak versi PostgreSQL:

```text
postgres:12
postgres:12-alpine
postgres:12.12
postgres:15
postgres:15-alpine
postgres:16
postgres:16-alpine
postgres:17
```

Artinya Docker bisa menyimpan **banyak image/versi** di komputer yang sama.

Contohnya:

```text
Docker
│
├── PostgreSQL 12
├── PostgreSQL 15
├── PostgreSQL 16
└── PostgreSQL 17
```

Kemudian kamu bisa membuat container berdasarkan image tertentu.

Misalnya:

```text
postgres:16-alpine
        ↓
postgres container
```

---

# 🧠 Sekarang ada satu konsep penting

Kamu tadi menjawab:

> "1 container hanya untuk 1 image?"

Kita sudah tahu:

**1 Image → bisa banyak Container.**

Tapi sekarang dari komputer kamu kita bisa melihat contohnya.

Misalnya kamu punya:

```text
redis:6.0.8
```

Image tersebut bisa digunakan untuk:

```text
redis:6.0.8
      │
      ├── redis-dev
      ├── redis-test
      └── redis-staging
```

Jadi:

```text
IMAGE
  │
  ├── CONTAINER
  ├── CONTAINER
  └── CONTAINER
```

---

# 🎯 Jangan hapus apa pun dulu

Karena Docker kamu sedang digunakan untuk project, **jangan menjalankan**:

```powershell
docker system prune
```

atau:

```powershell
docker image prune
```

atau menghapus container sembarangan.

Kita sedang belajar menggunakan environment yang sudah kamu punya.

---

# 🧪 Praktik berikutnya

Sekarang kita akan melakukan **praktik pertama yang aman**.

Kita akan mengambil **Nginx Image** dan membuat container baru.

Jalankan:

```powershell
docker pull nginx
```

Perintah ini artinya:

> "Docker, ambil Image `nginx` dari Docker Registry."

Alurnya:

```text
Docker Hub
    ↓
nginx Image
    ↓
Laptop kamu
```

Setelah selesai, jalankan:

```powershell
docker images
```

Lalu kirim hasilnya.

**Belum perlu `docker run` dulu.** Kita pahami dulu apa yang terjadi setelah `docker pull`. Setelah itu baru kita buat container pertama kita. 🚀

---

---

Mantap 👍 Berarti **`docker pull nginx` berhasil**.

Sekarang kita manfaatkan hasil ini untuk memahami satu konsep penting.

## 🧠 Apa yang baru saja terjadi?

Sebelumnya kamu **belum punya**:

```text
nginx:latest
```

Setelah menjalankan:

```powershell
docker pull nginx
```

sekarang muncul:

```text
nginx:latest
05b8cb60c354
253MB
```

Artinya:

```text
Docker Registry
      │
      │ docker pull nginx
      ↓
nginx IMAGE
      │
      │
      ↓
Laptop kamu
```

**Belum ada container Nginx.**

Saat ini baru ada **IMAGE**.

---

## 🔥 Ini perbedaan penting

Sekarang kondisi Docker kamu kira-kira:

```text
IMAGES
│
├── nginx:latest       ← baru kita download
├── postgres:16-alpine
├── redis:6.0.8
├── kafka
├── elasticsearch
└── dll
```

Sedangkan container:

```text
CONTAINERS
│
├── postgres           ← berjalan
├── redis              ← berjalan
├── kafka              ← berhenti
├── elasticsearch      ← berhenti
└── dll
```

Perhatikan:

```text
nginx:latest
```

**ada di IMAGE**

tetapi belum ada:

```text
nginx container
```

---

# 🧪 Sekarang kita buat Container Nginx

Ini command pertama yang benar-benar akan membuat container:

```powershell
docker run -d --name belajar-nginx -p 8080:80 nginx
```

Jangan takut, kita bedah dulu.

### `docker run`

Artinya:

> **Buat container dari sebuah image dan jalankan.**

---

### `-d`

Artinya:

> Jalankan di background.

Jadi terminal kamu tetap bisa digunakan.

---

### `--name belajar-nginx`

Kita memberikan nama:

```text
belajar-nginx
```

Kalau tidak diberikan, Docker biasanya membuat nama random seperti:

```text
sweet_hodgkin
```

Seperti yang kamu punya tadi. 😄

---

### `-p 8080:80`

Ini **port mapping**.

Artinya:

```text
Komputer kamu        Container
    8080       →        80
```

Jadi:

```text
localhost:8080
       ↓
Nginx container :80
```

Kita akan pelajari port lebih dalam nanti.

---

### `nginx`

Ini adalah **image yang digunakan**.

Jadi keseluruhannya:

```text
nginx IMAGE
     ↓
docker run
     ↓
belajar-nginx CONTAINER
     ↓
Nginx berjalan
```

---

## 🚀 Sekarang jalankan

```powershell
docker run -d --name belajar-nginx -p 8080:80 nginx
```

Kemudian:

```powershell
docker ps
```

Kirim hasil `docker ps`-nya ke saya.

**Belum perlu menjalankan command lain.** Setelah itu kita akan lihat bersama bagaimana `nginx:latest` yang tadi hanya berupa **image** sekarang berubah menjadi **container yang berjalan**.

---

---

Mantap 👍 **berhasil.** Docker Nginx kamu sekarang sudah berjalan.

Dari output:

```text
8bd65efcc5cc   nginx   ...   Up 5 seconds   0.0.0.0:8080->80/tcp   belajar-nginx
```

Artinya:

- `nginx` → image yang digunakan
- `belajar-nginx` → nama container
- `Up 5 seconds` → container sedang hidup
- `8080->80` → port Windows `8080` diarahkan ke port Nginx `80`
- `0.0.0.0:8080` → bisa diakses melalui port 8080 di komputer kamu

### 🧠 Gambaran sederhananya

```text
Browser
   ↓
localhost:8080
   ↓
Docker
   ↓
belajar-nginx
   ↓
Nginx :80
   ↓
Halaman Nginx
```

Sekarang coba buka browser:

**http://localhost:8080**

Kalau muncul halaman:

> Welcome to nginx!

berarti kita sudah berhasil membuat **container pertama dari image Nginx** 🎉

### Setelah itu kita belajar satu hal penting

Jalankan:

```powershell
docker stop belajar-nginx
```

Lalu:

```powershell
docker ps
```

Kemungkinan `belajar-nginx` sudah tidak muncul karena `docker ps` hanya menampilkan container yang **sedang berjalan**.

Kemudian jalankan:

```powershell
docker ps -a
```

Nah, `belajar-nginx` akan muncul lagi dengan status `Exited`.

Ini penting untuk memahami perbedaan:

```text
docker ps       → container yang sedang hidup
docker ps -a    → semua container
```

**Jangan hapus container PostgreSQL dan Redis kamu.** Kita cukup latihan menggunakan `belajar-nginx`.

---

---

Betul 👍 Kita lanjut **pelan-pelan**, dan saya ingat target latihan kita: setelah selesai, **container `belajar-nginx` dan image `nginx` kita hapus**. PostgreSQL dan Redis **jangan disentuh**.

## Langkah berikutnya: Stop → Start lagi

Sekarang kita belajar perbedaan **stop** dan **start**.

### 1. Stop container

Jalankan:

```powershell
docker stop belajar-nginx
```

Lalu:

```powershell
docker ps
```

`belajar-nginx` harusnya **hilang dari `docker ps`** karena sudah berhenti.

---

### 2. Cek semua container

Jalankan:

```powershell
docker ps -a
```

Kamu akan melihat kira-kira:

```text
belajar-nginx    nginx    Exited
postgres         postgres:16-alpine    Up
redis            redis:6.0.8            Up
```

Artinya:

```text
docker ps
    ↓
yang sedang hidup

docker ps -a
    ↓
semua container
```

---

### 3. Hidupkan kembali container

Sekarang:

```powershell
docker start belajar-nginx
```

Lalu:

```powershell
docker ps
```

`belajar-nginx` akan muncul lagi sebagai `Up`.

Coba buka:

```text
http://localhost:8080
```

Nginx harus muncul lagi.

---

## 🧠 Jadi sekarang kamu sudah mengenal

```text
docker run
    ↓
BUAT container + JALANKAN

docker stop
    ↓
MATIKAN container

docker start
    ↓
NYALAKAN kembali container

docker ps
    ↓
lihat container yang hidup

docker ps -a
    ↓
lihat semua container
```

**Jangan hapus apa pun dulu.** Setelah kamu berhasil `stop → start`, kita lanjut ke **`docker rm` untuk menghapus container**, lalu **`docker rmi` untuk menghapus image nginx**. Ini sekalian latihan bersih-bersih Docker.

---

---

Sip 👍 Kita lanjut **step berikutnya: hapus container Nginx**.

Karena nanti kita juga akan hapus **image `nginx`**, urutannya harus:

```text
Container → hapus dulu
Image     → baru hapus
```

### 1️⃣ Pastikan Nginx hidup

Jalankan:

```powershell
docker ps
```

Kalau ada:

```text
belajar-nginx   nginx   Up ...
```

lanjut.

### 2️⃣ Stop Nginx

```powershell
docker stop belajar-nginx
```

### 3️⃣ Hapus container

```powershell
docker rm belajar-nginx
```

Lalu cek:

```powershell
docker ps -a
```

`belajar-nginx` **sudah tidak boleh ada**.

---

### 🧠 Bedanya `stop` dan `rm`

Bayangkan:

```text
stop
↓
Rumahnya masih ada
Orangnya keluar

rm
↓
Rumahnya dibongkar
```

Jadi:

```text
docker stop belajar-nginx
        ↓
Container mati

docker rm belajar-nginx
        ↓
Container dihapus
```

**Jangan jalankan `docker rm postgres` atau `docker rm redis`**, kita hanya menghapus `belajar-nginx`.

Setelah `docker rm belajar-nginx` berhasil, **jangan hapus image dulu**. Kirim hasil `docker ps -a` ke saya. Kita lanjut **menghapus image `nginx`** setelah itu.

---

---

Mantap 👍 **`belajar-nginx` sudah benar-benar terhapus.**

Saya lihat di `docker ps -a`, memang sudah **tidak ada**:

```text
belajar-nginx
```

Container lain seperti PostgreSQL, Redis, Kafka, Elasticsearch, dll **jangan kita hapus**, karena kemungkinan masih digunakan project kamu.

Sekarang kita masuk ke langkah terakhir untuk latihan Nginx:

## 1️⃣ Cek image Nginx

Jalankan:

```powershell
docker images
```

Cari baris:

```text
nginx    latest    ...
```

Kalau masih ada, itu normal.

### Kenapa image masih ada?

Karena tadi kita hanya menghapus **container**.

Ingat:

```text
IMAGE
  ↓
docker run
  ↓
CONTAINER
```

Ketika kita menjalankan:

```powershell
docker rm belajar-nginx
```

yang dihapus hanya:

```text
❌ Container belajar-nginx
```

Sedangkan:

```text
✅ Image nginx
```

masih ada.

---

## 2️⃣ Hapus image Nginx

**Tapi sebelum menjalankan ini, kirim dulu hasil `docker images` ke saya.**

Kita cek dulu supaya tidak salah hapus image lain.

Jalankan:

```powershell
docker images
```

Lalu kirim hasilnya ke saya.

Setelah itu kita akan lakukan:

```text
Container nginx → sudah dihapus ✅
          ↓
Image nginx → kita hapus ⏭️
```

Dan setelah image-nya terhapus, latihan **Image & Container dasar** kita selesai. 🎯

---

---

Bagus 👍 Sekarang sudah jelas. Image `nginx:latest` **tidak sedang dipakai** karena kolom `U In Use` tidak muncul di baris Nginx.

Jadi aman untuk latihan kita hapus **hanya image Nginx**.

### 1️⃣ Hapus image Nginx

Jalankan:

```powershell
docker rmi nginx:latest
```

Kalau berhasil, biasanya muncul kira-kira:

```text
Untagged: nginx:latest
Deleted: sha256:...
```

### 2️⃣ Cek lagi

```powershell
docker images
```

Pastikan:

```text
nginx:latest
```

sudah tidak ada.

---

### 🧠 Sekarang bedakan 4 perintah ini

| Perintah | Fungsi |
|---|---|
| `docker run` | Buat + jalankan container |
| `docker stop` | Matikan container |
| `docker rm` | Hapus container |
| `docker rmi` | Hapus image |

Urutan latihan kita tadi:

```text
docker pull nginx
       ↓
nginx IMAGE
       ↓
docker run
       ↓
belajar-nginx CONTAINER
       ↓
docker stop
       ↓
docker rm
       ↓
belajar-nginx ❌
       ↓
docker rmi nginx
       ↓
nginx IMAGE ❌
```

⚠️ **Jangan gunakan `docker system prune` dulu**, karena Docker kamu punya banyak image/container project seperti PostgreSQL, Redis, Kafka, Elasticsearch, dan lainnya.

Jalankan **hanya**:

```powershell
docker rmi nginx:latest
```

Setelah berhasil, kirim hasilnya. Kita lanjut ke materi **Dockerfile**, yang menurut saya merupakan langkah paling penting berikutnya.

---

---

Mantap 👍 **berhasil.**

Sekarang `nginx:latest` sudah **tidak ada** di daftar `docker images`.

Jadi latihan kita benar-benar bersih:

```text
nginx IMAGE       ❌ sudah dihapus
belajar-nginx     ❌ sudah dihapus
PostgreSQL        ✅ aman
Redis             ✅ aman
Kafka             ✅ aman
Elasticsearch     ✅ aman
```

Dan kamu sekarang sudah memahami siklus dasar Docker:

```text
IMAGE
  │
  │ docker run
  ▼
CONTAINER
  │
  ├── docker stop → berhenti
  │
  ├── docker start → hidup lagi
  │
  └── docker rm → hapus container

IMAGE
  │
  └── docker rmi → hapus image
```

## 🚀 Selanjutnya: Dockerfile

Ini bagian yang **sangat penting** karena nanti kita akan membuat image Docker **sendiri**, bukan cuma mengambil image dari Docker Hub.

Contohnya nanti:

```text
Dockerfile
    ↓
docker build
    ↓
Image buatan kita
    ↓
docker run
    ↓
Container
```

Kita akan membuat contoh yang sangat sederhana dulu, **tidak menyentuh project DigiAsk**.

### Langkah pertama

Buat folder latihan:

```powershell
mkdir belajar-dockerfile
cd belajar-dockerfile
```

Lalu cek:

```powershell
pwd
```

**Berhenti sampai sini dulu.** Kirim hasil `pwd` ke saya, kemudian kita buat `Dockerfile` pertama kamu.

---

---

Perfect 👍 Posisi kita sudah benar:

```text
C:\Users\Admin\Desktop\belajar-dockerfile
```

Sekarang kita mulai **Dockerfile pertama**. Kita buat yang super sederhana dulu supaya kamu paham konsepnya.

## 1. Buat file bernama `Dockerfile`

Di PowerShell, jalankan:

```powershell
New-Item Dockerfile
```

Cek:

```powershell
dir
```

Harus ada:

```text
Dockerfile
```

### 2. Isi Dockerfile

Buka folder ini di VS Code:

```powershell
code .
```

Kemudian buat isi `Dockerfile` menjadi:

```dockerfile
FROM nginx
```

Sederhana sekali 😄

Artinya:

> "Saya mau membuat image baru berdasarkan image `nginx`."

Jadi:

```text
Dockerfile
    │
    │ FROM nginx
    ▼
Image nginx
```

Tapi ingat, **kita tidak akan menggunakan image ini untuk project kamu**. Ini murni latihan.

### 3. Jangan `build` dulu

Setelah kamu membuat:

```dockerfile
FROM nginx
```

**berhenti dulu di sini.**

Kirim ke saya kalau `Dockerfile` sudah dibuat dan isinya `FROM nginx`.

Setelah itu kita akan menjalankan:

```powershell
docker build ...
```

dan saya jelaskan **apa yang sebenarnya terjadi di belakang layar saat `docker build`**.

---

---

Betul 👍 **Dockerfile pertama kamu sudah benar.**

Isinya:

```dockerfile
FROM nginx
```

Sekarang kita lakukan **`docker build`**.

## 1. Build image

Pastikan terminal masih berada di:

```text
C:\Users\Admin\Desktop\belajar-dockerfile
```

Lalu jalankan:

```powershell
docker build -t belajar-nginx .
```

Perhatikan bagian ini:

```text
-t belajar-nginx
```

Artinya kita memberi nama image:

```text
belajar-nginx
```

Sedangkan:

```text
.
```

artinya:

> Docker gunakan folder yang sedang saya buka sekarang sebagai **build context**.

Jadi alurnya:

```text
C:\Users\Admin\Desktop\belajar-dockerfile
              │
              ├── Dockerfile
              │       │
              │       │ FROM nginx
              │       ▼
              │   docker build
              │       │
              │       ▼
              └── IMAGE: belajar-nginx
```

### 2. Setelah selesai

Jalankan:

```powershell
docker images
```

Nanti kita cari:

```text
belajar-nginx
```

⚠️ **Jangan jalankan `docker run` dulu.**

Jalankan hanya:

```powershell
docker build -t belajar-nginx .
```

Lalu kirim hasilnya ke saya. Kita lihat bersama apa yang terjadi saat Docker membuat image.

---

---

Mantap! 🎉 **Berhasil.** Sekarang kamu sudah membuat **image Docker sendiri** bernama `belajar-nginx`.

Perhatikan:

```text
belajar-nginx:latest    08c5d7841346    250MB
```

## 🧠 Apa yang sebenarnya terjadi?

Kita punya:

```text
Dockerfile
   │
   │ FROM nginx
   ▼
docker build -t belajar-nginx .
   │
   ▼
IMAGE: belajar-nginx
```

Yang menarik: kamu tadi **sudah menghapus `nginx:latest`**, tetapi ketika `docker build` dijalankan:

```dockerfile
FROM nginx
```

Docker membutuhkan image `nginx`, lalu menggunakan layer yang diperlukan untuk membuat:

```text
belajar-nginx
```

Makanya sekarang kamu punya:

```text
belajar-nginx:latest
```

---

## 🔥 Sekarang kita buktikan image ini bisa membuat container

Jalankan:

```powershell
docker run -d --name belajar-nginx-2 -p 8080:80 belajar-nginx
```

Perhatikan perbedaannya:

Sebelumnya kita menggunakan:

```powershell
docker run ... nginx
```

Sekarang:

```powershell
docker run ... belajar-nginx
```

Artinya kita menjalankan **image buatan kita sendiri**.

Setelah itu:

```powershell
docker ps
```

Dan buka:

```text
http://localhost:8080
```

Harus muncul halaman Nginx lagi. 😎

### Alur lengkapnya sekarang

```text
Dockerfile
   │
   │ docker build
   ▼
belajar-nginx IMAGE
   │
   │ docker run
   ▼
belajar-nginx-2 CONTAINER
   │
   ▼
Nginx berjalan
```

**Jangan hapus apa-apa dulu.** Setelah kita berhasil menjalankan container ini, kita akan belajar satu hal yang sangat penting: **mengubah isi Docker image dengan `COPY`**, baru setelah itu kita bersihkan container + image latihan kita.

---

---

Mantap! Kita lanjut **pelan-pelan tapi sampai benar-benar paham**, bukan cuma copy-paste.

Sekarang kita berada di tahap:

```text
Dockerfile
   ↓
docker build
   ↓
Image: belajar-nginx
```

Sekarang waktunya mengubah:

```text
Image
  ↓
Container
```

### 🚀 Step berikutnya

Jalankan:

```powershell
docker run -d --name belajar-nginx-2 -p 8080:80 belajar-nginx
```

Lalu:

```powershell
docker ps
```

Kalau muncul:

```text
belajar-nginx-2    belajar-nginx    Up ...
```

berarti **image buatan kamu berhasil menjadi container**. 🎉

Kemudian buka:

```text
http://localhost:8080
```

Kalau halaman **Welcome to nginx!** muncul, berarti semuanya sukses.

**Kirim hasil `docker ps` ke saya.** Setelah itu kita masuk ke bagian yang lebih seru: membuat **website HTML kita sendiri di dalam Docker**, sehingga tidak lagi memakai halaman default Nginx. 🔥

---

---

🔥 **Mantap, berhasil 100%!**

Sekarang kamu sudah melakukan alur Docker yang sebenarnya:

```text
Dockerfile
    ↓ docker build
IMAGE
belajar-nginx
    ↓ docker run
CONTAINER
belajar-nginx-2
    ↓
Nginx berjalan
    ↓
localhost:8080
```

Dari output kamu:

```text
20f479db476f   belajar-nginx   ...   Up 3 hours   8080->80   belajar-nginx-2
```

Artinya:

- `belajar-nginx` = **IMAGE**
- `belajar-nginx-2` = **CONTAINER**
- `Up 3 hours` = container sedang berjalan
- `8080->80` = port komputer → port Nginx di container

Dan PostgreSQL + Redis tetap aman. ✅

---

# 🚀 Sekarang kita naik level

Kita akan membuat **website HTML sendiri**.

Saat ini Nginx menampilkan:

> Welcome to nginx!

Kita akan ubah menjadi misalnya:

> **Halo Docker!**  
> Ini website pertama saya yang berjalan di Docker 🚀

Konsepnya nanti:

```text
index.html
     ↓
Dockerfile
     ↓ docker build
Image
     ↓ docker run
Container
     ↓
Nginx
     ↓
Browser
```

## 1. Buat `index.html`

Di folder:

```text
C:\Users\Admin\Desktop\belajar-dockerfile
```

buat file:

```text
index.html
```

Isi dengan:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Belajar Docker</title>
</head>
<body>
    <h1>Halo Docker! 🚀</h1>
    <p>Ini website pertama saya yang berjalan di Docker.</p>
</body>
</html>
```

---

## 2. Ubah Dockerfile

Sekarang Dockerfile kita:

```dockerfile
FROM nginx
```

ubah menjadi:

```dockerfile
FROM nginx

COPY index.html /usr/share/nginx/html/index.html
```

### 🧠 Apa arti `COPY`?

Ini sangat penting.

```dockerfile
COPY index.html /usr/share/nginx/html/index.html
```

Artinya:

> Ambil `index.html` dari komputer saya → masukkan ke dalam image Docker → letakkan di lokasi HTML Nginx.

Gambarnya:

```text
KOMPUTER KAMU
│
├── Dockerfile
│
└── index.html
       │
       │ COPY
       ▼
DOCKER IMAGE
│
└── /usr/share/nginx/html/index.html
       │
       ▼
CONTAINER
       │
       ▼
NGINX
       │
       ▼
localhost:8080
```

### ⚠️ Tapi jangan build dulu

Untuk sekarang, cukup:

1. Buat `index.html`
2. Ubah `Dockerfile` menjadi:

```dockerfile
FROM nginx

COPY index.html /usr/share/nginx/html/index.html
```

Setelah selesai, **kirim isi Dockerfile kamu ke saya**.

Nanti kita lakukan `docker build` lagi dan saya jelaskan kenapa **setiap kali Dockerfile berubah, kita perlu membuat image baru**. 😎

---

---

Perfect! ✅ Dockerfile kamu **sudah benar**.

Sekarang kita masuk ke konsep penting: **Build ulang image**.

Saat ini container `belajar-nginx-2` masih menggunakan **image versi lama**. Jadi meskipun Dockerfile sudah berubah, container yang sedang berjalan **belum otomatis berubah**.

### 1️⃣ Build image baru

Jalankan dari folder:

```text
C:\Users\Admin\Desktop\belajar-dockerfile
```

```powershell
docker build -t belajar-nginx:v2 .
```

Perhatikan kita menggunakan:

```text
belajar-nginx:v2
```

`v2` adalah **tag/versi** image.

Sebelumnya:

```text
belajar-nginx:latest
```

Sekarang:

```text
belajar-nginx:v2
```

---

### 2️⃣ Cek image

Setelah build selesai:

```powershell
docker images
```

Kamu kemungkinan akan melihat:

```text
belajar-nginx    v2       ...
belajar-nginx    latest   ...
```

Artinya kita sekarang punya **dua versi image**.

```text
Dockerfile
    │
    │ docker build
    ▼
belajar-nginx:v2
```

---

### ⚠️ Jangan hapus container lama dulu

Container:

```text
belajar-nginx-2
```

masih berjalan menggunakan:

```text
belajar-nginx:latest
```

Nanti kita akan belajar cara menggantinya dengan container baru yang menggunakan:

```text
belajar-nginx:v2
```

Ini konsep yang sangat penting di Docker:

> **Image yang sudah dibuat tidak otomatis berubah ketika Dockerfile berubah.**

Urutannya:

```text
Dockerfile berubah
       ↓
docker build
       ↓
Image baru
       ↓
docker run
       ↓
Container baru
```

Jalankan dulu:

```powershell
docker build -t belajar-nginx:v2 .
```

**Kirim output build-nya ke saya.** Setelah itu kita lanjut membuat container versi `v2`.

---

---

Mantap 👍 `belajar-nginx:v2` **sudah berhasil dibuat**.

Dari output kamu:

```text
belajar-nginx:latest   08c5d7841346   250MB   66.3MB   U
belajar-nginx:v2       45f45a79fc34   250MB   66.3MB
```

Artinya sekarang ada **2 versi image**:

- `belajar-nginx:latest` → versi lama
- `belajar-nginx:v2` → versi baru yang sudah berisi `index.html` kamu
- `U` = **Used / sedang digunakan oleh container**

Yang menarik: ukuran keduanya sama-sama 250 MB. **Tidak masalah.** Docker menggunakan layer sehingga ukuran yang terlihat bukan berarti benar-benar menggandakan seluruh data di disk.

### Sekarang kita test `v2`

Karena container lama `belajar-nginx-2` masih menggunakan port `8080`, kita bisa menjalankan v2 di port **8081** supaya tidak mengganggu container lama.

Jalankan:

```powershell
docker run -d --name belajar-nginx-v2 -p 8081:80 belajar-nginx:v2
```

Lalu:

```powershell
docker ps
```

Harusnya ada kira-kira:

```text
belajar-nginx-v2    ...    0.0.0.0:8081->80/tcp
belajar-nginx-2    ...    0.0.0.0:8080->80/tcp
```

Kemudian buka browser:

**http://localhost:8081**

Harus muncul:

> **Halo Docker! 🚀**  
> Ini website pertama saya yang berjalan di Docker.

### 🧠 Pelajaran penting

Sekarang kita punya:

```text
Dockerfile
    ↓
docker build
    ↓
belajar-nginx:v2  ← IMAGE
    ↓
docker run
    ↓
belajar-nginx-v2  ← CONTAINER
```

Dan ini konsep yang **sangat penting**:

> Mengubah `Dockerfile` **tidak mengubah container yang sudah berjalan**.

Makanya kita membuat **image v2 baru**, kemudian membuat **container baru** dari image v2.

Coba jalankan `docker run` di atas dan buka `http://localhost:8081`.

---

Mantap! 🎉 Itu berarti **container `belajar-nginx-v2` berhasil dibuat dan dijalankan**.

ID ini:

```text
8784661bf42e69bbe32ae79b37a775fafe1fb3df6ea3b3b2dc02102dda5
```

adalah **Container ID**.

Sekarang cek:

```powershell
docker ps
```

Kamu seharusnya melihat:

```text
belajar-nginx-v2
```

dengan port:

```text
0.0.0.0:8081->80/tcp
```

### 🌐 Sekarang buka browser

Masuk ke:

```text
http://localhost:8081
```

Kalau benar, harus muncul:

> **Halo Docker! 🚀**  
> Ini website pertama saya yang berjalan di Docker.

### 🧠 Perhatikan perbedaannya

Sekarang kamu punya:

```text
belajar-nginx:v2          ← IMAGE
        ↓
belajar-nginx-v2          ← CONTAINER
        ↓
localhost:8081
        ↓
Nginx
        ↓
index.html kamu
```

Sedangkan container lama:

```text
belajar-nginx:latest
        ↓
belajar-nginx-2
        ↓
localhost:8080
```

Jadi sekarang kamu bahkan bisa menjalankan **versi lama dan versi baru bersamaan**:

- `http://localhost:8080` → versi lama
- `http://localhost:8081` → versi baru 🚀

Ini contoh kecil konsep **versioning/deployment** yang nanti sangat berguna ketika kita belajar Docker untuk Spring Boot dan staging/production.

---

Oke 👍 Selanjutnya kita masuk ke **Volume & Storage**, tapi sebelum itu ada satu konsep Dockerfile yang bagus kita kuasai dulu: **`CMD` dan `EXPOSE`**.

Kita belajar pelan-pelan.

## 🐳 Level berikutnya: Dockerfile lebih dalam

Dockerfile kamu sekarang:

```dockerfile
FROM nginx

COPY index.html /usr/share/nginx/html/index.html
```

Kita tambahkan:

```dockerfile
FROM nginx

COPY index.html /usr/share/nginx/html/index.html

EXPOSE 80
```

### Apa itu `EXPOSE 80`?

Gampangnya:

> `EXPOSE 80` = memberi tahu bahwa aplikasi di dalam container menggunakan **port 80**.

Tapi ingat ⚠️:

```dockerfile
EXPOSE 80
```

**tidak otomatis membuat port bisa diakses dari Windows.**

Tetap saat `docker run` kita perlu:

```powershell
-p 8081:80
```

Jadi:

```text
Windows                  Container
8081       ───────────→    80
                 -p
```

---

## ✏️ Praktik

Buka Dockerfile kamu:

```text
C:\Users\Admin\Desktop\belajar-dockerfile\Dockerfile
```

ubah menjadi:

```dockerfile
FROM nginx

COPY index.html /usr/share/nginx/html/index.html

EXPOSE 80
```

Simpan.

Lalu build lagi:

```powershell
docker build -t belajar-nginx:v3 .
```

Setelah selesai, **jangan hapus container yang sekarang dulu**.

Kirimkan hasil:

```powershell
docker images
```

Nanti kita lanjut ke **`CMD`**, lalu setelah itu baru masuk ke **Volume**. 🐳🚀

---

Mantap 👍 **`belajar-nginx:v3` berhasil dibuat.**

Perhatikan ini:

```text
belajar-nginx:latest
belajar-nginx:v2
belajar-nginx:v3
```

Sekarang kamu sudah punya **3 versi image** hasil dari Dockerfile.

```text
Dockerfile
   │
   ├── build → belajar-nginx:latest
   │
   ├── build → belajar-nginx:v2
   │
   └── build → belajar-nginx:v3
```

Dan `v3` belum ada tanda `U`, karena **belum dipakai oleh container**.

---

# 🧠 Sekarang kita belajar `CMD`

Sebenarnya image `nginx` sudah punya `CMD` bawaan.

Kamu bisa melihatnya dengan:

```powershell
docker inspect belajar-nginx:v3
```

Tapi output-nya akan sangat panjang 😅.

Lebih mudah:

```powershell
docker inspect belajar-nginx:v3 --format='{{json .Config.Cmd}}'
```

Coba jalankan itu.

Kemungkinan hasilnya kira-kira:

```text
["/docker-entrypoint.sh","nginx","-g","daemon off;"]
```

### Apa artinya?

`CMD` adalah:

> **Perintah default yang dijalankan ketika container dibuat dari image.**

Jadi secara sederhana:

```text
Image
  ↓
docker run
  ↓
CMD dijalankan
  ↓
Aplikasi hidup
```

Pada Nginx, perintah tersebut membuat Nginx tetap berjalan di dalam container.

---

### ⚠️ Jangan ubah Dockerfile dulu

Cukup jalankan:

```powershell
docker inspect belajar-nginx:v3 --format='{{json .Config.Cmd}}'
```

Kirim hasilnya ke saya.

Setelah itu saya jelaskan **kenapa container Docker bisa langsung mati kalau proses `CMD`-nya selesai**. Ini konsep Docker yang sangat penting sebelum kita masuk ke **Volume**. 🐳

---

Betul 👍 Hasilnya:

```text
["nginx","-g","daemon off;"]
```

Sekarang kita bedah **pelan-pelan**.

### 🧠 `CMD` itu apa?

Docker image punya perintah default yang dijalankan ketika container dibuat.

Dalam kasus Nginx:

```text
nginx -g "daemon off;"
```

Artinya kira-kira:

```text
jalankan Nginx
dan jangan keluar dari proses utama
```

Kenapa **`daemon off`**?

Biasanya Nginx bisa berjalan sebagai **background process** (daemon).

Tetapi Docker membutuhkan **proses utama yang tetap hidup**.

Jadi:

```text
Docker Container
       │
       └── nginx
            │
            └── tetap berjalan
                 ↓
             Container tetap UP
```

Kalau proses utama selesai:

```text
nginx selesai
     ↓
proses utama selesai
     ↓
container STOP
```

Ini konsep yang **sangat penting** di Docker.

---

## 🔥 Sekarang kita coba eksperimen

Kita akan menjalankan image `v3`, tetapi **mengganti CMD sementara**.

Jalankan:

```powershell
docker run --name belajar-cmd nginx echo "Halo dari Docker"
```

Perintah ini artinya:

```text
nginx              → image
echo "Halo..."     → ganti CMD bawaan
```

Docker akan menjalankan:

```text
echo "Halo dari Docker"
```

Kemudian proses `echo` selesai.

Akibatnya container juga akan **langsung berhenti**.

Setelah menjalankan perintah tersebut, cek:

```powershell
docker ps -a
```

Kamu akan melihat `belajar-cmd` dengan status kira-kira:

```text
Exited (0)
```

### Kenapa `Exited (0)`?

`0` artinya:

> Program selesai **dengan sukses**.

Jadi **container berhenti bukan berarti error**.

Ini berbeda dengan:

```text
Exited (1)
Exited (137)
```

yang biasanya menunjukkan ada masalah/termination tertentu.

---

Coba jalankan **dua perintah ini**:

```powershell
docker run --name belajar-cmd nginx echo "Halo dari Docker"
```

lalu:

```powershell
docker ps -a
```

Kirim hasilnya. Setelah itu kita akan belajar **`docker logs`**, yang sangat berguna ketika nanti aplikasi Spring Boot kamu error di Docker. 🐳

---

Nah, ini **justru hasil praktik yang bagus** 👍 Kamu sudah melihat sendiri bagaimana `CMD` bekerja.

Ada satu hal kecil: kamu menjalankan perintah yang sama beberapa kali. Pada **percobaan pertama**, sebenarnya semuanya berhasil.

### 1. Percobaan pertama berhasil ✅

Ini bagian penting:

```text
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
...
Halo dari Docker
```

Artinya Docker:

```text
Belum punya nginx:latest
        ↓
Download dari Docker Hub
        ↓
Buat container belajar-cmd
        ↓
Jalankan echo
        ↓
"Halo dari Docker"
        ↓
echo selesai
        ↓
Container STOP
```

Dan terbukti di:

```text
belajar-cmd   nginx   ...   Exited (0)
```

---

### 2. Kenapa percobaan kedua error? ❌

Karena container bernama:

```text
belajar-cmd
```

**sudah dibuat.**

Walaupun statusnya `Exited`, namanya masih terdaftar.

Jadi ketika kamu menjalankan:

```powershell
docker run --name belajar-cmd ...
```

Docker berkata:

```text
Conflict. The container name "/belajar-cmd" is already in use
```

Artinya:

> "Nama `belajar-cmd` sudah dipakai."

---

## 🧹 Sekarang hapus container latihan ini

Karena `belajar-cmd` memang hanya untuk latihan:

```powershell
docker rm belajar-cmd
```

Lalu cek:

```powershell
docker ps -a
```

`belajar-cmd` seharusnya sudah hilang.

**Jangan hapus container lain** seperti:

```text
postgres
redis
belajar-nginx-2
belajar-nginx-v2
```

karena kita masih menggunakannya untuk latihan/project.

---

# 📝 Pelajaran hari ini

Sekarang kamu sudah memahami:

| Perintah | Fungsi |
|---|---|
| `docker images` | Melihat image |
| `docker ps` | Melihat container yang sedang hidup |
| `docker ps -a` | Melihat semua container |
| `docker run` | Membuat + menjalankan container |
| `docker stop` | Menghentikan container |
| `docker start` | Menyalakan kembali container |
| `docker rm` | Menghapus container |
| `docker rmi` | Menghapus image |
| `docker build` | Membuat image dari Dockerfile |
| `docker logs` | Melihat output/log container |
| `CMD` | Perintah default ketika container dijalankan |
| `EXPOSE` | Mendokumentasikan port aplikasi dalam image |

Dan konsep paling penting:

```text
IMAGE
  ↓
docker run
  ↓
CONTAINER
  ↓
proses utama/CMD
  ↓
selesai → CONTAINER STOP
```

Kalau kamu sudah hapus `belajar-cmd`, **langkah berikutnya saya sarankan kita belajar `docker logs` dulu**. Ini sangat berguna nanti saat kamu menjalankan **Spring Boot + Docker** dan aplikasinya tiba-tiba `Exited (1)`.

---

Okeee 😄 Kita lanjut pelan-pelan.

## 🐳 Belajar `docker logs`

`docker logs` sangat sederhana:

> **Untuk melihat apa yang dikeluarkan oleh aplikasi di dalam container.**

Contoh:

```powershell
docker logs nama-container
```

### Kita praktik

Kita sudah punya container:

```text
belajar-nginx-v2
```

Coba jalankan:

```powershell
docker logs belajar-nginx-v2
```

Kemungkinan output-nya **kosong atau sangat sedikit**. Itu normal, karena Nginx belum menerima request yang menghasilkan log ke stdout/stderr.

---

### Kita buat contoh yang lebih jelas

Jalankan:

```powershell
docker run --name belajar-logs alpine echo "Halo, ini log Docker!"
```

Kemudian:

```powershell
docker logs belajar-logs
```

Harus muncul:

```text
Halo, ini log Docker!
```

Perhatikan alurnya:

```text
docker run
    ↓
alpine
    ↓
echo "Halo, ini log Docker!"
    ↓
output
    ↓
container selesai
```

Tetapi walaupun container sudah `Exited`, log-nya **masih bisa dilihat**.

Coba setelah itu:

```powershell
docker ps -a
```

Kamu akan melihat:

```text
belajar-logs    alpine    Exited (0)
```

Lalu:

```powershell
docker logs belajar-logs
```

tetap menampilkan:

```text
Halo, ini log Docker!
```

### 🔥 Ini penting untuk kerja kamu

Nanti kalau Spring Boot kamu misalnya:

```text
Exited (1)
```

jangan langsung panik 😄

Kita bisa melakukan:

```powershell
docker logs nama-container
```

untuk mencari tahu **kenapa aplikasi mati**.

---

Sekarang jalankan 3 perintah ini:

```powershell
docker run --name belajar-logs alpine echo "Halo, ini log Docker!"
```

```powershell
docker logs belajar-logs
```

```powershell
docker ps -a
```

Kirim hasilnya. Setelah itu kita coba **`docker logs -f`**, yaitu melihat log secara **live/real-time**.

---

Nah, **berhasil** 👍 Bahkan ini menunjukkan kamu sudah memahami pola Docker dengan benar.

Yang terjadi:

### 1. `docker run` pertama sebenarnya sudah berhasil

Container ini sudah ada:

```text
belajar-logs
```

dengan status:

```text
Exited (0)
```

Jadi ketika kamu menjalankan lagi:

```powershell
docker run --name belajar-logs alpine echo "Halo, ini log Docker!"
```

Docker menolak karena **nama `belajar-logs` sudah digunakan**.

Ini sama persis seperti kasus `belajar-cmd` sebelumnya.

---

### 2. `docker logs` berhasil 🎯

Kamu menjalankan:

```powershell
docker logs belajar-logs
```

dan mendapatkan:

```text
Halo, ini log Docker!
```

Berarti:

```text
Container
   ↓
echo "Halo, ini log Docker!"
   ↓
Output
   ↓
docker logs
   ↓
Halo, ini log Docker!
```

Yang penting:

> **Container sudah `Exited`, tetapi log-nya masih bisa dibaca.**

---

# 🔥 Sekarang kita belajar `docker logs -f`

`-f` = **follow**

Artinya:

> "Tampilkan log dan terus pantau log baru yang masuk."

Ini sering digunakan untuk melihat aplikasi **secara real-time**.

Contohnya nanti:

```powershell
docker logs -f nama-container
```

Untuk Nginx kita bisa coba:

```powershell
docker logs -f belajar-nginx-v2
```

Setelah itu **jangan tutup PowerShell tersebut**.

Kemudian buka browser:

```text
http://localhost:8081
```

Setiap browser mengakses Nginx, kemungkinan akan muncul log request seperti:

```text
GET / HTTP/1.1
GET /favicon.ico HTTP/1.1
```

Jadi kita bisa melihat:

```text
Browser
   ↓
localhost:8081
   ↓
Nginx
   ↓
log muncul secara realtime
   ↓
docker logs -f
```

### 🛑 Cara menghentikan `logs -f`

Tekan:

```text
Ctrl + C
```

**Jangan khawatir:** `Ctrl + C` di sini hanya menghentikan tampilan `docker logs -f`, **bukan menghentikan container Nginx**.

---

## Coba sekarang

Jalankan:

```powershell
docker logs -f belajar-nginx-v2
```

Kemudian buka:

```text
http://localhost:8081
```

Refresh beberapa kali.

Setelah melihat log, tekan:

```text
Ctrl + C
```

Lalu beri tahu saya apa yang muncul.

Setelah `docker logs` selesai, **kita masuk ke bagian yang jauh lebih penting: Docker Volume** 💾 — ini yang menjelaskan bagaimana data PostgreSQL/Redis bisa tetap ada walaupun container dihapus.

---

Mantap! 🔥 Kalau sudah mulai paham `image → container → Dockerfile → CMD → logs`, sekarang kita masuk ke **salah satu konsep Docker paling penting: Volume**.

Ini penting banget untuk kamu karena nanti berhubungan langsung dengan **PostgreSQL, Redis, dan Spring Boot**.

# 🐳 LEVEL 5 — Docker Volume 💾

Bayangkan container seperti **rumah sementara**.

```text
Container
┌──────────────────────┐
│  Aplikasi             │
│  File                 │
│  Database             │
└──────────────────────┘
```

Kalau container dihapus:

```text
docker rm container
        ↓
🏠 Rumah dihancurkan
        ↓
data di dalamnya bisa ikut hilang
```

Nah, **Volume** adalah tempat penyimpanan yang dibuat supaya data bisa tetap hidup **di luar lifecycle container**.

```text
             Volume 💾
                │
                │
        ┌───────▼───────┐
        │   Container   │
        │               │
        │   Database    │
        └───────────────┘
```

Kalau container dihapus:

```text
Container ❌
    ↓
Volume ✅
    ↓
Data masih ada
```

---

# 🧪 Kita praktik

Kita tidak akan menyentuh PostgreSQL kamu yang sedang berjalan. Kita buat container latihan baru.

## Step 1 — Buat Volume

Jalankan:

```powershell
docker volume create belajar-volume
```

Kalau berhasil, hasilnya:

```text
belajar-volume
```

Kemudian cek:

```powershell
docker volume ls
```

Kamu akan melihat:

```text
DRIVER    VOLUME NAME
local     belajar-volume
```

---

# 🧠 Apa yang baru saja terjadi?

Perintah:

```powershell
docker volume create belajar-volume
```

artinya:

> "Docker, tolong buatkan tempat penyimpanan bernama `belajar-volume`."

Belum ada container yang menggunakan volume tersebut.

Jadi sekarang:

```text
Docker
  │
  └── Volume
       └── belajar-volume 💾
```

---

## Step 2 — Kita buat container Alpine

Sekarang jalankan:

```powershell
docker run -it --name belajar-volume-1 -v belajar-volume:/data alpine sh
```

Ada beberapa hal baru di sini 😎

### `-it`

Membuat kita bisa masuk ke terminal container.

### `-v`

Artinya **mount volume**.

Bagian ini:

```text
-v belajar-volume:/data
```

berarti:

```text
Volume Docker          Container
belajar-volume  ───→   /data
```

Jadi folder:

```text
/data
```

di dalam container sebenarnya terhubung dengan:

```text
belajar-volume
```

---

# 🧪 Setelah masuk container

Kemungkinan prompt berubah menjadi seperti:

```text
/ #
```

Sekarang ketik:

```sh
echo "Data saya aman di volume Docker" > /data/data.txt
```

Kemudian cek:

```sh
cat /data/data.txt
```

Harus muncul:

```text
Data saya aman di volume Docker
```

🎯 **Jangan keluar dulu dengan `exit`.**

Setelah kamu sampai di prompt:

```text
/ #
```

dan berhasil membuat `data.txt`, kasih tahu saya.

Kita kemudian akan melakukan eksperimen yang menurut saya bakal bikin kamu **benar-benar paham kenapa Volume penting untuk PostgreSQL**. 😁🔥

---

Siap! Kita **lanjut persis dari titik itu**. 😄🔥

Posisi kita sekarang seharusnya:

```text
Volume: belajar-volume
        │
        ▼
Container: belajar-volume-1
        │
        ▼
/data/data.txt
        │
        ▼
"Data saya aman di volume Docker"
```

Sekarang kita akan melakukan eksperimen yang paling penting:

> **Container pertama kita hapus → buat container baru → cek apakah `data.txt` masih ada.**

Kalau masih ada, berarti kamu benar-benar sudah memahami fungsi Volume. 💾

---

# 🧪 Eksperimen Volume

## STEP 1 — Keluar dari container

Kalau prompt kamu sekarang seperti:

```text
/ #
```

ketik:

```sh
exit
```

Kamu akan kembali ke PowerShell:

```text
PS C:\Users\Admin\Desktop\belajar-dockerfile>
```

---

## STEP 2 — Lihat container

Jalankan:

```powershell
docker ps -a
```

Cari:

```text
belajar-volume-1
```

Kemungkinan statusnya:

```text
Exited (0)
```

Kenapa `Exited`?

Karena tadi kita menjalankan:

```text
alpine sh
```

dan kita sudah keluar dari shell dengan `exit`.

**Container berhenti, tapi volume masih ada.**

---

# STEP 3 — Hapus CONTAINER

Sekarang kita sengaja hapus container:

```powershell
docker rm belajar-volume-1
```

Hasilnya biasanya:

```text
belajar-volume-1
```

Sekarang cek:

```powershell
docker ps -a
```

`belajar-volume-1` sudah tidak ada. ❌

Tapi...

```powershell
docker volume ls
```

Harus masih ada:

```text
local    belajar-volume
```

🔥 **Ini inti eksperimen kita.**

```text
Container
belajar-volume-1
      ❌
      │
      │ dihapus
      ▼

Volume
belajar-volume
      ✅
      │
      ▼
data.txt masih tersimpan
```

---

# STEP 4 — Buat CONTAINER BARU

Sekarang kita buat container yang benar-benar baru.

```powershell
docker run -it --name belajar-volume-2 -v belajar-volume:/data alpine sh
```

Perhatikan:

```text
belajar-volume:/data
```

Kita menggunakan **volume yang sama**:

```text
belajar-volume
```

Bukan membuat volume baru.

---

# STEP 5 — Cek isi `/data`

Setelah masuk ke container baru dan muncul:

```text
/ #
```

jalankan:

```sh
ls /data
```

🎯 Seharusnya kamu melihat:

```text
data.txt
```

Sekarang:

```sh
cat /data/data.txt
```

Hasilnya:

```text
Data saya aman di volume Docker
```

🔥🔥🔥

**Nah! Ini momen penting.**

Kita baru saja membuktikan:

```text
CONTAINER LAMA
     │
     ├── data.txt
     │
     ▼
belajar-volume 💾
     │
     │
     │ container dihapus
     ▼
CONTAINER BARU
     │
     ▼
mount belajar-volume
     │
     ▼
data.txt masih ada ✅
```

---

# 🧠 Sekarang bedakan 3 hal ini

Ini penting banget:

### 1. Image

```text
alpine
```

Adalah **cetakan/template**.

### 2. Container

```text
belajar-volume-1
belajar-volume-2
```

Adalah **instance yang menjalankan image**.

### 3. Volume

```text
belajar-volume
```

Adalah **tempat menyimpan data yang ingin dipertahankan**.

Jadi:

```text
          IMAGE
        ┌────────┐
        │ alpine │
        └───┬────┘
            │
       docker run
            │
            ▼
    ┌────────────────┐
    │   Container    │
    │ volume-2       │
    │                │
    │ /data          │
    └───────┬────────┘
            │
          mount
            │
            ▼
    ┌────────────────┐
    │ belajar-volume │
    │                │
    │ data.txt       │
    └────────────────┘
```

---

# 🚀 Dan sekarang hubungkan dengan PostgreSQL

Nah, **sekarang konsep yang tadi kamu tanyakan tentang `postgres_data` akan menjadi jauh lebih masuk akal.**

Misalnya:

```text
             postgres_data 💾
                    │
                    │ mount
                    ▼
          ┌──────────────────┐
          │ PostgreSQL       │
          │ Container        │
          └──────────────────┘
                    │
                    ▼
          /var/lib/postgresql/data
                    │
          ┌─────────┼─────────┐
          ▼         ▼         ▼
       database  database  database
```

Container PostgreSQL bisa dihapus:

```text
PostgreSQL Container ❌
```

tetapi:

```text
postgres_data 💾 ✅
```

tetap ada.

Kemudian container PostgreSQL baru dibuat:

```text
postgres_data 💾
       │
       ▼
PostgreSQL Container BARU
       │
       ▼
Database lama
       ✅
```

**Persis seperti eksperimen `data.txt` kita.**

---

## ⚠️ Tapi jangan sentuh PostgreSQL DigiAsk dulu

Untuk latihan ini kita hanya menggunakan:

```text
belajar-volume
```

dan:

```text
belajar-volume-1
belajar-volume-2
```

**Jangan menjalankan `docker volume rm postgres_data` atau menghapus volume PostgreSQL project kamu.**

---

Sekarang kamu masih berada di container `belajar-volume-2`.

Jalankan:

```sh
ls /data
```

lalu:

```sh
cat /data/data.txt
```

Kalau muncul:

```text
data.txt
```

dan:

```text
Data saya aman di volume Docker
```

berarti eksperimen **Volume Persistence berhasil 100%**. 🎯

Setelah itu kita lanjut ke bagian yang lebih menarik: **bagaimana melihat volume yang sebenarnya dibuat Docker, `docker volume inspect`, dan bedanya Named Volume vs Bind Mount**.

---
# 🐳 LEVEL 27 — Docker Volume: `docker volume inspect` & Named Volume vs Bind Mount

Sekarang kita bedah **apa sebenarnya yang dibuat Docker ketika kita membuat Volume**.

## Bagian 1 — `docker volume inspect`

Keluar dulu dari container `belajar-volume-2`:

```sh
exit
```

Kemudian di PowerShell jalankan:

```powershell
docker volume inspect belajar-volume
```

Kamu akan mendapatkan output JSON kira-kira seperti:

```json
[
    {
        "CreatedAt": "...",
        "Driver": "local",
        "Labels": {},
        "Mountpoint": "...",
        "Name": "belajar-volume",
        "Options": {},
        "Scope": "local"
    }
]
```

Yang paling penting adalah:

### `Name`

```text
belajar-volume
```

Nama Volume kita.

### `Driver`

```text
local
```

Artinya volume dikelola oleh Docker di mesin lokal.

### `Mountpoint`

Ini adalah **lokasi penyimpanan yang digunakan Docker untuk volume tersebut**.

⚠️ Karena kamu menggunakan Docker Desktop + Windows + WSL2, path ini bisa terlihat berbeda dari folder Windows biasa. **Jangan edit isi database Docker secara manual dari Mountpoint**, terutama nanti untuk PostgreSQL.

---

## Bagian 2 — Bagaimana Docker melakukan Mount?

Kita punya:

```text
Volume
belajar-volume
       │
       │ mount
       ▼
Container
/data
```

Ketika kita menjalankan:

```powershell
docker run -it `
  --name belajar-volume-3 `
  -v belajar-volume:/data `
  alpine sh
```

Docker melakukan:

```text
┌───────────────────────────────┐
│ Docker Volume                 │
│                               │
│ belajar-volume                │
│                               │
│ data.txt                      │
└───────────────┬───────────────┘
                │
                │ mount
                ▼
┌───────────────────────────────┐
│ Container                     │
│                               │
│ /data                         │
│   └── data.txt                │
└───────────────────────────────┘
```

Jadi `/data` **bukan volume**.

`/data` adalah **mount point di dalam container**.

Sedangkan:

```text
belajar-volume
```

adalah **Docker Volume**.

---

## Bagian 3 — Named Volume vs Bind Mount

Nah, ini penting banget.

Ada dua cara umum menyimpan file dari container.

### A. Named Volume

Yang kita lakukan tadi:

```powershell
-v belajar-volume:/data
```

Artinya:

```text
Docker mengelola storage
        ↓
belajar-volume
        ↓
/data di container
```

Cocok untuk:

- PostgreSQL
- MySQL
- Redis
- data aplikasi
- production

---

### B. Bind Mount

Sekarang misalnya kamu punya folder:

```text
C:\Users\Admin\Desktop\belajar-dockerfile\data
```

Kita bisa langsung menghubungkan folder Windows tersebut ke container.

Contohnya:

```powershell
docker run -it `
  --name belajar-bind `
  -v "${HOME}\Desktop\belajar-dockerfile\data:/data" `
  alpine sh
```

Sekarang mekanismenya:

```text
Windows
C:\Users\Admin\Desktop\belajar-dockerfile\data
                │
                │ bind mount
                ▼
Container
/data
```

Kalau kamu membuat:

```sh
echo "Halo dari Windows" > /data/test.txt
```

maka file tersebut juga akan terlihat di:

```text
C:\Users\Admin\Desktop\belajar-dockerfile\data
```

🔥 Ini perbedaan besarnya.

---

## 🆚 Named Volume vs Bind Mount

| | Named Volume | Bind Mount |
|---|---|---|
| Contoh | `belajar-volume:/data` | `./data:/data` |
| Dikelola | Docker | Kita |
| Lokasi | Docker menentukan | Kita menentukan |
| Cocok database | ⭐⭐⭐⭐⭐ | ⭐⭐ |
| Cocok source code | ⭐⭐ | ⭐⭐⭐⭐⭐ |

---

## 🎯 Contoh dunia kerja

Untuk PostgreSQL:

```yaml
volumes:
  - postgres_data:/var/lib/postgresql/data
```

Untuk source code Spring Boot:

```yaml
volumes:
  - ./src:/app/src
```

Jadi:

```text
                  Docker
                    │
        ┌───────────┴───────────┐
        │                       │
        ▼                       ▼
 postgres_data              ./src
 Named Volume              Bind Mount
        │                       │
        ▼                       ▼
 PostgreSQL                Spring Boot
```

---

## ⚠️ Satu hal penting

Jangan berpikir:

> "Volume = backup."

**Volume bukan backup.**

Kalau:

```powershell
docker volume rm belajar-volume
```

maka data di volume tersebut bisa hilang.

Backup adalah mekanisme terpisah, misalnya PostgreSQL:

```text
PostgreSQL
    ↓
pg_dump
    ↓
backup.sql
    ↓
disimpan di tempat lain
```

Jadi sistem production yang bagus biasanya:

```text
PostgreSQL
     │
     ├── Volume → persistence
     │
     └── Backup → disaster recovery
```

---

## 🔥 Praktik kecil

Jalankan:

```powershell
docker volume inspect belajar-volume
```

Setelah itu kita akan membuat **Bind Mount pertama kamu**, lalu kita akan membandingkan langsung:

```text
Named Volume
     VS
Bind Mount
```

Setelah ini baru kita masuk ke **Docker Network**, dan di situ kamu akan mulai melihat bagaimana:

```text
Spring Boot
    ↓
PostgreSQL
    ↓
Redis
    ↓
Kafka
```

bisa saling berbicara **tanpa menggunakan `localhost` di antara container**. 🚀

---


# 🐳 LEVEL 28 — Bedah Detail Volume & Praktik Bind Mount Pertama

Betul 👍 hasilnya **sudah benar**. Sekarang kita bedah pelan-pelan.

## 1. `Name`

```text
"Name": "belajar-volume"
```

Ini nama Volume yang kita buat:

```powershell
docker volume create belajar-volume
```

Jadi Docker mengenali volume tersebut dengan nama `belajar-volume`.

---

## 2. `Driver`

```text
"Driver": "local"
```

Artinya volume menggunakan **local storage**, yaitu penyimpanan yang dikelola oleh Docker di komputer kamu.

Untuk belajar dan kebanyakan kasus Docker biasa, `local` ini yang paling umum.

---

## 3. `Mountpoint` ⭐

```text
"Mountpoint": "/var/lib/docker/volumes/belajar-volume/_data"
```

Ini adalah **lokasi penyimpanan volume di dalam lingkungan Docker**.

Gambaran sederhananya:

```text
Windows
   │
   │ Docker Desktop
   ▼
Docker
   │
   └── Volume
        │
        └── belajar-volume
              │
              └── _data
                   │
                   └── data.txt
```

⚠️ Karena kamu menggunakan **Docker Desktop + Windows**, jangan mencoba mengedit langsung folder `/var/lib/docker/...` tersebut. Kita cukup mengaksesnya melalui Docker.

---

## 4. `Scope`

```text
"Scope": "local"
```

Artinya volume ini hanya berada pada **Docker environment lokal** kamu.

Bukan volume yang otomatis tersimpan di cloud atau server lain.

---

## 5. `CreatedAt`

```text
"CreatedAt": "2026-09-15T02:24:42Z"
```

Ini waktu volume tersebut dibuat.

`Z` menunjukkan waktu UTC.

---

## Jadi kesimpulannya 🧠

Volume kamu:

```text
belajar-volume
       │
       ▼
┌──────────────────────┐
│ Docker Volume        │
│                      │
│ belajar-volume       │
│        │             │
│        ▼             │
│      data.txt        │
└──────────────────────┘
```

Yang paling penting untuk kamu ingat:

> **Volume = tempat menyimpan data yang bisa tetap ada walaupun Container dihapus.**

Contohnya tadi:

```text
Container 1
    │
    └── belajar-volume
            │
            └── data.txt
```

Container 1 dihapus:

```text
Container 1 ❌
```

Volume tetap:

```text
belajar-volume ✅
      │
      └── data.txt
```

Lalu container baru bisa menggunakan volume yang sama:

```text
Container 2
    │
    └── belajar-volume
            │
            └── data.txt ✅
```

---

## Sekarang kita belajar Bind Mount

Ini penting karena kamu akan sering melihat **Volume vs Bind Mount** ketika bekerja dengan Docker + Spring Boot.

Buat folder Windows:

```powershell
mkdir "$HOME\Desktop\belajar-dockerfile\data"
```

Lalu jalankan:

```powershell
docker run -it --name belajar-bind -v "${HOME}\Desktop\belajar-dockerfile\data:/data" alpine sh
```

Kalau berhasil, kamu akan masuk ke dalam container:

```text
/ #
```

Kemudian jalankan:

```sh
echo "Halo dari Bind Mount" > /data/test.txt
```

Lalu:

```sh
cat /data/test.txt
```

Hasilnya:

```text
Halo dari Bind Mount
```

Kemudian:

```sh
exit
```

Setelah keluar dari container, di **PowerShell Windows** jalankan:

```powershell
Get-ChildItem "$HOME\Desktop\belajar-dockerfile\data"
```

Nanti harus terlihat:

```text
test.txt
```

🔥 **Ini eksperimen yang sangat bagus**, karena kamu akan melihat sendiri bedanya:

```text
NAMED VOLUME
Docker ──> belajar-volume ──> data
             ↑
        Docker yang kelola


BIND MOUNT
Windows Folder ──────────> Container
C:\...\data                  /data
             ↑
        Kita yang kelola
```

---

## 🚀 Selanjutnya: Docker Network

Setelah ini kita bisa masuk ke **Docker Network**, yang menurut saya penting banget untuk memahami kenapa **Spring Boot bisa connect ke PostgreSQL/Redis menggunakan nama container**, bukan `localhost`.

---

Siap 👍 Kita lanjut **pelan-pelan**.

# LEVEL 6 — Docker Network 🌐

Sekarang kita akan belajar **bagaimana Container berkomunikasi dengan Container lain**.

Ini penting banget nanti untuk:

```text
Spring Boot
   ↓
PostgreSQL
   ↓
Redis
   ↓
Kafka
```

---

## 🧠 1. Analogi paling sederhana

Bayangkan Docker seperti sebuah kompleks perumahan:

```text
🏠 Container A
🏠 Container B
🏠 Container C
```

Kalau mereka berada di **network Docker yang sama**, mereka bisa saling berkomunikasi.

```text
          Docker Network
     ┌──────────────────────┐
     │                      │
     │  🏠 App              │
     │     │                │
     │     ├── 🏠 PostgreSQL│
     │     │                │
     │     └── 🏠 Redis     │
     │                      │
     └──────────────────────┘
```

---

# 2. Buat Network

Jalankan:

```powershell
docker network create belajar-network
```

Kalau berhasil, Docker akan mengeluarkan ID panjang seperti:

```text
8f3xxxxxxxxxxxxxxxxxxxxxxxx
```

Sekarang cek:

```powershell
docker network ls
```

Kamu akan melihat kurang lebih:

```text
NETWORK ID     NAME               DRIVER    SCOPE
xxxxxx        bridge             bridge    local
xxxxxx        host               host      local
xxxxxx        none               null      local
xxxxxx        belajar-network    bridge    local
```

Yang kita buat adalah:

```text
belajar-network
```

---

# 3. Apa itu `bridge`?

Di hasil tadi ada:

```text
DRIVER
bridge
```

Untuk sekarang cukup ingat:

> **Bridge = jenis network Docker yang memungkinkan container-container berkomunikasi dalam network tersebut.**

Jangan terlalu dipikirkan dulu. 😄

---

# 4. Kita buat Container pertama

Kita akan menggunakan Nginx yang sudah kamu punya:

```text
belajar-nginx:v2
```

Jalankan:

```powershell
docker run -d --name belajar-nginx-network --network belajar-network -p 8082:80 belajar-nginx:v2
```

Perhatikan bagian ini:

```text
--network belajar-network
```

Artinya:

> Masukkan container ini ke `belajar-network`.

Cek:

```powershell
docker ps
```

Harus ada:

```text
belajar-nginx-network
```

dan port:

```text
0.0.0.0:8082->80/tcp
```

Sekarang buka:

```text
http://localhost:8082
```

Website Docker kamu harus muncul. 🚀

---

# 5. Sekarang kita buat Container kedua

Jalankan:

```powershell
docker run -it --name belajar-client --network belajar-network alpine sh
```

Perhatikan:

```text
--network belajar-network
```

Jadi sekarang ada **dua container dalam network yang sama**:

```text
                 belajar-network
                       │
            ┌──────────┴──────────┐
            │                     │
            ▼                     ▼
   belajar-nginx-network    belajar-client
        🌐 Nginx                 🐧 Alpine
```

Kamu sekarang berada di dalam container Alpine.

Prompt-nya kira-kira:

```text
/ #
```

---

# 6. 🔥 Bagian paling penting

Sekarang dari dalam `belajar-client`, kita coba mengakses Nginx.

Jalankan:

```sh
wget -qO- http://belajar-nginx-network
```

Perhatikan!

Kita **tidak** menggunakan:

```text
localhost
```

Tetapi:

```text
belajar-nginx-network
```

Kenapa?

Karena Docker Network memiliki **DNS internal**.

Docker otomatis bisa mengenali nama container.

Jadi:

```text
belajar-client
       │
       │ HTTP request
       ▼
belajar-nginx-network
       │
       ▼
     Nginx
```

Kalau berhasil, kamu akan mendapatkan HTML dari website kita, misalnya:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Belajar Docker</title>
</head>
<body>
    <h1>Halo Docker! 🚀</h1>
    ...
</body>
</html>
```

🎉 Kalau ini berhasil, berarti **Container A sudah berhasil berkomunikasi dengan Container B melalui Docker Network.**

---

## ⭐ Kenapa ini penting untuk Spring Boot?

Nanti ketika kamu punya:

```text
Docker Network
│
├── spring-boot
├── postgres
├── redis
└── kafka
```

Spring Boot **tidak perlu** menggunakan:

```properties
DB_HOST=localhost
```

Kalau PostgreSQL container bernama:

```text
postgres
```

maka Spring Boot bisa menggunakan:

```properties
DB_HOST=postgres
```

Contohnya:

```text
Spring Boot
     │
     │ jdbc
     ▼
  postgres
     │
     ▼
PostgreSQL
```

Dan Redis:

```text
Spring Boot
     │
     │ redis connection
     ▼
   redis
```

Ini akan sangat relevan dengan environment Docker yang kamu gunakan untuk belajar **Spring Boot + PostgreSQL + Redis + Kafka**.

---

### Sekarang lakukan 3 langkah ini saja dulu 👇

**1. Buat network**

```powershell
docker network create belajar-network
```

**2. Jalankan Nginx**

```powershell
docker run -d --name belajar-nginx-network --network belajar-network -p 8082:80 belajar-nginx:v2
```

**3. Jalankan client**

```powershell
docker run -it --name belajar-client --network belajar-network alpine sh
```

Setelah muncul:

```text
/ #
```

jalankan:

```sh
wget -qO- http://belajar-nginx-network
```

**Kirim hasil `wget`-nya ke saya.** Dari situ kita lanjut memahami **kenapa `localhost` berbeda dengan nama container**.

---

🔥 **Berhasil!** Ini bukti bahwa Docker Network kamu sudah bekerja dengan benar.

Yang baru saja terjadi:

```text
belajar-client
     │
     │ HTTP request
     │ http://belajar-nginx-network
     ▼
belajar-nginx-network
     │
     ▼
   Nginx
     │
     ▼
HTML website
```

Dan Docker berhasil menerjemahkan:

```text
belajar-nginx-network
```

menjadi alamat IP container Nginx melalui **DNS internal Docker**.

---

# 🧠 Sekarang bagian yang sangat penting: `localhost`

Kita coba eksperimen supaya kamu benar-benar melihat bedanya.

Kamu sekarang masih berada di:

```text
/ #
```

Di dalam `belajar-client`.

Coba jalankan:

```sh
wget -qO- http://localhost:80
```

Kemungkinan besar hasilnya:

```text
wget: can't connect to remote host: Connection refused
```

### Kenapa?

Karena:

```text
localhost
```

artinya **container yang sedang kamu tempati**.

Jadi ketika kamu menjalankan:

```text
belajar-client
    │
    └── localhost
```

`localhost` menunjuk ke:

```text
belajar-client
```

**BUKAN**:

```text
belajar-nginx-network
```

---

## Bandingkan

### ❌ localhost

```text
belajar-client
      │
      │ localhost
      ▼
belajar-client sendiri
```

### ✅ Nama container

```text
belajar-client
      │
      │ http://belajar-nginx-network
      ▼
belajar-nginx-network
      │
      ▼
     Nginx
```

Inilah salah satu konsep Docker yang **wajib banget kamu pahami**.

---

# 🚀 Hubungannya dengan Spring Boot

Misalnya nanti kita punya:

```text
┌─────────────────────────────────────┐
│          belajar-network            │
│                                     │
│  ┌────────────┐                     │
│  │ Spring Boot│                     │
│  └─────┬──────┘                     │
│        │                            │
│        │ database connection       │
│        ▼                            │
│  ┌────────────┐                     │
│  │  postgres  │                     │
│  └────────────┘                     │
│                                     │
└─────────────────────────────────────┘
```

Maka konfigurasi Spring Boot:

```properties
spring.datasource.url=jdbc:postgresql://postgres:5432/belajar_db
```

**bukan:**

```properties
spring.datasource.url=jdbc:postgresql://localhost:5432/belajar_db
```

Karena `postgres` adalah **nama container/service** di Docker Network.

---

# ⭐ Tapi ada pengecualian penting

Kalau **Spring Boot berjalan langsung di Windows**, sedangkan PostgreSQL berjalan di Docker:

```text
Windows
└── Spring Boot
       │
       │ localhost:5432
       ▼
Docker
└── PostgreSQL
```

Maka:

```properties
localhost:5432
```

bisa benar, karena Spring Boot berada di Windows/host.

Jadi gampangnya:

| Kondisi | Host database |
|---|---|
| Spring Boot di Windows → PostgreSQL Docker | `localhost` |
| Spring Boot Docker → PostgreSQL Docker | `postgres` |
| Container A → Container B | nama container |
| Container → dirinya sendiri | `localhost` |

💡 **Ini nanti akan menjelaskan banyak error connection yang sering muncul saat belajar Docker + Spring Boot.**

---

## Sekarang kita lanjut satu eksperimen kecil

Masih di:

```text
/ #
```

jalankan:

```sh
wget -qO- http://localhost:80
```

Kirim hasilnya ke saya.

Setelah itu kita lanjut ke **Docker Network: `docker network inspect`**, supaya kamu bisa melihat sendiri container mana saja yang masuk ke network dan IP masing-masing.

Betul sekali! 🎯 **Ini hasil yang kita harapkan.** Berarti konsep `localhost` vs nama container sudah terbukti langsung.

### Kenapa `localhost` gagal?

Kamu sedang berada di container:

```text
belajar-client
```

Ketika menjalankan:

```sh
wget -qO- http://localhost:80
```

Docker menganggap:

```text
localhost
   ↓
belajar-client
```

Padahal Nginx ada di container lain:

```text
belajar-nginx-network
```

Jadi tidak ada aplikasi yang sedang listen di port 80 pada `belajar-client` → **Connection refused**.

Sedangkan sebelumnya:

```sh
wget -qO- http://belajar-nginx-network
```

berhasil karena Docker Network punya DNS internal:

```text
belajar-client
      │
      │ http://belajar-nginx-network
      ▼
Docker DNS
      │
      ▼
IP belajar-nginx-network
      │
      ▼
Nginx :80
```

---

# 🔎 Sekarang kita lihat buktinya

Keluar dulu dari container:

```sh
exit
```

Sekarang di PowerShell Windows jalankan:

```powershell
docker network inspect belajar-network
```

Output-nya cukup panjang.

Cari bagian:

```text
"Containers": {
```

Di sana seharusnya ada kira-kira:

```text
"Containers": {
    "...": {
        "Name": "belajar-nginx-network",
        "IPv4Address": "172.xx.xx.x/16"
    },
    "...": {
        "Name": "belajar-client",
        "IPv4Address": "172.xx.xx.x/16"
    }
}
```

Artinya kedua container memang berada di network yang sama:

```text
        belajar-network
              │
       ┌──────┴──────┐
       │             │
       ▼             ▼
    Nginx          Client
172.xx.xx.x     172.xx.xx.x
```

Docker juga membuat DNS sehingga:

```text
belajar-nginx-network
```

bisa digunakan sebagai alamat tujuan dari container lain.

---

### Coba sekarang:

```powershell
docker network inspect belajar-network
```

Kirim hasilnya ke saya.

Nanti saya jelaskan **satu per satu bagian `network inspect`**, terutama `Containers`, `IPv4Address`, dan `Gateway`. Setelah itu baru kita masuk ke konsep yang sangat penting berikutnya: **Docker Compose**. 🚀

Mantap 👍 Output-nya benar. Tapi ada **satu hal menarik** di hasil kamu: sekarang `belajar-network` hanya berisi **`belajar-nginx-network`**, tidak ada `belajar-client`.

Kemungkinan kamu sudah menjalankan `exit` dari `belajar-client`, lalu container-nya berhenti. Container yang berhenti tidak tampil sebagai endpoint aktif di network.

Mari kita pahami output-nya.

## 1. `Name`

```text
"Name": "belajar-network"
```

Ini nama network yang kita buat:

```powershell
docker network create belajar-network
```

---

## 2. `Driver: bridge`

```text
"Driver": "bridge"
```

Ini jenis network-nya.

Untuk sekarang cukup ingat:

> **Bridge = network Docker yang memungkinkan container saling berkomunikasi.**

---

## 3. `Subnet`

```text
"Subnet": "172.19.0.0/16"
```

Docker membuat jaringan internal:

```text
172.19.0.0
     │
     ├── 172.19.0.1
     ├── 172.19.0.2
     ├── 172.19.0.3
     ├── 172.19.0.4
     └── ...
```

Jadi container-container yang masuk network ini mendapatkan IP dari rentang tersebut.

---

## 4. `Gateway`

```text
"Gateway": "172.19.0.1"
```

Anggap saja Gateway sebagai **pintu keluar/gerbang network**.

Gambaran sederhananya:

```text
              Gateway
            172.19.0.1
                 │
         ┌────────┴────────┐
         │ belajar-network │
         │                 │
         │  Nginx          │
         │ 172.19.0.2      │
         │                 │
         └─────────────────┘
```

---

# 5. ⭐ Bagian terpenting: `Containers`

Ini bagian:

```text
"Containers": {
    "...": {
        "Name": "belajar-nginx-network",
        ...
        "IPv4Address": "172.19.0.2/16"
    }
}
```

Artinya:

```text
belajar-nginx-network
        ↓
IP Docker
172.19.0.2
```

Jadi saat kita sebelumnya melakukan:

```sh
wget -qO- http://belajar-nginx-network
```

Docker kurang lebih melakukan:

```text
belajar-nginx-network
        ↓
172.19.0.2
        ↓
Nginx :80
```

Makanya berhasil. 🔥

---

# 6. Kenapa `belajar-client` tidak ada?

Sebelumnya kita menjalankan:

```powershell
docker run -it --name belajar-client --network belajar-network alpine sh
```

Kemudian kamu menjalankan:

```sh
exit
```

Container `belajar-client` berhenti.

Coba cek:

```powershell
docker ps -a
```

Kemungkinan akan ada:

```text
belajar-client    Exited (0)
```

Jadi **container-nya masih ada**, hanya tidak sedang aktif terhubung sebagai endpoint di network.

---

# 🧪 Kita buktikan lagi

Jalankan:

```powershell
docker start -ai belajar-client
```

Perintah ini akan:

- `start` → menjalankan kembali container
- `-a` → attach ke output
- `-i` → interactive

Kamu akan kembali melihat:

```text
/ #
```

Sekarang jangan `exit` dulu.

Di dalam container jalankan:

```sh
wget -qO- http://belajar-nginx-network
```

Harus berhasil lagi.

Kemudian **buka PowerShell baru** dan jalankan:

```powershell
docker network inspect belajar-network
```

Sekarang bagian `Containers` seharusnya memiliki **2 container**:

```text
Containers
│
├── belajar-nginx-network
│      └── 172.19.0.2
│
└── belajar-client
       └── 172.19.0.3
```

IP `belajar-client` kemungkinan `172.19.0.3`, tetapi **jangan diasumsikan**—lihat hasil `inspect` karena Docker yang menentukan.

---

## 🧠 Kesimpulan Network sejauh ini

Kamu sekarang sudah memahami 3 konsep penting:

```text
1. Container
       ↓
2. Network
       ↓
3. Container bisa komunikasi menggunakan nama container
```

Contohnya:

```text
belajar-client
      │
      │ http://belajar-nginx-network
      ▼
belajar-nginx-network
      │
      ▼
172.19.0.2
      │
      ▼
Nginx :80
```

Dan ini adalah dasar dari:

```text
Spring Boot
     │
     ├── postgres
     ├── redis
     └── kafka
```

di Docker.

**Langkah berikutnya setelah eksperimen ini adalah Docker Compose.** Di sana kita akan berhenti membuat container satu-per-satu dengan `docker run`, lalu membuat **Spring Boot + PostgreSQL + Redis** menggunakan satu file `docker-compose.yml`.

Okeee 🔥 sekarang kita masuk ke salah satu bagian **paling penting dalam Docker**:

# LEVEL 7 — Docker Compose 🐳

Sampai sekarang kita membuat container satu-satu seperti:

```powershell
docker run ...
docker run ...
docker run ...
```

Kalau aplikasinya sudah banyak, cara ini bakal merepotkan.

Misalnya project DigiAsk punya:

```text
Spring Boot
PostgreSQL
Redis
Kafka
Zookeeper
Elasticsearch
...
```

Kalau semuanya dibuat manual, command-nya panjang banget. 😵

Docker Compose dibuat untuk mengatasi itu.

---

# 🧠 1. Apa itu Docker Compose?

Bahasa bayi:

> **Docker Compose = file konfigurasi untuk mengatur banyak container sekaligus.**

Misalnya kita tulis:

```text
docker-compose.yml

PostgreSQL
Redis
Nginx
```

Lalu cukup menjalankan:

```powershell
docker compose up
```

Docker akan membuat dan menjalankan semuanya.

---

# 2. Analogi sederhana

Tanpa Compose:

```text
docker run postgres
docker run redis
docker run nginx
docker run ...
docker run ...
```

Dengan Compose:

```text
docker-compose.yml
        │
        ▼
docker compose up
        │
        ├── PostgreSQL
        ├── Redis
        └── Nginx
```

Jadi Compose seperti **"manager" container**.

---

# 3. Kita praktik langsung 🚀

Kita akan membuat project latihan baru supaya **tidak mengganggu PostgreSQL/Redis DigiAsk kamu**.

Buat folder:

```powershell
cd "$HOME\Desktop"
mkdir belajar-compose
cd belajar-compose
```

Cek lokasi:

```powershell
pwd
```

Harus:

```text
C:\Users\Admin\Desktop\belajar-compose
```

---

# 4. Buat file `docker-compose.yml`

Jalankan:

```powershell
New-Item docker-compose.yml
```

Kemudian buka file tersebut:

```powershell
code docker-compose.yml
```

Masukkan:

```yaml
services:

  nginx:
    image: nginx
    container_name: belajar-compose-nginx
    ports:
      - "8083:80"

  redis:
    image: redis:6.0.8
    container_name: belajar-compose-redis
```

Simpan `Ctrl + S`.

---

# 5. Kita baca pelan-pelan

Bagian:

```yaml
services:
```

Artinya:

> Kita akan mendefinisikan container/service yang ingin kita jalankan.

Kemudian:

```yaml
nginx:
```

Ini nama **service**.

Lalu:

```yaml
image: nginx
```

Artinya Docker menggunakan image:

```text
nginx
```

Kemudian:

```yaml
container_name: belajar-compose-nginx
```

Kita memberikan nama container:

```text
belajar-compose-nginx
```

Kemudian:

```yaml
ports:
  - "8083:80"
```

Artinya:

```text
Windows              Container
8083      ────────>    80
```

Jadi:

```text
http://localhost:8083
```

akan mengarah ke Nginx.

---

# 6. Redis

Bagian:

```yaml
redis:
  image: redis:6.0.8
  container_name: belajar-compose-redis
```

Artinya Compose akan membuat container Redis menggunakan:

```text
redis:6.0.8
```

Kita sengaja menggunakan port default Redis **tanpa `ports:`** dulu.

Kenapa?

Karena nanti kita akan belajar bahwa:

> Container dalam satu Compose bisa berkomunikasi melalui network internal tanpa harus membuka port ke Windows.

🔥 Ini konsep yang sangat penting.

---

# 7. Jalankan Compose

Di folder:

```text
C:\Users\Admin\Desktop\belajar-compose
```

jalankan:

```powershell
docker compose up -d
```

Kalau berhasil akan terlihat proses seperti:

```text
[+] Running 3/3
 ✔ Network belajar-compose_default
 ✔ Container belajar-compose-nginx
 ✔ Container belajar-compose-redis
```

Perhatikan ada:

```text
Network belajar-compose_default
```

😮

**Compose otomatis membuat Docker Network untuk project kita!**

Jadi kita tadi membuat network manual:

```powershell
docker network create belajar-network
```

Sekarang Compose membuatnya otomatis.

---

# 8. Cek container

Jalankan:

```powershell
docker compose ps
```

Harus ada kira-kira:

```text
NAME                    STATUS
belajar-compose-nginx   Up
belajar-compose-redis   Up
```

Kemudian:

```powershell
docker ps
```

juga akan menunjukkan keduanya.

---

# 9. Coba Nginx 🌐

Buka browser:

```text
http://localhost:8083
```

Harus muncul:

```text
Welcome to nginx!
```

🎉

---

# 10. Cek network

Sekarang jalankan:

```powershell
docker network ls
```

Kamu akan melihat sesuatu seperti:

```text
NETWORK ID     NAME                       DRIVER
xxxxxx         bridge                     bridge
xxxxxx         belajar-network            bridge
xxxxxx         belajar-compose_default    bridge
```

Compose otomatis membuat:

```text
belajar-compose_default
```

---

# 🧠 11. Ini yang keren

Compose kita sekarang kira-kira seperti ini:

```text
             Docker Compose
                    │
                    ▼
       belajar-compose_default
              │           │
              │           │
              ▼           ▼
           Nginx         Redis
              │           │
              │           │
              └─────┬─────┘
                    │
              Docker Network
```

Dan karena keduanya berada di network yang sama:

```text
belajar-compose-nginx
          ↕
belajar-compose-redis
```

bisa saling berkomunikasi.

---

# ⭐ Konsep yang harus kamu ingat

Docker Compose:

```text
docker-compose.yml
        │
        ▼
   docker compose up
        │
        ├── Container 1
        ├── Container 2
        ├── Container 3
        └── Network
```

Sedangkan:

```powershell
docker compose down
```

digunakan untuk menghentikan dan menghapus resource Compose yang dibuat oleh project tersebut.

**Untuk latihan kita aman menggunakan `docker compose down` di folder ini.**

---

## 🎯 Tugas sekarang

Jalankan **3 command ini**:

```powershell
cd "$HOME\Desktop\belajar-compose"
```

```powershell
docker compose up -d
```

```powershell
docker compose ps
```

Kirim hasil `docker compose ps` ke saya.

Setelah itu kita naik satu level lagi: **kita akan memasukkan PostgreSQL + Volume ke Docker Compose**. Nah, ini mulai mirip dengan environment backend sungguhan. 🚀

Mantap 🔥 **berhasil 100%**.

Dari output kamu:

```text
belajar-compose-nginx   nginx         Up    0.0.0.0:8083->80/tcp
belajar-compose-redis   redis:6.0.8   Up    6379/tcp
```

Artinya Docker Compose sudah berhasil membuat **2 container sekaligus**.

Tapi saya melihat satu hal kecil yang bagus untuk kita pahami:

```text
Network belajar-dockerfile_default
```

Padahal folder/project yang kita rencanakan adalah `belajar-compose`.

Kemungkinan command tadi dijalankan dari folder:

```text
belajar-dockerfile
```

seperti terlihat dari prompt kamu:

```text
 belajar-dockerfile 
```

Jadi Compose menggunakan nama folder **`belajar-dockerfile`** sebagai nama project.

Tidak masalah untuk latihan. 👍

---

# 🧠 Kenapa network-nya otomatis dibuat?

Karena di `docker-compose.yml` kita tidak menulis network apa pun.

Tetapi Compose otomatis membuat:

```text
belajar-dockerfile_default
```

Strukturnya:

```text
                 Docker Compose
                       │
                       ▼
          belajar-dockerfile_default
                 │          │
                 ▼          ▼
              Nginx        Redis
```

Dan keduanya otomatis masuk ke network yang sama.

---

# ⭐ Sekarang kita buktikan DNS Compose

Ini menarik.

Jalankan:

```powershell
docker exec -it belajar-compose-nginx sh
```

Kamu akan masuk ke Nginx:

```text
#
```

Kemudian kita sebenarnya bisa mengakses Redis berdasarkan **nama service**:

```text
redis
```

Namun Nginx container tidak memiliki tool Redis client, jadi kita tidak akan pakai cara itu.

Lebih mudah kita buat container kecil sebagai client.

Jalankan dari PowerShell baru:

```powershell
docker run -it --rm --network belajar-dockerfile_default alpine sh
```

Perhatikan:

```text
--network belajar-dockerfile_default
```

Kita memasukkan Alpine ke network Compose.

Sekarang dari Alpine:

```sh
wget -qO- http://belajar-compose-nginx
```

Harus mendapatkan:

```html
Welcome to nginx!
```

---

# 🚨 Tapi ada konsep yang lebih penting

Di Docker Compose, kita biasanya **tidak menggunakan `container_name` untuk komunikasi**.

Kita bisa menggunakan:

```text
nginx
redis
```

karena itu adalah **nama service**.

Jadi nanti:

```text
Spring Boot
    │
    ├── PostgreSQL → postgres:5432
    │
    ├── Redis      → redis:6379
    │
    └── Kafka      → kafka:9092
```

Bukan:

```text
localhost:5432
localhost:6379
localhost:9092
```

Ini salah satu alasan Docker Compose sangat populer untuk backend development.

---

# 🎯 Sekarang kita naik level

Kita akan ubah `docker-compose.yml` menjadi:

```text
Docker Compose
│
├── Nginx
│
├── Redis
│
└── PostgreSQL
       │
       ▼
    Volume
```

Dan PostgreSQL akan memiliki **data persistence**.

Jadi nanti kita belajar sekaligus:

**Compose + PostgreSQL + Volume + Network** 🔥

Sebelum itu, kita rapikan sedikit latihan Compose kamu.

Dari PowerShell, masuk ke folder yang benar:

```powershell
cd "$HOME\Desktop\belajar-compose"
```

Lalu:

```powershell
docker compose down
```

Kemudian:

```powershell
docker compose up -d
```

Setelah itu:

```powershell
docker compose ps
```

Kirim hasilnya. Kita pastikan Compose sudah menggunakan project folder yang benar, lalu langsung lanjut ke **PostgreSQL + Volume**.

Nah, dari output ini kita bisa memastikan satu hal: **kamu masih berada di folder `belajar-dockerfile`**.

Perhatikan prompt PowerShell kamu:

```text
 belajar-dockerfile 
```

Makanya Compose membuat:

```text
belajar-dockerfile_default
```

bukan:

```text
belajar-compose_default
```

Tapi **container-nya tetap berhasil**. Tidak masalah. 👍

### Kita rapikan dulu

Jalankan:

```powershell
cd "$HOME\Desktop\belajar-compose"
```

Lalu cek:

```powershell
pwd
```

Hasil yang kita inginkan:

```text
Path
----
C:\Users\Admin\Desktop\belajar-compose
```

Setelah itu jalankan:

```powershell
docker compose down
```

Kemudian:

```powershell
docker compose up -d
```

Dan:

```powershell
docker compose ps
```

Network-nya nanti seharusnya berubah menjadi:

```text
belajar-compose_default
```

### Kenapa kita peduli nama network?

Karena nama project Compose secara default berasal dari **nama folder**.

```text
Folder
   ↓
belajar-compose
   ↓
Docker Compose project
   ↓
belajar-compose_default
```

Jadi ini normal:

```text
C:\Users\Admin\Desktop\belajar-compose
                         ↑
                    nama project
```

---

Setelah `docker compose ps` menunjukkan `belajar-compose_default`, **jangan hapus apa pun yang berkaitan dengan PostgreSQL/Redis milik DigiAsk**.

Kita akan membuat PostgreSQL latihan sendiri dengan nama:

```text
belajar-compose-postgres
```

dan volume:

```text
belajar-postgres-data
```

Jadi aman terpisah dari PostgreSQL project kamu. 🚀

Ohh, kalau begitu **justru sudah benar** 👍😄

Berarti kita **tidak perlu pindah folder**. Kita akan menggunakan:

```text
C:\Users\Admin\Desktop\belajar-dockerfile
```

sebagai **folder utama pembelajaran Docker kita**.

Dan nama network:

```text
belajar-dockerfile_default
```

juga **sudah sesuai** dengan nama folder/project Compose.

Saya jadikan ini acuan untuk pembelajaran Docker kita ke depannya.

---

## 📚 Struktur belajar kita sekarang

Folder:

```text
belajar-dockerfile
│
├── Dockerfile
├── index.html
├── docker-compose.yml
├── data/
└── catatan pembelajaran Docker
```

Di dalamnya nanti kita bisa punya latihan:

```text
belajar-dockerfile
│
├── Dockerfile
│
├── docker-compose.yml
│
├── index.html
│
├── data/
│
└── notes/
```

Jadi **semua materi praktik Docker kita kumpulkan di satu tempat**. 👍

---

# 🚀 Lanjut: PostgreSQL + Volume + Compose

Sekarang `docker-compose.yml` kita masih:

```yaml
services:

  nginx:
    image: nginx
    container_name: belajar-compose-nginx
    ports:
      - "8083:80"

  redis:
    image: redis:6.0.8
    container_name: belajar-compose-redis
```

Kita akan menambahkan PostgreSQL.

Tapi **PostgreSQL latihan ini jangan menggunakan port 5432**, karena kamu sudah punya container PostgreSQL DigiAsk yang menggunakan port 5432.

Kita pakai:

```text
5433 → PostgreSQL latihan
```

Jadi aman.

Ubah `docker-compose.yml` menjadi:

```yaml
services:

  nginx:
    image: nginx
    container_name: belajar-compose-nginx
    ports:
      - "8083:80"

  redis:
    image: redis:6.0.8
    container_name: belajar-compose-redis

  postgres:
    image: postgres:16-alpine
    container_name: belajar-compose-postgres
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: belajar_db
    ports:
      - "5433:5432"
    volumes:
      - belajar-postgres-data:/var/lib/postgresql/data

volumes:
  belajar-postgres-data:
```

### Perhatikan ini ⭐

```yaml
ports:
  - "5433:5432"
```

Artinya:

```text
Windows              Container
localhost:5433 ────> PostgreSQL:5432
```

Sedangkan:

```yaml
volumes:
  - belajar-postgres-data:/var/lib/postgresql/data
```

artinya:

```text
Docker Volume
belajar-postgres-data
        │
        ▼
PostgreSQL
/data PostgreSQL
```

Jadi kalau container PostgreSQL dihapus, **data database tetap ada di volume**.

---

Setelah file disimpan, jalankan dari folder:

```text
C:\Users\Admin\Desktop\belajar-dockerfile
```

```powershell
docker compose down
```

lalu:

```powershell
docker compose up -d
```

Kemudian:

```powershell
docker compose ps
```

Target kita sekarang:

```text
belajar-compose-nginx       Up
belajar-compose-redis       Up
belajar-compose-postgres    Up
```

dan PostgreSQL:

```text
0.0.0.0:5433->5432/tcp
```

**Kirim hasil `docker compose ps`**, lalu kita akan masuk ke bagian yang sangat menarik: **connect ke PostgreSQL dengan DBeaver dan membuktikan bahwa database benar-benar tersimpan di Volume.** 🐘🐳

Mantappp 🔥 **berhasil 100%**. Sekarang kamu sudah punya mini environment backend sendiri, tanpa mengganggu environment DigiAsk.

```text
Docker Compose
│
├── 🌐 Nginx
│    └── localhost:8083
│
├── 🐘 PostgreSQL
│    └── localhost:5433
│
└── 🔴 Redis
     └── internal Docker network
```

Yang paling penting, PostgreSQL kamu:

```text
belajar-compose-postgres
        │
        ▼
belajar-postgres-data
        │
        ▼
/var/lib/postgresql/data
```

Jadi database-nya sudah menggunakan **named volume**. 👍

---

# 🧪 Sekarang kita buktikan PostgreSQL + Volume

Kita akan membuat database dan tabel sederhana.

### 1. Masuk ke PostgreSQL

Jalankan:

```powershell
docker exec -it belajar-compose-postgres psql -U postgres -d belajar_db
```

Kalau berhasil, prompt akan berubah menjadi:

```text
belajar_db=#
```

Artinya kamu sekarang berada di PostgreSQL **di dalam container**.

---

### 2. Cek database

Jalankan:

```sql
\l
```

Kamu akan melihat:

```text
belajar_db
postgres
template0
template1
```

---

### 3. Buat tabel sederhana

Jalankan:

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100),
    email VARCHAR(100)
);
```

Kalau berhasil:

```text
CREATE TABLE
```

---

### 4. Masukkan data

```sql
INSERT INTO users (name, email)
VALUES
('Firman', 'firman@example.com'),
('Budi', 'budi@example.com');
```

Hasil:

```text
INSERT 0 2
```

---

### 5. Lihat datanya

```sql
SELECT * FROM users;
```

Kira-kira:

```text
 id |  name  |       email
----+--------+-------------------
  1 | Firman | firman@example.com
  2 | Budi   | budi@example.com
```

🎉 Sekarang kita punya database sungguhan di container.

---

# 🔥 Bagian paling penting

Sekarang **jangan hapus volume**.

Kita akan melakukan simulasi:

```text
Container PostgreSQL
       ↓
      ❌ hapus
       ↓
Volume tetap ada
       ↓
Container PostgreSQL baru
       ↓
Data tetap ada
```

Ini akan membuktikan langsung kenapa kita menggunakan:

```yaml
volumes:
  - belajar-postgres-data:/var/lib/postgresql/data
```

---

Untuk sekarang lakukan sampai:

```sql
SELECT * FROM users;
```

lalu kirim hasilnya.

Setelah itu kita lakukan **uji ketahanan data** dengan `docker compose down` → `up` dan kita lihat apakah data `users` masih ada. 🐘🔥

Mantap 🔥 Sekarang kita punya **bukti nyata** bahwa PostgreSQL di Docker sudah menyimpan data.

Data kamu sekarang:

```text
 id |  name  |       email
----+--------+-------------------
  1 | Firman | firman@example.com
  2 | Budi   | budi@example.com
```

Sekarang kita lakukan eksperimen paling penting dari pelajaran **Volume**.

# 🧪 Eksperimen: Apakah data tetap ada?

Saat ini kondisinya:

```text
Docker Compose
      │
      ▼
PostgreSQL Container
      │
      ▼
belajar-postgres-data
      │
      ▼
users table
      │
      ├── Firman
      └── Budi
```

Kita akan **mematikan dan menghapus container PostgreSQL**, tetapi **tidak menghapus volume**.

---

## 1. Keluar dari PostgreSQL

Di prompt:

```text
belajar_db=#
```

jalankan:

```sql
\q
```

Kamu akan kembali ke PowerShell.

---

## 2. Matikan Compose

Pastikan kamu berada di:

```text
C:\Users\Admin\Desktop\belajar-dockerfile
```

Kemudian:

```powershell
docker compose down
```

Perhatikan:

`docker compose down` akan menghapus container yang dibuat Compose, tetapi **volume tidak dihapus secara default**.

Jadi:

```text
Container PostgreSQL ❌
Volume PostgreSQL    ✅
```

---

## 3. Cek volume

Jalankan:

```powershell
docker volume ls
```

Cari:

```text
belajar-postgres-data
```

Harus masih ada. 👍

---

## 4. Jalankan Compose lagi

```powershell
docker compose up -d
```

Tunggu sebentar, kemudian:

```powershell
docker compose ps
```

PostgreSQL harus kembali:

```text
belajar-compose-postgres
```

---

# 🔥 5. Cek data lagi

Masuk kembali:

```powershell
docker exec -it belajar-compose-postgres psql -U postgres -d belajar_db
```

Kemudian:

```sql
SELECT * FROM users;
```

### Kalau Volume bekerja dengan benar...

Data **Firman dan Budi masih ada**:

```text
 id |  name  |       email
----+--------+-------------------
  1 | Firman | firman@example.com
  2 | Budi   | budi@example.com
```

🎯 Inilah konsep utama:

> **Container bisa dihapus, tetapi data tetap hidup karena disimpan di Volume.**

---

## ⚠️ Ada satu perbedaan penting

Nanti kamu akan melihat:

```powershell
docker compose down
```

berbeda dengan:

```powershell
docker compose down -v
```

### `down`

```text
Container ❌
Volume    ✅
Data      ✅
```

### `down -v`

```text
Container ❌
Volume    ❌
Data      ❌
```

Jadi **jangan jalankan `docker compose down -v` dulu**. 😄

Kita justru ingin membuktikan bahwa volume menyelamatkan data.

Lakukan langkah **1–5**, terutama jalankan:

```sql
SELECT * FROM users;
```

setelah container dibuat ulang. Itu akan menjadi demonstrasi Docker Volume yang paling jelas.

🔥🔥 **Nah, ini dia! Berhasil!**

Ini adalah bukti langsung bahwa **Docker Volume bekerja**.

Kamu tadi melakukan:

```text
PostgreSQL Container
       │
       ├── users
       ├── Firman
       └── Budi
              ↓
       docker compose down
              ↓
       Container ❌
              ↓
       docker compose up -d
              ↓
       Container BARU ✅
              ↓
       Volume yang SAMA
              ↓
       Data Firman & Budi ✅
```

Jadi:

> **Container bukan tempat utama penyimpanan data. Volume-lah yang menyimpan data agar tetap bertahan.**

---

# 🧠 Sekarang kita satukan semua yang sudah dipelajari

Kamu sekarang sudah memahami:

### 1. Image

```text
Image
  ↓
Cetakan/template
```

Contoh:

```text
postgres:16-alpine
redis:6.0.8
nginx
```

---

### 2. Container

```text
Image
  ↓
Container
```

Contoh:

```text
belajar-compose-postgres
belajar-compose-redis
belajar-compose-nginx
```

---

### 3. Volume

```text
Container
    │
    ▼
Volume
    │
    ▼
Data
```

Contoh:

```text
belajar-compose-postgres
          │
          ▼
belajar-postgres-data
          │
          ▼
users
```

---

### 4. Network

```text
        belajar-dockerfile_default
                    │
          ┌─────────┼─────────┐
          │         │         │
          ▼         ▼         ▼
       Nginx      Redis    PostgreSQL
```

Mereka bisa saling berkomunikasi menggunakan nama service:

```text
nginx
redis
postgres
```

---

### 5. Docker Compose ⭐

Compose menyatukan semuanya:

```text
                docker-compose.yml
                        │
                        ▼
              docker compose up
                        │
          ┌─────────────┼─────────────┐
          │             │             │
          ▼             ▼             ▼
       Nginx          Redis       PostgreSQL
          │             │             │
          └─────────────┼─────────────┘
                        │
                   Network
                        │
                   PostgreSQL
                        │
                      Volume
                        │
                   Database Data
```

🔥 **Ini sudah mulai menyerupai environment backend sungguhan.**

---

# 🚀 Langkah berikutnya: Docker + Spring Boot

Menurut saya ini bagian yang akan **paling relevan dengan pekerjaan kamu**.

Kita akan membuat:

```text
                 Docker
                   │
         ┌──────────┼──────────┐
         │          │          │
         ▼          ▼          ▼
    Spring Boot  PostgreSQL   Redis
         │          │          │
         └──────────┼──────────┘
                    │
                 Network
```

Dan kita akan belajar sesuatu yang tadi sudah kita singgung:

### Spring Boot → PostgreSQL

Jika keduanya berada dalam Docker Compose:

```properties
spring.datasource.url=jdbc:postgresql://postgres:5432/belajar_db
```

**bukan:**

```properties
spring.datasource.url=jdbc:postgresql://localhost:5433/belajar_db
```

Karena dari dalam Docker:

```text
postgres
```

adalah alamat service PostgreSQL.

Sedangkan dari Windows/DBeaver:

```text
localhost:5433
```

adalah alamat untuk mengakses PostgreSQL dari host.

---

## Sebelum masuk Spring Boot

Ada satu konsep Docker Compose yang sangat penting yang sebaiknya kita pelajari dulu:

**`depends_on` dan environment variables.**

Contohnya nanti:

```yaml
services:

  app:
    ...
    depends_on:
      - postgres
      - redis

  postgres:
    ...
    
  redis:
    ...
```

Ini akan membantu kamu memahami bagaimana aplikasi backend dan database di-start bersama.

Setelah itu baru kita **benar-benar membuat project Spring Boot sederhana di Docker**. 🐳☕

Siap 🚀 Kita lanjut. Sekarang kita belajar **`depends_on` + environment variable**, sebelum masuk ke Spring Boot.

# LEVEL 7.1 — `depends_on`

Bayangkan kita punya:

```text
Spring Boot
    ↓
PostgreSQL
    ↓
Redis
```

Spring Boot membutuhkan PostgreSQL dan Redis.

Di Docker Compose kita bisa menulis:

```yaml
services:

  app:
    depends_on:
      - postgres
      - redis

  postgres:
    image: postgres:16-alpine

  redis:
    image: redis:6.0.8
```

Artinya:

> "Docker, jalankan PostgreSQL dan Redis sebagai dependency sebelum menjalankan `app`."

---

## ⚠️ Tapi ada jebakan kecil

`depends_on` **bukan berarti PostgreSQL sudah siap menerima koneksi**.

Misalnya:

```text
Docker menjalankan
      ↓
PostgreSQL container START
      ↓
PostgreSQL masih initialization...
      ↓
Spring Boot START
      ↓
Spring Boot mencoba koneksi
      ↓
❌ PostgreSQL belum siap
```

Jadi `depends_on` lebih tepat dipahami sebagai:

> **urutan startup**, bukan jaminan service sudah benar-benar ready.

Nanti kita akan belajar `healthcheck` untuk menangani ini.

---

# LEVEL 7.2 — Environment Variable

Sekarang lihat PostgreSQL kita:

```yaml
postgres:
  image: postgres:16-alpine
  environment:
    POSTGRES_USER: postgres
    POSTGRES_PASSWORD: postgres
    POSTGRES_DB: belajar_db
```

Bagian:

```yaml
environment:
```

adalah **environment variable**.

Sederhananya:

```text
Environment Variable
        ↓
informasi/config yang diberikan
ke container
```

PostgreSQL membaca:

```text
POSTGRES_USER
POSTGRES_PASSWORD
POSTGRES_DB
```

lalu menggunakannya ketika pertama kali membuat database.

---

# 🧠 Kenapa environment variable penting?

Bayangkan kita punya:

```text
Development
   ↓
localhost database

Staging
   ↓
staging database

Production
   ↓
production database
```

Kita tidak mau mengubah kode aplikasi setiap kali environment berubah.

Kita bisa menggunakan:

```text
DB_HOST
DB_PORT
DB_USERNAME
DB_PASSWORD
DB_NAME
```

Contohnya:

```text
Development:
DB_HOST=postgres

Production:
DB_HOST=production-db.example.com
```

Kode aplikasi tetap sama.

🔥 Ini konsep yang akan sering kamu temui di Spring Boot.

---

# 🧪 Sekarang praktik

Kita tidak perlu mengubah banyak hal.

Pastikan kamu berada di:

```text
C:\Users\Admin\Desktop\belajar-dockerfile
```

Buka:

```text
docker-compose.yml
```

Sekarang kita ubah menjadi seperti ini:

```yaml
services:

  nginx:
    image: nginx
    container_name: belajar-compose-nginx
    ports:
      - "8083:80"

  redis:
    image: redis:6.0.8
    container_name: belajar-compose-redis

  postgres:
    image: postgres:16-alpine
    container_name: belajar-compose-postgres
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: belajar_db
    ports:
      - "5433:5432"
    volumes:
      - belajar-postgres-data:/var/lib/postgresql/data

volumes:
  belajar-postgres-data:
```

Untuk sekarang **tidak perlu menambahkan `depends_on`** karena belum ada aplikasi yang bergantung kepada PostgreSQL/Redis.

---

# 🔥 Sekarang kita akan melihat environment variable langsung

Jalankan:

```powershell
docker exec belajar-compose-postgres env
```

Output-nya akan cukup panjang.

Cari bagian:

```text
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
POSTGRES_DB=belajar_db
```

Artinya nilai yang kita tulis di:

```yaml
environment:
```

benar-benar masuk ke dalam container.

---

## ⭐ Tapi ada hal penting soal password

Untuk **latihan**, kita menggunakan:

```text
POSTGRES_PASSWORD=postgres
```

Tidak masalah.

Tetapi untuk production:

❌ Jangan:

```yaml
POSTGRES_PASSWORD: postgres
```

Lebih baik menggunakan secret management / environment variable yang aman.

Nanti kita pelajari ketika masuk deployment.

---

# 🎯 Setelah ini

Kita sudah punya:

```text
Docker
 ├── Image
 ├── Container
 ├── Dockerfile
 ├── Volume
 ├── Network
 └── Docker Compose
       │
       ├── Environment
       └── depends_on
```

**Langkah berikutnya akan jauh lebih menarik:**

```text
☕ Spring Boot
       ↓
🐳 Dockerfile
       ↓
Docker Image
       ↓
Container
       ↓
Docker Compose
       ↓
🐘 PostgreSQL
```

Kita akan membuat **aplikasi Spring Boot sederhana yang benar-benar berjalan di Docker dan mengambil data dari PostgreSQL**.

Untuk sekarang jalankan:

```powershell
docker exec belajar-compose-postgres env
```

dan kalau sudah muncul `POSTGRES_USER`, `POSTGRES_DB`, dll., kirim hasilnya.

Mantap 👍 Berarti **environment variable di container PostgreSQL sudah berhasil**.

Yang paling penting dari output kamu:

```text
POSTGRES_DB=belajar_db
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
```

Artinya PostgreSQL container kita dikonfigurasi seperti ini:

| Variable | Nilai | Arti |
|---|---|---|
| `POSTGRES_DB` | `belajar_db` | Nama database |
| `POSTGRES_USER` | `postgres` | Username |
| `POSTGRES_PASSWORD` | `postgres` | Password |

### 🧠 Kenapa environment variable penting?

Bayangkan Docker seperti sebuah komputer kecil.

Kita memberikan informasi konfigurasi kepada komputer itu:

```text
Nama database     → belajar_db
Username          → postgres
Password          → postgres
```

Aplikasi di dalam container bisa membaca informasi tersebut.

Contohnya nanti ketika kita punya **Spring Boot**:

```text
Spring Boot
    ↓
membutuhkan database
    ↓
PostgreSQL
```

Spring Boot perlu tahu:

```text
Database  = belajar_db
Username  = postgres
Password  = postgres
Host      = postgres
Port      = 5432
```

---

## ⚠️ Satu hal penting

Password:

```yaml
POSTGRES_PASSWORD: postgres
```

**aman untuk latihan kita.**

Tapi untuk aplikasi production, jangan menyimpan password asli langsung seperti itu di `docker-compose.yml`.

Nanti kita akan belajar `.env`, Docker secrets, dan cara mengelola credential.

---

# 🚀 Sekarang masuk ke bagian yang sangat penting

Kita akan mulai belajar:

**Docker + Spring Boot + PostgreSQL**

Arsitekturnya nanti seperti ini:

```text
                 Docker Compose
┌──────────────────────────────────────────┐
│                                          │
│   Spring Boot                            │
│   ┌───────────────┐                      │
│   │     app       │                      │
│   │   port 8080   │                      │
│   └───────┬───────┘                      │
│           │                              │
│           │ postgres:5432                │
│           ↓                              │
│   ┌───────────────┐                      │
│   │   postgres    │                      │
│   │   port 5432   │                      │
│   │   belajar_db  │                      │
│   └───────────────┘                      │
│                                          │
└──────────────────────────────────────────┘
```

Ada **satu konsep yang sangat penting** di sini.

### Kalau Spring Boot jalan di Windows

Gunakan:

```text
localhost:5433
```

karena port Docker kita:

```text
5433 → 5432
```

Jadi:

```text
Windows
   ↓
localhost:5433
   ↓
Docker PostgreSQL:5432
```

### Kalau Spring Boot nanti juga jalan di Docker

**Jangan menggunakan `localhost`.**

Gunakan:

```text
postgres:5432
```

Karena `postgres` adalah **nama service Docker Compose**.

Jadi:

```text
Spring Boot container
        ↓
   postgres:5432
        ↓
PostgreSQL container
```

Ini salah satu alasan kenapa tadi kita belajar **Docker Network**. 😄

---

## Langkah berikutnya

Kita akan membuat project Spring Boot **khusus untuk belajar**, jadi **tidak mengganggu project DigiAsk**.

Urutannya:

```text
1. Buat project Spring Boot
          ↓
2. Jalankan Spring Boot di Windows
          ↓
3. Hubungkan Spring Boot → PostgreSQL Docker
          ↓
4. Test API
          ↓
5. Buat Dockerfile
          ↓
6. Masukkan Spring Boot ke Docker
          ↓
7. Docker Compose menjalankan semuanya
```

Dan kita lakukan **pelan-pelan satu tahap dahulu**, bukan langsung semuanya.

**Tahap pertama:** membuat project Spring Boot sederhana.

