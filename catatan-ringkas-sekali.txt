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

