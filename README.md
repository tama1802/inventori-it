# Inventori Barang IT

Aplikasi inventori berbasis Laravel untuk mencatat perangkat IT, mencari dan memfilter aset, serta mengelola stok barang.

## Kebutuhan

- PHP 8.3 atau lebih baru dan Composer
- SQLite (bawaan konfigurasi awal) atau database lain yang didukung Laravel

## Menjalankan di Laragon (Windows)

1. Buka folder proyek `C:\laragon\www\inventori-it`.
2. Salin `.env.example` menjadi `.env` bila file `.env` belum ada.
3. Pasang dependensi dengan `composer install`.
4. Siapkan database SQLite: `if (-not (Test-Path database\database.sqlite)) { New-Item database\database.sqlite -ItemType File }`.
5. Jalankan migrasi dan data contoh: `php artisan migrate --seed`.
6. Jalankan aplikasi: `php artisan serve`.
7. Buka `http://127.0.0.1:8000`.

Di Laragon, PHP dan Composer biasanya sudah masuk ke PATH terminal Laragon. Alternatifnya, aktifkan web server Laragon lalu arahkan virtual host ke folder `public`.

## Fitur

- Tambah, edit, dan hapus aset
- Validasi server, kode aset unik, dan perlindungan CSRF
- Cari berdasarkan kode, nama, kategori, model, nomor seri, lokasi, atau pengguna
- Filter status, pagination, dan ringkasan aset
- Pencatatan tanggal barang masuk dan tanggal barang keluar; tanggal keluar membutuhkan tanggal masuk dan tidak boleh mendahuluinya
- Saat menambah barang, tanggal masuk otomatis terisi tanggal hari ini dan dapat disesuaikan; tanggal keluar tetap kosong sampai barang benar-benar keluar
- Pencatatan serta pencarian berdasarkan nama pemberi barang
- Tabel stok barang habis pakai terpisah dengan tambah/edit barang, jumlah tersedia, satuan, lokasi, dan indikator restok
- Status stok otomatis memakai ambang tetap 5 unit: **Habis** saat jumlah 0, **Menipis** saat jumlah 1–4, dan **Tersedia** saat jumlah 5 atau lebih. Pengaturan minimum lama tidak mengubah status.
- Riwayat transaksi stok masuk/keluar dengan tanggal, catatan, dan saldo setelah transaksi; stok tidak dapat dikeluarkan melebihi saldo tersedia
- Tiga record contoh dapat dimuat melalui seeder
- Data aset dan stok disimpan di database aplikasi.

Tanggal masuk dan keluar tersimpan sebagai tanggal opsional supaya aset lama tanpa riwayat tanggal tetap dapat digunakan. Jika tanggal keluar diisi, tanggal masuk juga wajib dan tanggal keluar harus sama dengan atau setelah tanggal masuk. Migrasi menambahkan kedua kolom tanpa menghapus data aset yang sudah ada.

## Akses aplikasi

Aplikasi ini belum menggunakan login. Jalankan hanya di komputer sendiri atau jaringan internal tepercaya. Jangan paparkan ke internet publik sebelum menambahkan autentikasi dan pembatasan akses.

## Mengganti database

Konfigurasi awal memakai SQLite. Untuk MySQL Laragon, sesuaikan `DB_CONNECTION=mysql`, `DB_HOST`, `DB_PORT`, `DB_DATABASE`, `DB_USERNAME`, dan `DB_PASSWORD` dalam `.env`, lalu jalankan `php artisan migrate --seed`.

## Pengujian

Jalankan `php artisan test`.
