# 🐳 LEVEL 1 — Konsep Dasar Docker

Sebelum belajar command seperti `docker run`, `docker ps`, dan `docker compose`, kita harus memahami **5 benda utama**:

```text
Docker
Image
Container
Dockerfile
Registry
```

## 1. Apa itu Docker?

Misalnya aplikasi Spring Boot kamu membutuhkan:

```text
Java 21
Spring Boot
PostgreSQL
Redis
```

Masalahnya:

```text
Laptop saya      → berhasil
Laptop teman     → error
Server           → error
```

> **Docker adalah alat untuk menjalankan aplikasi di dalam lingkungan yang terisolasi yang disebut container.**

# 2. Apa itu Container?

Bayangkan kamu punya **kotak** 📦.

```text
┌──────────────────────┐
│      CONTAINER       │
│                      │
│   Aplikasi           │
│   + kebutuhan        │
│                      │
└──────────────────────┘
```

```text
┌──────────────────────┐
│ PostgreSQL Container │
│                      │
│ PostgreSQL           │
└──────────────────────┘
```

> **Container = tempat aplikasi berjalan.**

# 3. Apa itu Image?

```text
IMAGE = cetakan
CONTAINER = hasil cetakan
```

```text
      CETAKAN
        ↓
      🍪
      🍪
      🍪
```

```text
      IMAGE
        ↓
   ┌────┴────┐
   ↓         ↓
Container  Container
```

```text
Redis Image
     ↓
     ├── Redis Container 1
     ├── Redis Container 2
     └── Redis Container 3
```

> **Image adalah template/cetakan untuk membuat container.**

# 4. Image dan Container jangan tertukar

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

```text
Denah rumah
     ↓
   IMAGE

Rumah yang dibangun
     ↓
 CONTAINER
```

# 5. Contoh nyata

```text
nginx IMAGE
     ↓
nginx CONTAINER
```

```text
Docker Image
     ↓
buat container
     ↓
Container berjalan
     ↓
Nginx berjalan
```

# 6. Apa itu Dockerfile?

**Dockerfile.**

Dockerfile adalah file yang berisi **instruksi untuk membuat Docker Image**.

```text
Dockerfile
    ↓
"Gunakan Java"
"Copy aplikasi"
"Jalankan aplikasi"
    ↓
Docker Image
```

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

# 7. Apa itu Registry?

Gunakan **Registry**.

Registry adalah tempat menyimpan Docker Image.

Contoh paling terkenal:

**Docker Hub**

```text
GitHub
↓
menyimpan source code

Docker Hub
↓
menyimpan Docker Image
```

```text
Docker Hub
│
├── nginx
├── redis
├── postgres
├── mysql
└── dll
```

```text
Docker Hub
     ↓
  nginx image
     ↓
    Laptop
```

# 8. Jadi hubungan semuanya bagaimana?

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

# 9. Contoh dengan Spring Boot

```text
DigiAsk Backend
```

```text
Java
Spring Boot
```

```text
Dockerfile
    ↓
Spring Boot Image
    ↓
Spring Boot Container
    ↓
Spring Boot berjalan
```

```text
PostgreSQL Image
       ↓
PostgreSQL Container
       ↓
Database berjalan
```

```text
Redis Image
       ↓
Redis Container
       ↓
Redis berjalan
```

```text
Docker
│
├── Spring Boot Container
│
├── PostgreSQL Container
│
└── Redis Container
```

# 10. Kenapa tidak satu container saja?

```text
❌ 1 Container
   ├── Spring Boot
   ├── PostgreSQL
   └── Redis
```

```text
✅ Spring Boot Container
        +
   PostgreSQL Container
        +
      Redis Container
```

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

# 🧠 11. Lima istilah yang harus kamu ingat

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

# ⭐ Rumus paling gampang

```text
Dockerfile
    ↓
  IMAGE
    ↓
CONTAINER
    ↓
 APLIKASI
```

```text
Docker Hub
    ↓
  IMAGE
    ↓
CONTAINER
```

# 🎯 Tes kecil sebelum LEVEL 2

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


---

# 🐳 LEVEL 2 — Docker CLI

**CLI** adalah singkatan dari:

> **Command Line Interface**

> **CLI = cara kita menyuruh Docker menggunakan terminal.**

```powershell
docker ps
```

## 1. Sebelum praktik: pastikan Docker hidup

```text
failed to connect to the docker API
dockerDesktopLinuxEngine
```

```powershell
docker --version
```

```text
Docker version 28.x.x, build xxxxx
```

> Docker CLI sudah dikenali oleh Windows.

# 2. Command pertama: `docker ps`

```powershell
docker ps
```

> **lihat container yang sedang berjalan**

```text
CONTAINER ID   IMAGE   COMMAND   CREATED   STATUS   PORTS   NAMES
```

> Docker hidup, tetapi saat ini belum ada container yang berjalan.

# 3. `docker ps -a`

```powershell
docker ps -a
```

```text
docker ps
    ↓
container yang sedang berjalan
```

```text
docker ps -a
    ↓
semua container
```

# 4. Analogi gampang

Bayangkan kamu punya garasi 🚗.

```text
docker ps
```

> "Tampilkan mobil yang **sedang ada di jalan**."

```text
docker ps -a
```

> "Tampilkan **semua mobil**, termasuk yang sedang parkir."

# 5. Command berikutnya: `docker images`

```powershell
docker images
```

> **Image apa saja yang sudah ada di komputer kita.**

```text
REPOSITORY   TAG       IMAGE ID
nginx        latest    xxxxx
redis        7         xxxxx
postgres     16        xxxxx
```

```text
docker ps
```

→ melihat **container**

```text
docker images
```

→ melihat **image**

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

# 🧪 Praktik pertama

```powershell
docker --version
```

```powershell
docker ps
```

```powershell
docker ps -a
```

```powershell
docker images
```

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


---

## 1. `docker --version` ✅

```text
Docker version 29.7.2
```

> Docker CLI sudah terinstall dan bisa digunakan.

## 2. `docker ps` ✅

Kamu punya **2 container yang sedang berjalan**:

```text
postgres
redis
```

```text
IMAGE
postgres:16-alpine
redis:6.0.8
```

```text
postgres:16-alpine
        ↓
    PostgreSQL
        ↓
   postgres container
```

```text
redis:6.0.8
        ↓
      Redis
        ↓
   redis container
```

> **Image → Container**

# 3. Mari baca `docker ps`

```text
CONTAINER ID   IMAGE                STATUS       PORTS       NAMES
976323b33abd   postgres:16-alpine   Up 2 hours   ...         postgres
n454c3c2085b   redis:6.0.8          Up 2 hours   ...         redis
```

### `CONTAINER ID`

```text
976323b33abd
```

Ini adalah **ID unik container**.

### `IMAGE`

```text
postgres:16-alpine
```

Ini adalah **image yang digunakan untuk membuat container tersebut**.

```text
postgres:16-alpine
       ↓
   container
       ↓
    postgres
```

### `STATUS`

```text
Up 2 hours
```

Artinya container tersebut **sedang berjalan**.

```text
Exited
```

artinya container tersebut **sudah berhenti**.

### `PORTS`

```text
0.0.0.0:5432->5432/tcp
```

```text
Windows : 5432
    ↓
Docker PostgreSQL : 5432
```

```text
0.0.0.0:6379->6379/tcp
```

```text
Windows : 6379
    ↓
Docker Redis : 6379
```

### `NAMES`

```text
postgres
redis
```

```powershell
docker logs postgres
```

```powershell
docker logs redis
```

# 4. Sekarang lihat `docker ps -a`

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

```text
postgres
redis
```

```text
Up
```

```text
Exited
```

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

# 5. `Exited (137)` itu apa?

```text
kafka
Exited (137)
```

> `Exited` = container berhenti.

```powershell
docker logs
```

# 6. Sekarang `docker images`

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

```text
IMAGE
  ↓
membuat
  ↓
CONTAINER
```

```text
postgres:16-alpine
        ↓
     postgres
```

```text
redis:6.0.8
        ↓
       redis
```


---

# 7. Ada hal menarik di sini

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

```text
Docker
│
├── PostgreSQL 12
├── PostgreSQL 15
├── PostgreSQL 16
└── PostgreSQL 17
```

```text
postgres:16-alpine
        ↓
postgres container
```

# 🧠 Sekarang ada satu konsep penting

**1 Image → bisa banyak Container.**

```text
redis:6.0.8
```

```text
redis:6.0.8
      │
      ├── redis-dev
      ├── redis-test
      └── redis-staging
```

```text
IMAGE
  │
  ├── CONTAINER
  ├── CONTAINER
  └── CONTAINER
```

# 🎯 Jangan hapus apa pun dulu

**jangan menjalankan**:

```powershell
docker system prune
```

```powershell
docker image prune
```

# 🧪 Praktik berikutnya

Kita akan mengambil **Nginx Image** dan membuat container baru.

```powershell
docker pull nginx
```

> "Docker, ambil Image `nginx` dari Docker Registry."

```text
Docker Hub
    ↓
nginx Image
    ↓
Laptop kamu
```

```powershell
docker images
```

**Belum perlu `docker run` dulu.**

---

Mantap 👍 Berarti **`docker pull nginx` berhasil**.

## 🧠 Apa yang baru saja terjadi?

```text
nginx:latest
```

```powershell
docker pull nginx
```

```text
nginx:latest
05b8cb60c354
253MB
```

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

## 🔥 Ini perbedaan penting

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

```text
CONTAINERS
│
├── postgres           ← berjalan
├── redis              ← berjalan
├── kafka              ← berhenti
├── elasticsearch      ← berhenti
└── dll
```

```text
nginx:latest
```

**ada di IMAGE**

```text
nginx container
```

# 🧪 Sekarang kita buat Container Nginx

```powershell
docker run -d --name belajar-nginx -p 8080:80 nginx
```

### `docker run`

> **Buat container dari sebuah image dan jalankan.**

### `-d`

> Jalankan di background.

### `--name belajar-nginx`

```text
belajar-nginx
```

```text
sweet_hodgkin
```

### `-p 8080:80`

Ini **port mapping**.

```text
Komputer kamu        Container
    8080       →        80
```

```text
localhost:8080
       ↓
Nginx container :80
```

### `nginx`

Ini adalah **image yang digunakan**.

```text
nginx IMAGE
     ↓
docker run
     ↓
belajar-nginx CONTAINER
     ↓
Nginx berjalan
```

## 🚀 Sekarang jalankan

```powershell
docker run -d --name belajar-nginx -p 8080:80 nginx
```

```powershell
docker ps
```

**Belum perlu menjalankan command lain.**


---

Mantap 👍 **berhasil.** Docker Nginx kamu sekarang sudah berjalan.

```text
8bd65efcc5cc   nginx   ...   Up 5 seconds   0.0.0.0:8080->80/tcp   belajar-nginx
```

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

**http://localhost:8080**

> Welcome to nginx!

berarti kita sudah berhasil membuat **container pertama dari image Nginx** 🎉

```powershell
docker stop belajar-nginx
```

```powershell
docker ps
```

```powershell
docker ps -a
```

```text
docker ps       → container yang sedang hidup
docker ps -a    → semua container
```

**Jangan hapus container PostgreSQL dan Redis kamu.**

---

## Langkah berikutnya: Stop → Start lagi

Sekarang kita belajar perbedaan **stop** dan **start**.

### 1. Stop container

```powershell
docker stop belajar-nginx
```

```powershell
docker ps
```

### 2. Cek semua container

```powershell
docker ps -a
```

```text
belajar-nginx    nginx    Exited
postgres         postgres:16-alpine    Up
redis            redis:6.0.8            Up
```

```text
docker ps
    ↓
yang sedang hidup

docker ps -a
    ↓
semua container
```

### 3. Hidupkan kembali container

```powershell
docker start belajar-nginx
```

```powershell
docker ps
```

```text
http://localhost:8080
```

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

---

Sip 👍 Kita lanjut **step berikutnya: hapus container Nginx**.

```text
Container → hapus dulu
Image     → baru hapus
```

### 1️⃣ Pastikan Nginx hidup

```powershell
docker ps
```

```text
belajar-nginx   nginx   Up ...
```

### 2️⃣ Stop Nginx

```powershell
docker stop belajar-nginx
```

### 3️⃣ Hapus container

```powershell
docker rm belajar-nginx
```

```powershell
docker ps -a
```

### 🧠 Bedanya `stop` dan `rm`

```text
stop
↓
Rumahnya masih ada
Orangnya keluar

rm
↓
Rumahnya dibongkar
```

```text
docker stop belajar-nginx
        ↓
Container mati

docker rm belajar-nginx
        ↓
Container dihapus
```

**Jangan jalankan `docker rm postgres` atau `docker rm redis`**


---

Mantap 👍 **`belajar-nginx` sudah benar-benar terhapus.**

```text
belajar-nginx
```

## 1️⃣ Cek image Nginx

```powershell
docker images
```

```text
nginx    latest    ...
```

```text
IMAGE
  ↓
docker run
  ↓
CONTAINER
```

```powershell
docker rm belajar-nginx
```

```text
❌ Container belajar-nginx
```

```text
✅ Image nginx
```

## 2️⃣ Hapus image Nginx

```powershell
docker images
```

```text
Container nginx → sudah dihapus ✅
          ↓
Image nginx → kita hapus ⏭️
```

---

Image `nginx:latest` **tidak sedang dipakai** karena kolom `U In Use` tidak muncul di baris Nginx.

### 1️⃣ Hapus image Nginx

```powershell
docker rmi nginx:latest
```

```text
Untagged: nginx:latest
Deleted: sha256:...
```

### 2️⃣ Cek lagi

```powershell
docker images
```

### 🧠 Sekarang bedakan 4 perintah ini

| Perintah | Fungsi |
|---|---|
| `docker run` | Buat + jalankan container |
| `docker stop` | Matikan container |
| `docker rm` | Hapus container |
| `docker rmi` | Hapus image |

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

⚠️ **Jangan gunakan `docker system prune` dulu**

```powershell
docker rmi nginx:latest
```

---

Mantap 👍 **berhasil.**

Sekarang `nginx:latest` sudah **tidak ada** di daftar `docker images`.

```text
nginx IMAGE       ❌ sudah dihapus
belajar-nginx     ❌ sudah dihapus
PostgreSQL        ✅ aman
Redis             ✅ aman
Kafka             ✅ aman
Elasticsearch     ✅ aman
```

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

```powershell
mkdir belajar-dockerfile
cd belajar-dockerfile
```

```powershell
pwd
```

---

Perfect 👍 Posisi kita sudah benar:

```text
C:\Users\Admin\Desktop\belajar-dockerfile
```

Sekarang kita mulai **Dockerfile pertama**.

## 1. Buat file bernama `Dockerfile`

```powershell
New-Item Dockerfile
```

```powershell
dir
```

```text
Dockerfile
```

### 2. Isi Dockerfile

```powershell
code .
```

```dockerfile
FROM nginx
```

> "Saya mau membuat image baru berdasarkan image `nginx`."

```text
Dockerfile
    │
    │ FROM nginx
    ▼
Image nginx
```

### 3. Jangan `build` dulu

```dockerfile
FROM nginx
```

```powershell
docker build ...
```

---

Betul 👍 **Dockerfile pertama kamu sudah benar.**

```dockerfile
FROM nginx
```

## 1. Build image

```text
C:\Users\Admin\Desktop\belajar-dockerfile
```

```powershell
docker build -t belajar-nginx .
```

```text
-t belajar-nginx
```

```text
belajar-nginx
```

```text
.
```

> Docker gunakan folder yang sedang saya buka sekarang sebagai **build context**.

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

```powershell
docker images
```

```text
belajar-nginx
```

⚠️ **Jangan jalankan `docker run` dulu.**

```powershell
docker build -t belajar-nginx .
```

---

Mantap! 🎉 **Berhasil.** Sekarang kamu sudah membuat **image Docker sendiri** bernama `belajar-nginx`.

```text
belajar-nginx:latest    08c5d7841346    250MB
```

## 🧠 Apa yang sebenarnya terjadi?

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

```dockerfile
FROM nginx
```

```text
belajar-nginx
```

```text
belajar-nginx:latest
```

## 🔥 Sekarang kita buktikan image ini bisa membuat container

```powershell
docker run -d --name belajar-nginx-2 -p 8080:80 belajar-nginx
```

```powershell
docker run ... nginx
```

```powershell
docker run ... belajar-nginx
```

```powershell
docker ps
```

```text
http://localhost:8080
```

Harus muncul halaman Nginx lagi. 😎

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

**mengubah isi Docker image dengan `COPY`**

---

```text
Dockerfile
   ↓
docker build
   ↓
Image: belajar-nginx
```

```text
Image
  ↓
Container
```

### 🚀 Step berikutnya

```powershell
docker run -d --name belajar-nginx-2 -p 8080:80 belajar-nginx
```

```powershell
docker ps
```

```text
belajar-nginx-2    belajar-nginx    Up ...
```

berarti **image buatan kamu berhasil menjadi container**. 🎉

```text
http://localhost:8080
```

membuat **website HTML kita sendiri di dalam Docker** 🔥

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

Di folder `C:\Users\Admin\Desktop\belajar-dockerfile`, buat file `index.html`:

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

## 2. Ubah Dockerfile

```dockerfile
FROM nginx

COPY index.html /usr/share/nginx/html/index.html
```

### 🧠 Apa arti `COPY`?

> Ambil `index.html` dari komputer saya → masukkan ke dalam image Docker → letakkan di lokasi HTML Nginx.

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

⚠️ Untuk sekarang jangan build dulu: cukup buat `index.html` dan ubah Dockerfile.

---

Perfect! ✅ Dockerfile kamu **sudah benar**.

Sekarang kita masuk ke konsep penting: **Build ulang image**.

Saat ini container `belajar-nginx-2` masih menggunakan **image versi lama**. Jadi meskipun Dockerfile sudah berubah, container yang sedang berjalan **belum otomatis berubah**.

### 1️⃣ Build image baru

```powershell
docker build -t belajar-nginx:v2 .
```

`v2` adalah **tag/versi** image. Sebelumnya `belajar-nginx:latest`, sekarang `belajar-nginx:v2`.

### 2️⃣ Cek image

```powershell
docker images
```

Kamu kemungkinan akan melihat:

```text
belajar-nginx    v2       ...
belajar-nginx    latest   ...
```

Artinya kita sekarang punya **dua versi image**.

### ⚠️ Jangan hapus container lama dulu

Container `belajar-nginx-2` masih berjalan menggunakan `belajar-nginx:latest`.

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

---

Mantap 👍 `belajar-nginx:v2` **sudah berhasil dibuat**.

```text
belajar-nginx:latest   08c5d7841346   250MB   66.3MB   U
belajar-nginx:v2       45f45a79fc34   250MB   66.3MB
```

Artinya sekarang ada **2 versi image**:

- `belajar-nginx:latest` → versi lama
- `belajar-nginx:v2` → versi baru yang sudah berisi `index.html` kamu
- `U` = **Used / sedang digunakan oleh container**

Ukuran keduanya sama-sama 250 MB. **Tidak masalah.** Docker menggunakan layer sehingga ukuran yang terlihat bukan berarti benar-benar menggandakan seluruh data di disk.

### Sekarang kita test `v2`

Karena container lama `belajar-nginx-2` masih menggunakan port `8080`, kita bisa menjalankan v2 di port **8081** supaya tidak mengganggu container lama.

```powershell
docker run -d --name belajar-nginx-v2 -p 8081:80 belajar-nginx:v2
```

Lalu:

```powershell
docker ps
```

Kemudian buka browser: **http://localhost:8081**

Harus muncul:

> **Halo Docker! 🚀**  
> Ini website pertama saya yang berjalan di Docker.

### 🧠 Pelajaran penting

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

> Mengubah `Dockerfile` **tidak mengubah container yang sudah berjalan**.

Makanya kita membuat **image v2 baru**, kemudian membuat **container baru** dari image v2.

---

Mantap! 🎉 Itu berarti **container `belajar-nginx-v2` berhasil dibuat dan dijalankan**.

Sekarang cek:

```powershell
docker ps
```

Kamu seharusnya melihat:

```text
belajar-nginx-v2
0.0.0.0:8081->80/tcp
```

### 🌐 Buka browser

```text
http://localhost:8081
```

Kalau benar, harus muncul:

> **Halo Docker! 🚀**  
> Ini website pertama saya yang berjalan di Docker.

### 🧠 Perhatikan perbedaannya

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

## 🐳 Level berikutnya: Dockerfile lebih dalam

Dockerfile kamu sekarang:

```dockerfile
FROM nginx

COPY index.html /usr/share/nginx/html/index.html
```

Kita tambahkan `EXPOSE 80`:

```dockerfile
FROM nginx

COPY index.html /usr/share/nginx/html/index.html

EXPOSE 80
```

### Apa itu `EXPOSE 80`?

> `EXPOSE 80` = memberi tahu bahwa aplikasi di dalam container menggunakan **port 80**.

Tapi ingat ⚠️: `EXPOSE 80` **tidak otomatis membuat port bisa diakses dari Windows.**

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

## ✏️ Praktik

Ubah Dockerfile menjadi seperti di atas, simpan, lalu build lagi:

```powershell
docker build -t belajar-nginx:v3 .
```

Setelah selesai, **jangan hapus container yang sekarang dulu**. Nanti kita lanjut ke **`CMD`**, lalu setelah itu baru masuk ke **Volume**. 🐳🚀

---

Mantap 👍 **`belajar-nginx:v3` berhasil dibuat.**

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

Lebih mudah melihatnya dengan:

```powershell
docker inspect belajar-nginx:v3 --format='{{json .Config.Cmd}}'
```

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

Betul 👍 Hasilnya:

```text
["nginx","-g","daemon off;"]
```

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

## 🔥 Sekarang kita coba eksperimen

Jalankan image `v3`, tetapi **mengganti CMD sementara**:

```powershell
docker run --name belajar-cmd nginx echo "Halo dari Docker"
```

```text
nginx              → image
echo "Halo..."     → ganti CMD bawaan
```

Proses `echo` selesai → container juga akan **langsung berhenti**. Cek:

```powershell
docker ps -a
```

Status `belajar-cmd`:

```text
Exited (0)
```

### Kenapa `Exited (0)`?

> Program selesai **dengan sukses**.

Jadi **container berhenti bukan berarti error**.

Berbeda dengan `Exited (1)` dan `Exited (137)` yang biasanya menunjukkan ada masalah/termination tertentu.

---

Nah, ini **justru hasil praktik yang bagus** 👍 Kamu sudah melihat sendiri bagaimana `CMD` bekerja.

### 1. Percobaan pertama berhasil ✅

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

### 2. Kenapa percobaan kedua error? ❌

Karena container bernama `belajar-cmd` **sudah dibuat**. Walaupun statusnya `Exited`, namanya masih terdaftar.

```text
Conflict. The container name "/belajar-cmd" is already in use
```

> "Nama `belajar-cmd` sudah dipakai."

## 🧹 Sekarang hapus container latihan ini

```powershell
docker rm belajar-cmd
```

Lalu cek `docker ps -a` — `belajar-cmd` seharusnya sudah hilang.

**Jangan hapus container lain** seperti `postgres`, `redis`, `belajar-nginx-2`, `belajar-nginx-v2`, karena kita masih menggunakannya untuk latihan/project.

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

---

Okeee 😄 Kita lanjut pelan-pelan.

## 🐳 Belajar `docker logs`

`docker logs` sangat sederhana:

> **Untuk melihat apa yang dikeluarkan oleh aplikasi di dalam container.**

```powershell
docker logs nama-container
```

### Kita praktik

```powershell
docker logs belajar-nginx-v2
```

Kemungkinan output-nya **kosong atau sangat sedikit**. Itu normal, karena Nginx belum menerima request yang menghasilkan log ke stdout/stderr.

### Kita buat contoh yang lebih jelas

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

Tetapi walaupun container sudah `Exited`, log-nya **masih bisa dilihat** (`belajar-logs    alpine    Exited (0)`).

### 🔥 Ini penting untuk kerja kamu

Nanti kalau Spring Boot kamu misalnya `Exited (1)`, jangan langsung panik 😄 — jalankan `docker logs nama-container` untuk mencari tahu **kenapa aplikasi mati**.

---

Nah, **berhasil** 👍 Bahkan ini menunjukkan kamu sudah memahami pola Docker dengan benar.

### 1. `docker run` pertama sebenarnya sudah berhasil

Container `belajar-logs` sudah ada dengan status `Exited (0)`. Jadi ketika kamu menjalankan lagi, Docker menolak karena **nama `belajar-logs` sudah digunakan** — ini sama persis seperti kasus `belajar-cmd` sebelumnya.

### 2. `docker logs` berhasil 🎯

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

> **Container sudah `Exited`, tetapi log-nya masih bisa dibaca.**

---

# 🔥 Sekarang kita belajar `docker logs -f`

`-f` = **follow**

Artinya:

> "Tampilkan log dan terus pantau log baru yang masuk."

Ini sering digunakan untuk melihat aplikasi **secara real-time**.

```powershell
docker logs -f belajar-nginx-v2
```

## Coba sekarang

Buka browser `http://localhost:8081` — setiap browser mengakses Nginx, kemungkinan akan muncul log request seperti:

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

Tekan `Ctrl + C`.

**Jangan khawatir:** `Ctrl + C` di sini hanya menghentikan tampilan `docker logs -f`, **bukan menghentikan container Nginx**.

Setelah `docker logs` selesai, **kita masuk ke bagian yang jauh lebih penting: Docker Volume** 💾 — ini yang menjelaskan bagaimana data PostgreSQL/Redis bisa tetap ada walaupun container dihapus.

---


# 🐳 LEVEL 5 — Docker Volume 💾

Bayangkan container seperti **rumah sementara**.

Kalau container dihapus:

```text
docker rm container
        ↓
🏠 Rumah dihancurkan
        ↓
data di dalamnya bisa ikut hilang
```

Nah, **Volume** adalah tempat penyimpanan yang dibuat supaya data bisa tetap hidup **di luar lifecycle container**.

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

```powershell
docker volume create belajar-volume
```

Hasilnya:

```text
belajar-volume
```

```powershell
docker volume ls
```

```text
DRIVER    VOLUME NAME
local     belajar-volume
```

# 🧠 Apa yang baru saja terjadi?

Perintah:

```powershell
docker volume create belajar-volume
```

artinya:

> "Docker, tolong buatkan tempat penyimpanan bernama `belajar-volume`."

Belum ada container yang menggunakan volume tersebut.

```text
Docker
  │
  └── Volume
       └── belajar-volume 💾
```

---

## Step 2 — Kita buat container Alpine

```powershell
docker run -it --name belajar-volume-1 -v belajar-volume:/data alpine sh
```

### `-it`

Membuat kita bisa masuk ke terminal container.

### `-v`

Artinya **mount volume**.

```text
-v belajar-volume:/data
```

berarti:

```text
Volume Docker          Container
belajar-volume  ───→   /data
```

Jadi folder `/data` di dalam container sebenarnya terhubung dengan `belajar-volume`.

---

# 🧪 Setelah masuk container

Kemungkinan prompt berubah menjadi seperti:

```text
/ #
```

```sh
echo "Data saya aman di volume Docker" > /data/data.txt
```

```sh
cat /data/data.txt
```

Harus muncul:

```text
Data saya aman di volume Docker
```

🎯 **Jangan keluar dulu dengan `exit`.**

---


# 🧪 Eksperimen Volume

> **Container pertama kita hapus → buat container baru → cek apakah `data.txt` masih ada.**

## STEP 1 — Keluar dari container

```text
/ #
```

```sh
exit
```

Kamu akan kembali ke PowerShell:

```text
PS C:\Users\Admin\Desktop\belajar-dockerfile>
```

---

## STEP 2 — Lihat container

```powershell
docker ps -a
```

Kemungkinan statusnya:

```text
Exited (0)
```

Kenapa `Exited`? Karena tadi kita menjalankan `alpine sh` dan kita sudah keluar dari shell dengan `exit`.

**Container berhenti, tapi volume masih ada.**

---

# STEP 3 — Hapus CONTAINER

```powershell
docker rm belajar-volume-1
```

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

```powershell
docker run -it --name belajar-volume-2 -v belajar-volume:/data alpine sh
```

Kita menggunakan **volume yang sama** `belajar-volume`. Bukan membuat volume baru.

---

# STEP 5 — Cek isi `/data`

```text
/ #
```

```sh
ls /data
```

🎯 Seharusnya kamu melihat:

```text
data.txt
```

```sh
cat /data/data.txt
```

Hasilnya:

```text
Data saya aman di volume Docker
```

🔥🔥🔥

**Nah! Ini momen penting.** Kita baru saja membuktikan:

```text
CONTAINER LAMA
     │
     ├── data.txt
     │
     ▼
belajar-volume 💾
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

### 1. Image — `alpine` — Adalah **cetakan/template**.

### 2. Container — `belajar-volume-1` / `belajar-volume-2` — Adalah **instance yang menjalankan image**.

### 3. Volume — `belajar-volume` — Adalah **tempat menyimpan data yang ingin dipertahankan**.

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
```

Container PostgreSQL bisa dihapus:

```text
PostgreSQL Container ❌
```

tetapi:

```text
postgres_data 💾 ✅
```

tetap ada. Kemudian container PostgreSQL baru dibuat:

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

**Jangan menjalankan `docker volume rm postgres_data` atau menghapus volume PostgreSQL project kamu.**

```sh
ls /data
```

```sh
cat /data/data.txt
```

Kalau muncul `data.txt` dan `Data saya aman di volume Docker`, berarti eksperimen **Volume Persistence berhasil 100%**. 🎯

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

Output JSON berisi: `Name`, `Driver` (`local` = volume dikelola Docker di mesin lokal), `Mountpoint` (**lokasi penyimpanan yang digunakan Docker untuk volume tersebut**), `Scope`, dll.

⚠️ Di Docker Desktop + Windows + WSL2, path Mountpoint bisa terlihat berbeda dari folder Windows biasa. **Jangan edit isi database Docker secara manual dari Mountpoint**, terutama untuk PostgreSQL.

## Bagian 2 — Bagaimana Docker melakukan Mount?

```powershell
docker run -it `
  --name belajar-volume-3 `
  -v belajar-volume:/data `
  alpine sh
```

```text
┌───────────────────────────────┐
│ Docker Volume                 │
│ belajar-volume                │
│ data.txt                      │
└───────────────┬───────────────┘
                │ mount
                ▼
┌───────────────────────────────┐
│ Container                     │
│ /data                         │
│   └── data.txt                │
└───────────────────────────────┘
```

- `/data` **bukan volume** — itu **mount point di dalam container**.
- `belajar-volume` = **Docker Volume**.

## Bagian 3 — Named Volume vs Bind Mount

### A. Named Volume

```powershell
-v belajar-volume:/data
```

```text
Docker mengelola storage
        ↓
belajar-volume
        ↓
/data di container
```

Cocok untuk: PostgreSQL, MySQL, Redis, data aplikasi, production.

### B. Bind Mount

Folder Windows langsung dihubungkan ke container:

```powershell
docker run -it `
  --name belajar-bind `
  -v "${HOME}\Desktop\belajar-dockerfile\data:/data" `
  alpine sh
```

```text
Windows
C:\Users\Admin\Desktop\belajar-dockerfile\data
                │
                │ bind mount
                ▼
Container
/data
```

`echo "Halo dari Windows" > /data/test.txt` di dalam container → file juga terlihat di folder Windows. 🔥

## 🆚 Named Volume vs Bind Mount

| | Named Volume | Bind Mount |
|---|---|---|
| Contoh | `belajar-volume:/data` | `./data:/data` |
| Dikelola | Docker | Kita |
| Lokasi | Docker menentukan | Kita menentukan |
| Cocok database | ⭐⭐⭐⭐⭐ | ⭐⭐ |
| Cocok source code | ⭐⭐ | ⭐⭐⭐⭐⭐ |

## 🎯 Contoh dunia kerja

- PostgreSQL: `volumes: - postgres_data:/var/lib/postgresql/data`
- Source code Spring Boot: `volumes: - ./src:/app/src`

```text
                  Docker
                    │
        ┌───────────┴───────────┐
        ▼                       ▼
 postgres_data              ./src
 Named Volume              Bind Mount
        │                       │
        ▼                       ▼
 PostgreSQL                Spring Boot
```

## ⚠️ Satu hal penting

`docker volume rm belajar-volume` → data bisa hilang.

Backup mekanisme terpisah: PostgreSQL → `pg_dump` → `backup.sql` → disimpan di tempat lain.

```text
PostgreSQL
     │
     ├── Volume → persistence
     └── Backup → disaster recovery
```

## 🔥 Praktik kecil

```powershell
docker volume inspect belajar-volume
```

Selanjutnya: **Bind Mount pertama**, lalu **Docker Network** — bagaimana Spring Boot, PostgreSQL, Redis, Kafka bisa saling berbicara **tanpa `localhost` di antara container**. 🚀


---

# 🐳 LEVEL 28 — Bedah Detail Volume & Praktik Bind Mount Pertama

Hasilnya **sudah benar**. Sekarang kita bedah pelan-pelan output `docker volume inspect`.

## 1. `Name`

```text
"Name": "belajar-volume"
```

Ini nama Volume yang kita buat dengan `docker volume create belajar-volume`.

---

## 2. `Driver`

```text
"Driver": "local"
```

Artinya volume menggunakan **local storage**, penyimpanan yang dikelola oleh Docker di komputer kamu. Untuk belajar dan kebanyakan kasus Docker biasa, `local` ini yang paling umum.

---

## 3. `Mountpoint` ⭐

```text
"Mountpoint": "/var/lib/docker/volumes/belajar-volume/_data"
```

Ini adalah **lokasi penyimpanan volume di dalam lingkungan Docker**.

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

Artinya volume ini hanya berada pada **Docker environment lokal** kamu — bukan otomatis tersimpan di cloud atau server lain.

---

## 5. `CreatedAt`

```text
"CreatedAt": "2026-09-15T02:24:42Z"
```

Ini waktu volume tersebut dibuat. `Z` menunjukkan waktu UTC.

---

## Jadi kesimpulannya 🧠

> **Volume = tempat menyimpan data yang bisa tetap ada walaupun Container dihapus.**

```text
Container 1
    │
    └── belajar-volume
            │
            └── data.txt
```

Container 1 dihapus → volume tetap → container baru pakai volume yang sama:

```text
Container 1 ❌
belajar-volume ✅
      │
      └── data.txt

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

