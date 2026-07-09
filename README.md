# Sistem Manajemen Inventaris - PT Telkomsel

Aplikasi web untuk mengelola inventaris kantor: master data barang, peminjaman barang, laporan, dan manajemen pengguna berbasis role. Dibangun menggunakan Laravel sebagai bagian dari Challenge Seleksi Magang Sistem Informasi.

## Fitur Utama

- **Autentikasi**: Login, Register, Logout, Forgot Password
- **Role Management**: Admin (Full Access), Staff (kelola inventaris), Manager (lihat laporan)
- **Master Data Barang**: CRUD lengkap, pencarian, filter kategori, pagination, upload gambar
- **Peminjaman Barang**: Tambah peminjaman (multi-barang), pengembalian otomatis (stok tersinkron), riwayat, status
- **Dashboard**: Statistik real-time, grafik peminjaman bulanan, aktivitas terbaru, notifikasi stok menipis
- **Laporan Inventaris**: Laporan stok + ringkasan peminjaman, filter tanggal, export PDF
- **Export Excel**: Export data Master Data Barang ke `.xlsx`
- **Atur Pengguna**: Admin dapat menambah, mengubah role, dan menghapus akun pengguna

## Tech Stack

- **Backend**: Laravel 13, PHP 8.3
- **Frontend**: Blade Templates, Tailwind CSS
- **Database**: MySQL
- **Autentikasi**: Laravel Breeze
- **PDF Export**: barryvdh/laravel-dompdf
- **Excel Export**: maatwebsite/excel
- **Chart**: Chart.js (grafik peminjaman bulanan)

## Struktur Database

| Tabel               | Keterangan                                |
| ------------------- | ----------------------------------------- |
| `users`             | Data pengguna sistem, terhubung ke role   |
| `roles`             | Referensi role (Admin, Staff, Manager)    |
| `categories`        | Kategori barang                           |
| `products`          | Master data barang                        |
| `borrowings`        | Data transaksi peminjaman                 |
| `borrowing_details` | Detail barang yang dipinjam per transaksi |

## Cara Instalasi

### 1. Clone repository

```bash
git clone <url-repository-ini>
cd inventaris-telkomsel
```

### 2. Install dependency PHP

```bash
composer install
```

### 3. Install dependency JavaScript

```bash
npm install
```

### 4. Salin file environment

```bash
cp .env.example .env
```

Kemudian sesuaikan konfigurasi database di `.env`:

```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=inventaris_telkomsel
DB_USERNAME=root
DB_PASSWORD=
```

### 5. Generate application key

```bash
php artisan key:generate
```

### 6. Buat database

Buat database kosong dengan nama sesuai `DB_DATABASE` di `.env` (contoh: `inventaris_telkomsel`) lewat phpMyAdmin, HeidiSQL, atau CLI MySQL.

> Alternatif: import langsung file `database.sql` yang disertakan di repository ini ke database kosong tersebut, lalu **lewati langkah migration & seeding di bawah**.

### 7. Jalankan migration & seeder

```bash
php artisan migrate --seed
```

Perintah ini akan membuat seluruh tabel sekaligus mengisi data awal (role, akun testing, kategori & produk contoh).

### 8. Buat symbolic link storage (untuk upload gambar barang)

```bash
php artisan storage:link
```

### 9. Compile asset frontend

```bash
npm run build
```

Untuk mode development (auto-recompile saat ada perubahan):

```bash
npm run dev
```

### 10. Jalankan server

```bash
php artisan serve
```

Aplikasi dapat diakses di `http://127.0.0.1:8000` (atau sesuai domain lokal Laragon/Valet yang kamu pakai).

## Akun Login Testing

> ⚠️ Sesuaikan tabel ini dengan email & password asli yang ada di `database/seeders/UserSeeder.php` pada project kamu.

| Role    | Email                 | Password      |
| ------- | --------------------- | ------------- |
| Admin   | admin@telkomsel.com   | password 1234 |
| Staff   | staff@telkomsel.com   | password 123  |
| Manager | manager@telkomsel.com | password 123  |

## Struktur Role & Hak Akses

| Role        | Master Data Barang | Kategori Barang | Peminjaman Barang | Laporan Inventaris | Atur Pengguna  |
| ----------- | :----------------: | :-------------: | :---------------: | :----------------: | :------------: |
| **Admin**   |    Full Access     |   Full Access   |    Full Access    | ✅ Lihat + Export  | ✅ Full Access |
| **Staff**   |    Full Access     |   Full Access   |    Full Access    |         ❌         |       ❌       |
| **Manager** |     Lihat saja     |   Lihat saja    |    Lihat saja     | ✅ Lihat + Export  |       ❌       |

## Alur Bisnis Singkat

1. **Peminjaman**: Saat peminjaman baru dicatat, stok barang terkait otomatis berkurang sesuai jumlah yang dipinjam.
2. **Pengembalian**: Saat barang ditandai "dikembalikan", stok barang otomatis dikembalikan (increment) sesuai jumlah yang dipinjam sebelumnya.
3. **Notifikasi Stok Menipis**: Sistem otomatis menandai barang dengan stok ≤ 5 sebagai "stok menipis", ditampilkan di dashboard dan lonceng notifikasi pada sidebar.
4. **Laporan**: Menggabungkan data stok barang terkini dengan ringkasan aktivitas peminjaman pada rentang tanggal yang dipilih, dapat diexport ke PDF.

## Link Demo

(https://inventaris-telkomsel-production.up.railway.app )

## Kontak

Dibuat oleh **[Fasikhul Lisan]** untuk Challenge Seleksi Magang Sistem Informasi PT Telkomsel.
