========================================
CATATAN DOCKER — RINGKAS SEKALI
(disingkat dari catatan-ringkas.txt)
========================================

## KONSEP DASAR

- Docker Desktop harus hidup dulu sebelum pakai perintah Docker.
- Docker = aplikasi untuk menjalankan program di dalam container, supaya "di laptop jalan, di mana pun juga jalan".
- Image = templat/rancangan aplikasi (contoh: nginx, postgres).
- Container = aplikasi yang berjalan, dibuat dari image. 1 image → bisa banyak container.
- Dockerfile = resep untuk membuat image sendiri.
- Registry (Docker Hub) = tempat upload/download image.
- Container ≠ Virtual Machine: container jauh lebih ringan.

## ALUR UTAMA

Dockerfile → docker build → IMAGE → docker run → CONTAINER → aplikasi hidup

## COMMAND PENTING

| Perintah | Fungsi |
|---|---|
| `docker --version` | cek Docker terinstall |
| `docker ps` | container yang sedang hidup (Up) |
| `docker ps -a` | semua container, termasuk Exited |
| `docker images` | daftar image |
| `docker pull nginx` | download image dari Docker Hub |
| `docker run -d --name web -p 8080:80 nginx` | buat + jalankan container |
| `docker stop <nama>` | matikan container |
| `docker start <nama>` | nyalakan kembali |
| `docker rm <nama>` | hapus container |
| `docker rmi <image>` | hapus image |
| `docker build -t <nama:tag> .` | build image dari Dockerfile |
| `docker logs <nama>` | lihat log container |
| `docker logs -f <nama>` | log real-time (follow) |
| `docker inspect <image> --format='{{json .Config.Cmd}}'` | lihat CMD bawaan image |

- `-d` = berjalan di background, `-p 8080:80` = port komputer → port container, `--name` = nama container.
- Hapus: container dulu (`docker rm`), baru image (`docker rmi`).

## PRAKTIK NGINX

```powershell
docker run -d --name belajar-nginx-2 -p 8080:80 belajar-nginx
```

Buka `http://localhost:8080` → Welcome to nginx!

- `Up 3 hours` = sedang berjalan; `8080->80` = port komputer → port Nginx di container.
- Versi lama & baru bisa jalan bersamaan: `8080` → versi lama, `8081` → versi baru 🚀

## DOCKERFILE

```dockerfile
FROM nginx

COPY index.html /usr/share/nginx/html/index.html

EXPOSE 80
```

- `COPY` = "Ambil `index.html` dari komputer saya → masukkan ke dalam image Docker → letakkan di lokasi HTML Nginx."
- `EXPOSE 80` = "memberi tahu bahwa aplikasi di dalam container menggunakan **port 80**", tapi **tidak otomatis membuat port bisa diakses dari Windows** — tetap perlu `-p 8081:80` saat `docker run`.

```text
Windows                  Container
8081       ───────────→    80
                 -p
```

## VERSI / TAG IMAGE

```powershell
docker build -t belajar-nginx:v2 .
```

- `v2` = **tag/versi** image. `latest` = versi lama.
- `U` = **Used / sedang digunakan oleh container**.
- Ukuran image sama 250 MB tidak masalah — Docker memakai layer.
- > **Image yang sudah dibuat tidak otomatis berubah ketika Dockerfile berubah.**
- > Mengubah `Dockerfile` **tidak mengubah container yang sudah berjalan**.
- Urutannya: Dockerfile berubah → docker build → Image baru → docker run → Container baru.

## CMD

- `CMD` = **Perintah default yang dijalankan ketika container dibuat dari image.**
- CMD bawaan nginx: `["/docker-entrypoint.sh","nginx","-g","daemon off;"]`
- `daemon off` = "jalankan Nginx dan jangan keluar dari proses utama" — Docker membutuhkan **proses utama yang tetap hidup**.
- Alur: Image → docker run → CMD dijalankan → Aplikasi hidup. Proses utama selesai → container STOP.
- Ganti CMD sementara: `docker run --name belajar-cmd nginx echo "Halo dari Docker"` → echo selesai → container langsung berhenti.
- `Exited (0)` = program selesai **dengan sukses** (bukan error). `Exited (1)` / `Exited (137)` = ada masalah/termination.

## LOGS

- `docker logs <nama>` = "Untuk melihat apa yang dikeluarkan oleh aplikasi di dalam container."
- Log tetap bisa dibaca walaupun container sudah `Exited`.
- `docker logs -f` = follow: "Tampilkan log dan terus pantau log baru yang masuk." (real-time)
- `Ctrl + C` hanya menghentikan tampilan `logs -f`, **bukan menghentikan container**.

## KESALAHAN UMUM

- `Conflict. The container name "/x" is already in use` → nama masih terdaftar walau Exited → `docker rm` dulu.
- Jangan asal hapus container project (postgres, redis, dll) — masih digunakan.


## VOLUME

- Container = **rumah sementara** — kalau container dihapus, data di dalamnya bisa ikut hilang.
- **Volume** = tempat penyimpanan yang dibuat supaya data bisa tetap hidup **di luar lifecycle container**. Container ❌ → Volume ✅ → Data masih ada.
- Buat volume: `docker volume create belajar-volume` = "Docker, tolong buatkan tempat penyimpanan bernama `belajar-volume`."
- Lihat volume: `docker volume ls` (`DRIVER VOLUME NAME` → `local belajar-volume`)
- Mount ke container: `docker run -it --name belajar-volume-1 -v belajar-volume:/data alpine sh`
  - `-it` = bisa masuk ke terminal container; `-v` = **mount volume** (`belajar-volume ───→ /data`)
- Eksperimen: `echo "Data saya aman di volume Docker" > /data/data.txt` → `exit` → `docker rm belajar-volume-1` (container ❌, volume ✅) → `docker run -it --name belajar-volume-2 -v belajar-volume:/data alpine sh` (volume yang sama) → `ls /data` → `data.txt` masih ada ✅
- Beda 3 hal: Image (`alpine`) = **cetakan/template**; Container (`belajar-volume-1/2`) = **instance yang menjalankan image**; Volume (`belajar-volume`) = **tempat menyimpan data yang ingin dipertahankan**.
- PostgreSQL: `postgres_data 💾` tetap ada walau container dihapus; container baru mount volume yang sama → database lama kembali. **Jangan menjalankan `docker volume rm postgres_data`.**

## KONSEP INTI

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

## VOLUME: INSPECT & NAMED VS BIND MOUNT

- `docker volume inspect belajar-volume` → detail volume: `Name`, `Driver` (`local`), `Mountpoint` (lokasi penyimpanan Docker), `Scope`.
- ⚠️ **Jangan edit isi database secara manual dari Mountpoint** (path WSL2 di Docker Desktop).
- Mount: `docker run -it --name belajar-volume-3 -v belajar-volume:/data alpine sh` → `/data` di container = **mount point**, `belajar-volume` = **volume-nya**.
- **Named Volume** (`-v belajar-volume:/data`) → dikelola Docker, lokasi ditentukan Docker → cocok untuk **database** (PostgreSQL, MySQL, Redis) & production.
- **Bind Mount** (`-v "${HOME}\Desktop\belajar-dockerfile\data:/data"`) → folder Windows ↔ container, file langsung terlihat dua arah → cocok untuk **source code** (`./src:/app/src`).
- Production pattern: `postgres_data:/var/lib/postgresql/data` (Named Volume) + `./src:/app/src` (Bind Mount).
- ⚠️ **Volume ≠ backup.** `docker volume rm` = data bisa hilang. Backup terpisah: PostgreSQL → `pg_dump` → `backup.sql` → disimpan di tempat lain.
- Volume = **persistence**; Backup = **disaster recovery**.


## VOLUME INSPECT (DETAIL) & BIND MOUNT PRAKTIK

- `docker volume inspect belajar-volume` → 5 field penting:
  - `Name` = nama volume (`belajar-volume`).
  - `Driver` = `local` → local storage, dikelola Docker di komputer sendiri (paling umum).
  - `Mountpoint` ⭐ = `/var/lib/docker/volumes/belajar-volume/_data` → **lokasi nyata data disimpan di dalam lingkungan Docker** (Windows → Docker Desktop → Docker → Volume → belajar-volume → `_data` → `data.txt`).
  - `Scope` = `local` → hanya ada di Docker environment lokal, bukan cloud/server lain.
  - `CreatedAt` = waktu volume dibuat (`Z` = UTC).
- ⚠️ Jangan edit langsung folder `/var/lib/docker/...` (Docker Desktop + Windows) — akses cukup lewat Docker.
- Inti: **Volume = tempat menyimpan data yang tetap ada walaupun Container dihapus.** Container 1 ❌ → `belajar-volume` ✅ → Container 2 ✅ (`data.txt` masih ada).
- **Praktik Bind Mount pertama**:
  ```powershell
  mkdir "$HOME\Desktop\belajar-dockerfile\data"
  docker run -it --name belajar-bind -v "${HOME}\Desktop\belajar-dockerfile\data:/data" alpine sh
  ```
  - Di dalam container: `echo "Halo dari Bind Mount" > /data/test.txt` → `cat /data/test.txt` → `exit`.
  - Cek dari Windows: `Get-ChildItem "$HOME\Desktop\belajar-dockerfile\data"` → `test.txt` muncul ✅ (folder Windows ↔ container, dua arah).
- Bedanya: **Named Volume** = Docker yang kelola; **Bind Mount** = kita yang kelola.
- Selanjutnya: **Docker Network** — kenapa Spring Boot bisa connect ke PostgreSQL/Redis pakai **nama container**, bukan `localhost`.


## DOCKER NETWORK

- Container di **network Docker yang sama** bisa saling berkomunikasi; `bridge` = jenis network Docker untuk itu (sejajar `host`, `none`).
- `docker network create belajar-network` → buat network; `docker network ls` → daftar network (bridge/host/none + milik kita).
- Masukkan container ke network: `--network belajar-network` saat `docker run`.
- ⭐ Docker punya **DNS internal** → container diakses pakai **nama container**, bukan `localhost`.
- Praktik:

```powershell
docker network create belajar-network
docker run -d --name belajar-nginx-network --network belajar-network -p 8082:80 belajar-nginx:v2
docker run -it --name belajar-client --network belajar-network alpine sh
```

- Dari dalam `belajar-client`: `wget -qO- http://belajar-nginx-network` → HTML Nginx muncul ✅ (container ↔ container lewat network).
- Nanti untuk Spring Boot: `DB_HOST=postgres` (nama container), bukan `localhost`.
- Eksperimen `localhost`: dari dalam container, `localhost` = **container itu sendiri** → `wget http://localhost:80` di `belajar-client` gagal (`Connection refused`), bukan menuju Nginx.
- ✅ Antar container → pakai **nama container**; ❌ `localhost` hanya untuk akses ke dirinya sendiri.
- JDBC Spring Boot: `jdbc:postgresql://postgres:5432/belajar_db` (nama service), bukan `localhost` — KECUALI Spring Boot jalan di Windows & PostgreSQL di Docker → `localhost:5432` benar.
- Lanjut: `docker network inspect` → lihat container mana saja di network + **IP** masing-masing.
- Kenapa `localhost` gagal: tidak ada app **listen port 80** di `belajar-client` → `Connection refused`; `wget` via **nama container** berhasil karena Docker DNS → IP container → Nginx :80.
- Cara cek: `exit` dari container, lalu `docker network inspect belajar-network` → bagian `"Containers"` berisi `Name` + `IPv4Address` (`172.xx.xx.x/16`) → bukti kedua container di network yang sama.
- Berikutnya: bedah `Containers` / `IPv4Address` / `Gateway`, lalu masuk ke **Docker Compose** 🚀.
- Bedah hasil inspect: `Name` (nama network), `Driver: bridge`, `Subnet` (`172.19.0.0/16` = rentang IP container), `Gateway` (`172.19.0.1` = gerbang network), `Containers` (container aktif + IP → `belajar-nginx-network` = `172.19.0.2`).
- Container yang di-`exit` (**Exited**) masih ada tapi tidak tampil di `Containers` network → hidupkan lagi: `docker start -ai belajar-client` (`-a` attach output, `-i` interactive) → `wget` nama container berhasil lagi → inspect kini menampilkan **2 container** (client ± `172.19.0.3`, jangan diasumsikan — lihat `inspect`).
- Kesimpulan network: Container → Network → komunikasi pakai **nama container** = dasar Spring Boot + postgres + redis + kafka.
- Berikutnya: **Docker Compose** — satu file `docker-compose.yml` untuk Spring Boot + PostgreSQL + Redis, tanpa `docker run` satu-per-satu.


## DOCKER COMPOSE (LEVEL 7)

- **Docker Compose = file konfigurasi untuk mengatur banyak container sekaligus** — pengganti `docker run` satu-per-satu (seperti "manager" container).
- Struktur: `services:` → tiap service punya `image:`, `container_name:`, `ports:` (opsional).
- Jalankan: `docker compose up -d` (dari folder file berada); cek: `docker compose ps`; hentikan + hapus resource project: `docker compose down`.
- Compose **otomatis membuat Docker Network** untuk project (`belajar-compose_default`) → semua service satu network → bisa saling komunikasi pakai nama service/container.
- Redis **tanpa `ports:`** tetap bisa diakses container lain lewat **network internal** — tidak semua port perlu dibuka ke Windows.
- Compose pertama:

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

- Cek di browser: `http://localhost:8083` → Welcome to nginx!
- **Nama project Compose = nama folder** tempat `docker-compose.yml` dijalankan (mis. `belajar-dockerfile_default`) — tanpa definisi network, Compose otomatis buat network project dan masukkan semua service.
- Bukti DNS Compose: `docker run -it --rm --network <project>_default alpine sh` → `wget -qO- http://belajar-compose-nginx` ✅ (`--rm` = auto-hapus setelah exit).
- ⭐ Di Compose, komunikasi pakai **nama service** (`nginx`, `redis`, `postgres`), bukan `container_name` dan bukan `localhost` → `postgres:5432`, `redis:6379`, `kafka:9092`.
- Berikutnya: **PostgreSQL + Volume di Compose** (data persistence) — rapikan dulu: `down` → `up -d` → `ps` dari folder yang benar.
- Merapikan: `cd` ke folder `belajar-compose` (cek dengan `pwd`) → `docker compose down` → `docker compose up -d` → `docker compose ps` → network berubah jadi `belajar-compose_default` (nama folder = nama project).
- PostgreSQL latihan nanti dibuat **terpisah & aman**: container `belajar-compose-postgres` + volume `belajar-postgres-data` — jangan sentuh PostgreSQL/Redis milik DigiAsk.
- Keputusan: `belajar-dockerfile` = **folder utama** pembelajaran (nama network `belajar-dockerfile_default` sudah benar, jadi acuan).
- PostgreSQL masuk Compose: image `postgres:16-alpine`, env `POSTGRES_USER/PASSWORD/DB`, port **`5433:5432`** (5433 karena 5432 dipakai DigiAsk), volume `belajar-postgres-data:/var/lib/postgresql/data` + deklarasi top-level `volumes:` → container dihapus, **data tetap ada**.
- Update compose: `down` → `up -d` → `ps` → target 3 container Up, postgres di `0.0.0.0:5433->5432/tcp`.
- ✅ Mini backend jadi: Nginx `localhost:8083`, PostgreSQL `localhost:5433`, Redis internal — PostgreSQL sudah pakai **named volume** (`belajar-postgres-data` → `/var/lib/postgresql/data`).
- Masuk PostgreSQL container: `docker exec -it belajar-compose-postgres psql -U postgres -d belajar_db` → prompt `belajar_db=#`.
- Uji SQL: `\l` (daftar DB) → `CREATE TABLE users (...)` → `INSERT INTO users ...` → `INSERT 0 2` → `SELECT * FROM users;` (Firman & Budi) 🎉.
- Berikutnya: **uji ketahanan data** — `docker compose down` → `up` → data `users` harus masih ada (bukti named volume). Jangan hapus volume!
- Eksperimen uji ketahanan: `\q` (keluar psql) → `docker compose down` (**container ❌, volume ✅** — volume tidak ikut dihapus) → cek `docker volume ls` (`belajar-postgres-data` masih ada) → `docker compose up -d` → `SELECT * FROM users;` → **data Firman & Budi masih ada** 🎯.
- Konsep: **container bisa dihapus, data tetap hidup karena di Volume**.
- ⚠️ Beda `down` vs `down -v`: `down` = volume & data ✅; `down -v` = volume & data ❌ (hapus semua). Jangan jalankan `down -v` dulu.
- ✅ **Hasil: Volume terbukti bekerja** — container dihapus & dibuat ulang, data Firman & Budi selamat karena volume sama. Inti: **container bukan tempat penyimpanan data; volume-lah yang menyimpan**.
- Rangkuman 5 konsep: **Image** (cetakan) → **Container** (hasil jalan) → **Volume** (simpan data) → **Network** (komunikasi via nama service) → **Compose** (menyatukan semuanya) = mini environment backend sungguhan.
- Arah berikutnya: **Docker + Spring Boot** — JDBC pakai `postgres:5432` (dari dalam Docker) vs `localhost:5433` (dari host/DBeaver); pelajari dulu `depends_on` + environment variables, baru project Spring Boot di Docker 🐳☕.
- **`depends_on`** (LEVEL 7.1) = urutan startup dependency (app → postgres, redis), **bukan jaminan service ready** — bisa ❌ koneksi karena PostgreSQL masih initialization; solusinya nanti `healthcheck`.
- **Environment variable** (LEVEL 7.2) = config ke container; PostgreSQL baca `POSTGRES_USER/PASSWORD/DB` saat init. Penting untuk multi-environment: `DB_HOST=postgres` (dev) vs `production-db.example.com` (prod), kode tetap sama. Cek langsung: `docker exec belajar-compose-postgres env`.
- Password di compose = latihan saja; **production pakai secret management**, jangan hardcode.
- Docker lengkap: Image, Container, Dockerfile, Volume, Network, Compose (+ env & depends_on) → berikutnya **Spring Boot di Docker** (ambil data dari PostgreSQL).
- Env var PostgreSQL terbukti masuk: `POSTGRES_DB=belajar_db`, `POSTGRES_USER=postgres`, `POSTGRES_PASSWORD=postgres` (aman untuk latihan; production nanti pakai `.env`/Docker secrets).
- **LEVEL 9 dimulai — Docker + Spring Boot + PostgreSQL**: app `:8080` → connect ke `postgres:5432` (nama service) di dalam Compose.
- Aturan host: dari **Windows** pakai `localhost:5433` (mapping 5433→5432); dari **container** pakai `postgres:5432` (nama service, bukan `localhost`) — inti pelajaran Docker Network.
- Rencana 7 tahap (project latihan terpisah dari DigiAsk): buat Spring Boot → jalan di Windows → connect PostgreSQL Docker → test API → buat Dockerfile → masukkan ke Docker → Compose jalan semuanya. Mulai pelan-pelan: **tahap 1 = project Spring Boot sederhana**.

