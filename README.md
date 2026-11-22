# RumaKost

**Sistem Manajemen Kost Mandiri** — Aplikasi web pengelolaan operasional kost berbasis Next.js 16, React 19, Neon PostgreSQL, Prisma 7, dan Better Auth.

---

## Prasyarat Sistem

- **Node.js**: Versi `24.x` atau `>=24.0.0`
- **Package Manager**: `npm` (atau `pnpm`)
- **Database**: Akun / project **Neon PostgreSQL** (Pooled connection & Direct connection)
- **Object Storage**: Neon Object Storage (S3-compatible)

---

## Panduan Instalasi dan Menjalankan

### 1. Clone Repository
```powershell
git clone <url-repository>
cd rumakost
```

### 2. Setup Environment Variable
Salin template environment ke `.env.local`:
```powershell
Copy-Item .env.example .env.local
```
Buka `.env.local` dan isi nilainya sesuai branch database Neon Anda:
- `DATABASE_URL`: Connection string Neon dengan **Pooled connection** aktif.
- `DIRECT_URL`: Connection string Neon dengan **Pooled connection** nonaktif (untuk migrasi Prisma).
- `BETTER_AUTH_URL`: `http://localhost:3000` (atau URL domain live).
- `BETTER_AUTH_SECRET`: Secret acak minimal 32 karakter (buat via `node -e "process.stdout.write(require('node:crypto').randomBytes(32).toString('base64url'))"`).
- `APP_ORIGIN`: Harus sama dengan `BETTER_AUTH_URL` (`http://localhost:3000`).
- `CRON_SECRET`: Secret acak minimal 16 karakter.
- Kredensial Neon Object Storage: `NEON_STORAGE_ENDPOINT`, `NEON_STORAGE_REGION`, `NEON_STORAGE_BUCKET`, `NEON_STORAGE_ACCESS_KEY`, `NEON_STORAGE_SECRET_KEY`.

> [!IMPORTANT]
> File `.env.local` bersifat rahasia dan sudah diabaikan oleh `.gitignore`. Jangan pernah commit file tersebut atau membagikan isinya ke publik.

### 3. Instalasi Dependency
```powershell
npm install
```

> [!TIP]
> **Khusus Pengguna Windows**: Jika instalasi terasa lambat atau muncul peringatan `mkdir / errno -4094`, tambahkan folder repositori `rumakost` ke dalam *Exclusion List* di Windows Defender (*Virus & threat protection settings > Exclusions*) untuk mencegah scanning disk real-time yang mengunci file saat instalasi.

### 4. Menjalankan Aplikasi

#### Mode Development:
```powershell
npm run dev
```
Server lokal akan aktif di `http://localhost:3000`. Perintah ini otomatis menjalankan `prisma generate` tanpa koneksi database terlebih dahulu, lalu menyalakan server Next.js dengan Webpack.

#### Mode Production:
Kompilasi dan jalankan build produksi:
```powershell
npm run build
npm run prod
```
Aplikasi yang sudah dioptimasi akan melayani permintaan di `http://localhost:3000`.

---

## Daftar Script (`package.json`)

| Command | Keterangan |
|---|---|
| `npm run dev` | Menjalankan server development Next.js dengan Webpack |
| `npm run build` | Menghasilkan production build yang telah dioptimasi |
| `npm run prod` / `npm start` | Menjalankan server hasil build produksi |
| `npm run db:generate` | Menghasilkan Prisma Client lokal ke `src/generated/prisma` |
| `npm run db:migrate` | Menerapkan migrasi schema Prisma ke database Neon (`DIRECT_URL`) |
| `npm run typecheck` | Menjalankan validasi tipe TypeScript strict tanpa emit file |
| `npm run lint` | Menjalankan pemeriksaan kode dengan ESLint |
| `npm test` | Menjalankan suite pengujian unit dan integrasi |
| `npm run db:bootstrap-demo` | Menyiapkan akun demo awal sintetis pada branch Neon yang ditentukan |

---

## Struktur Folder Utama

- `src/app/`: App router Next.js (halaman publik, autentikasi, portal penghuni, dan dasbor).
- `src/components/`: Komponen antarmuka pengguna (UI reusable, layout, dashboard, tickets, dsb).
- `src/lib/`: Logika bisnis domain, repositori database, otorisasi, billing, storage, dan validasi Zod.
- `src/actions/`: Server actions untuk mutasi data dan alur kerja aplikasi.
- `prisma/`: Definisi `schema.prisma`, migrasi database, dan seed data.
- `docs/`: Dokumentasi spesifikasi produk (PRD, FSD, arsitektur, panduan desain, dan task TODO).
- `tests/`: Suite pengujian fungsional dan integritas data.

---

## Panduan Pengembang

Silakan pelajari [CONVENTIONS.md](CONVENTIONS.md) untuk aturan teknis, standar TypeScript strict, arsitektur server-first, dan [RUNBOOK_LOCAL.md](RUNBOOK_LOCAL.md) untuk panduan teknis operasional lokal dan pengujian database disposable.
