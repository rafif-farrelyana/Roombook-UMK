
# RoomBook UMK — Sistem Peminjaman Ruang Kelas & Aula Fakultas

> Sistem informasi peminjaman ruang kelas dan aula terpadu berbasis web untuk sivitas akademika Universitas Muria Kudus (UMK).

---

## 1. Latar Belakang & Gambaran Umum

Sebelum adanya sistem RoomBook UMK, proses permohonan peminjaman ruangan di lingkungan Universitas Muria Kudus (seperti Auditorium, Aula Gedung J, Aula Masjid, hingga ruang-ruang kelas perkuliahan) dilakukan secara manual menggunakan surat fisik yang memakan waktu lama, sulit dilacak statusnya, serta rawan terjadi jadwal ganda (bentrok).

**RoomBook UMK** hadir sebagai solusi digital menyeluruh (*full-stack web application*) yang memfasilitasi pemohon (mahasiswa, ormawa/UKM, dan dosen) untuk memeriksa ketersediaan ruangan secara *real-time*, mengajukan booking dengan unggahan proposal/surat pendukung, memantau riwayat secara transparan, serta mengunduh bukti persetujuan resmi berformat PDF. Di sisi pengelola, admin ruangan dibekali instrumen verifikasi terpadu (setujui/tolak dengan alasan wajib), manajemen blokir tanggal pemeliharaan, rekapitulasi statistik penggunaan ruangan, dan ekspor laporan berkas CSV yang aman.

---

## 2. Fitur Utama Berdasarkan Peran

### A. Pemohon (Mahasiswa, Ormawa, Dosen)
- **Registrasi & Autentikasi Mandiri**: Pendaftaran akun sivitas UMK dengan validasi ketat nama, email, nomor HP (format Indonesia `08...`), serta nama instansi/organisasi.
- **Katalog Ruangan Dinamis**: Penjelajahan daftar ruangan aktif dilengkapi pencarian multi-kata kunci, filter kapasitas minimum, filter fasilitas (*projector*, AC, sound system, dsb.), dan pengurutan (*sorting*).
- **Detail Ruangan & Kalender Ketersediaan**: Inspeksi galeri foto, spesifikasi fasilitas, batas kapasitas, dan kalender interaktif FullCalendar yang memetakan status pemakaian (Disetujui, Menunggu Persetujuan, dan Diblokir).
- **Pengajuan Booking Cerdas**: Form interaktif dengan validasi aturan bisnis *real-time* (batas H-3, jam operasional 07:00–21:00 WIB, kelipatan 30 menit, kapasitas maksimum), deteksi dini bentrok jadwal, serta opsi unggah dokumen pendukung (PDF/JPG/PNG maks 2 MB).
- **Halaman "Booking Saya"**: Pelacakan status peminjaman (*pending*, *approved*, *rejected*, *cancelled*) dengan filter tab status dan pencarian.
- **Pembatalan Mandiri**: Pemohon dapat membatalkan booking berstatus *pending* atau *approved* (selama kegiatan belum terlewati) disertai konfirmasi aman.
- **Unduh Bukti Persetujuan (PDF)**: Booking yang disetujui menghasilkan lembar bukti resmi satu halaman A4 berformat `RB/<tahun>/<id 6 digit>` dengan tata letak institusional UMK.

### B. Admin (Pengelola Ruangan Fakultas/Kampus)
- **Dashboard Ringkasan Eksekutif**: Metrik kartu instan (pengajuan menunggu persetujuan, booking hari ini, total ruangan aktif, booking bulan ini) serta diagram tren peminjaman per bulan.
- **Verifikasi Pengajuan (Approve / Reject)**: Antarmuka evaluasi detail peminjam, jadwal, berkas proposal, tombol setujui dengan *anti-conflict check*, serta modal penolakan dengan catatan alasan wajib.
- **CRUD Ruangan Terpadu**: Tambah, edit, dan arsipkan ruangan dilengkapi *drag-and-drop / selector* foto ruangan (kompresi & validasi Multer) serta pengelolaan tag fasilitas dinamis.
- **Manajemen Blokir Tanggal (*Blocked Dates*)**: Blokir jadwal ruangan untuk keperluan ujian serentak, wisuda, Dies Natalis, atau pemeliharaan gedung (*maintenance*) dengan alasan transparan.
- **Laporan Penggunaan & Ekspor CSV**:
  - Rekapitulasi utilisasi jam operasional (dihitung berbasis 14 jam kerja harian).
  - Distribusi status booking dan ranking 5 besar organisasi/unit pemohon paling aktif.
  - Grafik utilisasi harian/bulanan.
  - Ekspor CSV komprehensif (mode Detail Booking & mode Ringkasan Ruangan) berstandar UTF-8 BOM dan proteksi injeksi formula Excel.
- **Manajemen Pengguna (*User Management*)**: Pemantauan daftar akun terdaftar, aktivasi/penonaktifan akun, dan penggantian peran pengguna.

---

## 3. Tech Stack

| Lapisan / Komponen | Teknologi & Pustaka | Keterangan & Rationale |
| :--- | :--- | :--- |
| **Frontend** | React 18 (Vite 6) | SPA performa tinggi dengan *fast refresh* |
| **Routing** | React Router v6 | *Nested layout*, rute privat (`ProtectedRoute`), rute peran (`RoleRoute`), dan *lazy loading* |
| **Styling** | Tailwind CSS v3 | Desain responsif bertema resmi UMK: Hijau Tua (`#0D5C3A`) & Navy (`#0E2A47`) |
| **Kalender** | FullCalendar v6 | Tampilan bulanan (`dayGridMonth`), mingguan (`timeGridWeek`), dan harian (`timeGridDay`) |
| **Ikonografi** | Lucide React | Ikon modern, ringan, dan konsisten |
| **HTTP Client** | Axios | *Request interceptor* injeksi token Bearer & *response interceptor* auto-logout (401) |
| **Backend** | Node.js (ES Modules) | Arsitektur modern ES Modules (`"type": "module"`) |
| **Web Framework** | Express 4 | RESTful API terstruktur (*Controller-Service-Route*) |
| **Database** | PostgreSQL 18 | RDBMS tangguh di port 5433 (menggunakan driver `pg`) |
| **Autentikasi** | JSON Web Token (JWT) + bcryptjs | Token Bearer *stateless* & enkripsi *salt* 10 putaran |
| **Validasi Skema** | Zod | Validasi deklaratif terpadu pada frontend dan backend |
| **Upload Berkas** | Multer | Penanganan unggahan berkas gambar ruangan dan proposal PDF/JPG/PNG (maks 2 MB) |
| **Mesin PDF** | pdf-lib | Pembuatan berkas PDF vektor A4 mandiri dengan sanitasi WinAnsi |
| **Keamanan HTTP** | Helmet, CORS, Express-Rate-Limit | *Security headers*, pembatasan asal permintaan, dan perlindungan *brute-force* |

---

## 4. Prasyarat Sistem (*Prerequisites*)

Sebelum memulai instalasi, pastikan sistem Anda telah terpasang:
1. **Node.js**: Versi LTS 20.x atau 22.x (mendukung npm v10+)
2. **PostgreSQL**: Versi 14, 15, 16, 17, atau 18
3. **OS**: Windows 10/11 (menggunakan terminal PowerShell) atau Linux/macOS
4. **Git**: Untuk kloning repositori

---

## 5. Panduan Instalasi & Menjalankan Proyek (Windows PowerShell)

### Langkah 1: Kloning Repositori & Buka Direktori
```powershell
cd C:\Users\ASUS\.gemini\antigravity\scratch
git clone <URL_REPOSITORI> roombook-umk
cd roombook-umk
```

### Langkah 2: Persiapan Database PostgreSQL
Buka psql atau pgAdmin, pastikan PostgreSQL berjalan (pada konfigurasi lokal ini berjalan di port **5433**):
```sql
CREATE DATABASE roombook_umk;
```
Pastikan ekstensi `btree_gist` dapat diaktifkan (migrasi otomatis mengeksekusi `CREATE EXTENSION IF NOT EXISTS btree_gist;` untuk *constraint* anti-bentrok).

---

### Langkah 3: Konfigurasi & Menjalankan Backend

1. **Masuk ke folder backend**:
   ```powershell
   cd C:\Users\ASUS\.gemini\antigravity\scratch\roombook-umk\backend
   ```

2. **Instal dependensi**:
   ```powershell
   npm install
   ```

3. **Buat file konfigurasi lingkungan `.env`**:
   Salin `.env.example` menjadi `.env`:
   ```powershell
   Copy-Item .env.example .env
   ```
   Sesuaikan nilainya sesuai konfigurasi PostgreSQL Anda:
   ```ini
   PORT=5000
   NODE_ENV=development

   # Konfigurasi Database PostgreSQL
   DB_HOST=127.0.0.1
   DB_PORT=5433
   DB_NAME=roombook_umk
   DB_USER=postgres
   DB_PASSWORD=postgres

   # Kunci Rahasia JWT (Wajib minimal 32 karakter!)
   JWT_SECRET=rahasia-kunci-jwt-roombook-umk-sangat-aman-2026-xyz

   # CORS Client URL (Bisa dipisah koma untuk multiple origin)
   CLIENT_URL=http://localhost:5173,http://127.0.0.1:5173

   # Batasan Ukuran Upload
   MAX_FILE_SIZE_MB=2
   ```

4. **Jalankan Migrasi Database**:
   ```powershell
   npm run db:migrate
   ```
   *Skrip ini akan mengeksekusi migrasi skema tabel utama dan tabel audit log secara berurutan dengan pencatatan `schema_migrations`.*

5. **Jalankan Seeder Master Data Ruangan & Akun Default**:
   ```powershell
   npm run db:seed
   ```

6. **(Opsional) Jalankan Seeder Data Demo Lengkap**:
   Untuk mengisi database dengan berbagai variasi booking (disetujui, menunggu, ditolak, dibatalkan) serta blokir tanggal untuk pengujian laporan dan kalender:
   ```powershell
   npm run db:seed:demo
   ```

7. **Jalankan Server Backend (Development)**:
   ```powershell
   npm run dev
   # Atau eksekusi langsung: node server.js
   ```
   Backend akan aktif di `http://localhost:5000`. Cek kesehatan server di `http://localhost:5000/api/health`.

---

### Langkah 4: Konfigurasi & Menjalankan Frontend

1. **Buka terminal baru, masuk ke folder frontend**:
   ```powershell
   cd C:\Users\ASUS\.gemini\antigravity\scratch\roombook-umk\frontend
   ```

2. **Instal dependensi**:
   ```powershell
   npm install
   ```

3. **Buat file konfigurasi `.env`**:
   ```powershell
   Copy-Item .env.example .env
   ```
   Isi file `frontend/.env`:
   ```ini
   VITE_API_BASE_URL=http://localhost:5000/api
   ```

4. **Jalankan Server Frontend (Development)**:
   ```powershell
   npm run dev
   ```
   Aplikasi dapat diakses melalui browser pada `http://localhost:5173`.

5. **Build Produksi Frontend**:
   ```powershell
   npm run build
   ```
   *Hasil build statis tersimpan di folder `frontend/dist/`. Pada build produksi, tombol instan "Demo Akun" pada halaman login otomatis disembunyikan demi keamanan.*

---

## 6. Akun Demo Bawaan

Aplikasi dilengkapi akun terdaftar siap pakai untuk pengujian:

| Peran (*Role*) | Email | Password | Kegunaan |
| :--- | :--- | :--- | :--- |
| **Admin** | `admin@umk.ac.id` | `Admin123!` | Kelola ruangan, persetujuan booking, blokir tanggal, unduh laporan |
| **Dosen (Pemohon)** | `dosen@umk.ac.id` | `Dosen123!` | Peminjaman ruang kuliah umum, sidang, seminar |
| **Mahasiswa (Pemohon)** | `mahasiswa@umk.ac.id` | `Mahasiswa123!` | Peminjaman aula kegiatan BEM/HIMA/UKM |

> [!WARNING]
> **Peringatan Keamanan Produksi**: Sebelum mendistribusikan ke lingkungan produksi (*live*), segera ubah kata sandi akun default di atas, ganti `JWT_SECRET` dengan string acak dengan entropi tinggi, dan set `NODE_ENV=production`.

---

## 7. Aturan Bisnis & Logika Sistem

1. **Zona Waktu Terpadu**:
   - Seluruh aturan waktu dievaluasi dalam Waktu Indonesia Barat (**WIB**, UTC+7).
   - Backend menggunakan konstanta `APP_UTC_OFFSET = "+07:00"` sehingga tidak terpengaruh perbedaan zona waktu lokal server hosting maupun perangkat pengguna.
2. **Aturan Batas Waktu Booking (H-3)**:
   - Pengajuan peminjaman wajib dilakukan minimal **3 hari kalender penuh** sebelum tanggal kegiatan (`event_date >= today + 3 days`).
   - Hari ini (H+0), besok (H+1), dan lusa (H+2) otomatis terkunci dan ditolak oleh validator skema.
3. **Jam Operasional & Durasi**:
   - Jam operasional peminjaman: **07:00 WIB hingga 21:00 WIB**.
   - Waktu mulai dan waktu selesai wajib mengikuti kelipatan interval **30 menit** (contoh: 08:00, 08:30, 09:00).
   - Durasi peminjaman: minimal **60 menit** (1 jam) dan maksimal **14 jam** (seluruh slot hari tersebut).
4. **Prinsip Satu Hari Per Booking**:
   - Satu nomor permohonan booking berlaku untuk satu tanggal kegiatan (`start_time` dan `end_time` pada tanggal yang sama).
   - Kegiatan beruntun multi-hari diajukan sebagai pengajuan booking terpisah per masing-masing hari.
5. **Pencegahan Bentrok Jadwal (*Anti-Conflict Mechanism*)**:
   - Dilindungi ganda (*two-tier defense*):
     - **Tingkat Aplikasi**: Pengecekan *overlap query* saat pengajuan dan saat admin mengklik tombol "Setujui". Jika slot telah terisi booking *approved* lain atau tanggal terblokir (*blocked dates*), sistem mengembalikan HTTP 409 Conflict.
     - **Tingkat Database**: PostgreSQL `EXCLUDE USING gist (room_id WITH =, tstzrange(start_time, end_time) WITH &&) WHERE (status = 'approved')` menjamin integritas data secara mutlak bahkan jika terjadi *race condition* konkurensi tinggi.
6. **Formula Utilisasi Ruangan**:
   $$\text{Tingkat Utilisasi (\%)} = \min\left(100, \frac{\text{Total Jam Disetujui}}{14 \times \text{Jumlah Hari Periode}} \times 100\right)$$
   *(Dasar perhitungan kapasitas: 14 jam operasional per hari dari pukul 07:00 hingga 21:00 WIB).*

---

## 8. Ringkasan Endpoint API

Semua endpoint diawali dengan prefiks `/api`:

### Autentikasi (`/api/auth`)
| Metode | Endpoint | Hak Akses | Keterangan |
| :--- | :--- | :--- | :--- |
| `POST` | `/auth/register` | Publik | Registrasi akun baru (peran otomatis `pemohon`) |
| `POST` | `/auth/login` | Publik | Autentikasi kredensial & penerbitan Bearer token |
| `GET` | `/auth/me` | Wajib Login | Mengambil profil pengguna yang sedang aktif |

### Ruangan (`/api/rooms`)
| Metode | Endpoint | Hak Akses | Keterangan |
| :--- | :--- | :--- | :--- |
| `GET` | `/rooms` | Wajib Login | Daftar katalog ruangan (pemohon: hanya aktif; admin: semua) |
| `GET` | `/rooms/:id` | Wajib Login | Detail lengkap satu ruangan |
| `GET` | `/rooms/meta/filters` | Wajib Login | Daftar unik gedung dan fasilitas untuk filter UI |
| `POST` | `/rooms` | Admin | Menambah ruangan baru (multipart/form-data foto) |
| `PUT` | `/rooms/:id` | Admin | Memperbarui data ruangan & foto |
| `DELETE`| `/rooms/:id` | Admin | Menghapus ruangan (soft-delete jika ada riwayat booking) |

### Peminjaman Ruangan (`/api/bookings`)
| Metode | Endpoint | Hak Akses | Keterangan |
| :--- | :--- | :--- | :--- |
| `GET` | `/bookings/calendar` | Wajib Login | Data event kalender ketersediaan (approved, pending, blocked) |
| `POST` | `/bookings` | Wajib Login | Mengajukan peminjaman baru (unggah proposal opsional) |
| `GET` | `/bookings/my` | Wajib Login | Mengambil daftar riwayat booking milik pengguna aktif |
| `GET` | `/bookings/:id` | Wajib Login | Detail booking (hanya pemilik atau admin) |
| `PATCH`| `/bookings/:id/cancel` | Wajib Login | Membatalkan booking (hanya pemilik booking) |
| `GET` | `/bookings/:id/attachment`| Wajib Login | Mengunduh berkas surat proposal terlampir |
| `GET` | `/bookings/:id/receipt` | Wajib Login | Mengunduh lembar bukti persetujuan resmi (PDF) |

### Panel & Operasional Admin (`/api/admin`)
| Metode | Endpoint | Hak Akses | Keterangan |
| :--- | :--- | :--- | :--- |
| `GET` | `/admin/dashboard` | Admin | Statistik kartu ringkasan & grafik booking bulanan |
| `GET` | `/admin/bookings` | Admin | Daftar seluruh pengajuan masuk dengan filter status/ruang |
| `PATCH`| `/admin/bookings/:id/approve` | Admin | Menyetujui pengajuan peminjaman |
| `PATCH`| `/admin/bookings/:id/reject` | Admin | Menolak pengajuan (alasan penolakan wajib diisi) |
| `GET` | `/admin/blocked-dates` | Admin | Daftar tanggal pemeliharaan yang diblokir |
| `POST` | `/admin/blocked-dates` | Admin | Menambahkan blokir tanggal baru |
| `DELETE`| `/admin/blocked-dates/:id` | Admin | Membuka kunci blokir tanggal |
| `GET` | `/admin/users` | Admin | Daftar seluruh pengguna terdaftar |
| `PATCH`| `/admin/users/:id/status` | Admin | Mengaktifkan/menonaktifkan akun pengguna |

### Laporan & Ekspor (`/api/reports`)
| Metode | Endpoint | Hak Akses | Keterangan |
| :--- | :--- | :--- | :--- |
| `GET` | `/reports/usage` | Admin | Mengambil metrik utilisasi ruangan, agregasi status, & tren |
| `GET` | `/reports/usage/export` | Admin | Mengunduh berkas CSV laporan (format detail / summary) |

### Pemeriksaan Kesehatan Sistem
| Metode | Endpoint | Hak Akses | Keterangan |
| :--- | :--- | :--- | :--- |
| `GET` | `/health` | Publik | Status server & konektivitas aktif PostgreSQL |

---

## 9. Struktur Direktori Proyek

```
roombook-umk/
├── backend/
│   ├── src/
│   │   ├── config/             # Konfigurasi db, env validator, multer, constants
│   │   ├── controllers/        # Request handlers (auth, room, booking, admin, report)
│   │   ├── db/
│   │   │   ├── migrations/     # File migrasi SQL mentah (001_initial, 002_enhancements, ...)
│   │   │   ├── migrate.js      # Pelari migrasi otomatis dengan schema_migrations
│   │   │   ├── seed.js         # Seeder master akun & ruangan awal
│   │   │   └── seedDemo.js     # Seeder skenario demo multi-status
│   │   ├── middlewares/        # auth, role, validate, upload, errorHandler
│   │   ├── routes/             # Deklarasi router express per modul
│   │   ├── services/           # Logika bisnis (booking, report, receipt PDF, audit)
│   │   ├── utils/              # csvUtils, dateUtils, pdfSanitizer, response
│   │   └── validators/         # Skema validasi Zod backend
│   ├── uploads/                # Direktori berkas privat (lampiran surat & foto ruangan)
│   ├── .env.example
│   ├── package.json
│   └── server.js               # Titik masuk server express, routes, & graceful shutdown
├── frontend/
│   ├── public/
│   │   └── favicon.svg         # Favicon tema kampus UMK
│   ├── src/
│   │   ├── api/                # Klien Axios & fungsi API terisolasi per domain
│   │   ├── components/         # Komponen umum (Navbar, Modal, Dialog, Skeleton, dll.)
│   │   ├── contexts/           # AuthContext (state sesi global pengguna)
│   │   ├── layouts/            # MainLayout & AuthLayout
│   │   ├── pages/
│   │   │   ├── admin/          # Dashboard, Rooms, Bookings, BlockedDates, Reports, Users
│   │   │   ├── pemohon/        # RoomsCatalog, RoomDetail, BookingCreate, MyBookings, BookingDetail
│   │   │   └── public/         # LoginPage, RegisterPage, NotFoundPage
│   │   ├── utils/              # dateUtils, currency/formatters
│   │   ├── App.jsx             # React Router v6, Suspense, Lazy loading, ErrorBoundary
│   │   ├── index.css           # Konfigurasi Tailwind CSS
│   │   └── main.jsx
│   ├── .env.example
│   ├── index.html
│   ├── package.json
│   ├── tailwind.config.js
│   └── vite.config.js
└── README.md
```

---

## 10. Catatan Keamanan & Solusi Kendala (*Security & Troubleshooting*)

### Mitigasi Keamanan Khusus
1. **Proteksi Injeksi Formula CSV (*Formula Injection / CSV Injection*)**:
   - Berkas CSV yang diunduh rentan dieksploitasi jika ada input nama kegiatan atau nama pengguna yang diawali karakter `=`, `+`, `-`, `@`, `\t`, atau `\r`.
   - Modul `backend/src/utils/csvUtils.js` secara otomatis memprefix karakter sensitif tersebut dengan tanda kutip tunggal `'` sebelum mengekspor data ke Excel.
2. **Kesesuaian Font PDF (*pdf-lib WinAnsi Glyphs*)**:
   - Standar font Helvetica pada `pdf-lib` hanya mendukung set karakter WinAnsi. Karakter non-Latin, *smart-quotes*, dan emoji akan menyebabkan galat jika tidak ditangani.
   - Modul `backend/src/utils/pdfSanitizer.js` menyaring karakter non-WinAnsi menjadi karakter terdekat yang aman atau tanda tanya (`?`), serta memotong baris teks berlebih (*word-wrap*) secara proporsional.
3. **Penyimpanan Berkas Terisolasi (*Private Storage*)**:
   - Berkas lampiran surat/proposal pemohon tidak disajikan secara publik sebagai *static files*. Berkas dilindungi di balik endpoint `GET /api/bookings/:id/attachment` dengan otorisasi JWT (hanya pemilik booking dan admin yang diizinkan).
4. **Hasil Audit Keamanan (`npm audit`)**:
   - Backend: **0 Vulnerabilities**.
   - Frontend: Ditemukan advisori dependensi pengembangan bawaan (`esbuild`/`vite` dev server serta `react-router`/`zod` v4). Pembaruan otomatis tidak dipaksakan (`--force`) karena melibatkan perubahan versi mayor (*breaking change*) yang berpotensi merusak kompatibilitas pustaka inti.

### Pemecahan Masalah Umum (*Troubleshooting*)
- **Galat `EADDRINUSE: address already in use :::5000`**:
  Proses server Node.js sebelumnya masih aktif di latar belakang. Jalankan perintah berikut di PowerShell untuk mematikan proses:
  ```powershell
  Get-Process node -ErrorAction SilentlyContinue | Stop-Process -Force
  ```
- **Koneksi Database Gagal (`ECONNREFUSED 127.0.0.1:5433`)**:
  Pastikan layanan PostgreSQL berjalan di Windows Services (`services.msc`) dan periksa port pada file `backend/.env`. Jika PostgreSQL Anda berjalan di port standar (5432), sesuaikan `DB_PORT=5432`.
- **Karakter Indonesia Tidak Muncul di Microsoft Excel**:
  Modul ekspor CSV telah menyisipkan **UTF-8 BOM** (`\uFEFF`) di awal berkas. Jika Excel versi lawas Anda menggunakan pemisah titik-koma (*semicolon*), tambahkan query `?delimiter=;` saat memanggil ekspor CSV dari browser.

---

## 11. Rencana Pengembangan Selanjutnya (*Future Roadmap*)

- [ ] Integrasi notifikasi email otomatis (SMTP / Mailgun) saat status booking disetujui atau ditolak.
- [ ] Integrasi notifikasi WhatsApp Gateway ke nomor HP sivitas pemohon.
- [ ] Fitur peminjaman fasilitas tambahan bergerak (proyektor portabel, mic nirkabel, podium) secara terpisah.
- [ ] Dukungan booking berulang (*recurring booking*) mingguan untuk jadwal perkuliahan semester berjalan.
- [ ] Autentikasi Single Sign-On (SSO) terintegrasi dengan akun Portal Akademik Kampus (OAuth2 / SAML).

---

&copy; 2026 RoomBook UMK — Universitas Muria Kudus. Dibuat dengan dedikasi untuk kemudahan sivitas akademika.

## Panduan Setup Lokal (Database & Backend)

### Prasyarat
- **Node.js** & **npm** terpasang (`node -v` dan `npm -v`)
- **PostgreSQL** & **pgAdmin 4** terpasang dan berjalan
- **Git** & akses ke repositori GitHub **Roombook-UMK**

### Langkah Instalasi & Menjalankan Database
1. **Clone repositori & masuk ke branch `develop`:**
   ```bash
   git clone <URL_REPO_ROOMBOOK_UMK>
   cd <nama_folder_repo>
   git checkout develop
   git pull origin develop

# Roombook-UMK
Sistem pemesanan ruang kelas dan aula berbasis web - RPL Kelompok 2
## Aturan Branch
- main: versi stabil, tidak boleh push langsung
- develop: gabungan fitur yang sudah selesai
- feat/nama-fitur: fitur baru, contoh feat/login-auth
- fix/nama-bug: perbaikan bug, contoh fix/bentrok-jadwal
- docs/nama-dokumen: dokumentasi, contoh docs/erd

## Aturan Commit
Awali pesan dengan jenis perubahan, contoh:
- feat: tambah halaman katalog ruangan
- fix: perbaiki validasi jam booking
- docs: tambah diagram ERD

## Alur Kerja
1. Ambil update terbaru dari develop
2. Buat branch dari develop
3. Commit dan push branch itu
4. Buka Pull Request ke develop
5. Tunggu 1 orang me-review sebelum merge
