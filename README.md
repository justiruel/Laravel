# Langkah instalasi Laravel

```
composer create-project laravel/laravel project-latihan --prefer-dist
```
definisi "--prefer-dist" dapat dibaca di : https://getcomposer.org/doc/03-cli.md#install

# Cara Akses 
http://localhost:9000/project-latihan/public/


# Basic Laravel 
- Routing (routes/web.php)
- View (resources/views)

# Migration 
## Setting Database
- config/database.php
- .ENV
## Proses Migrasi
```
php artisan make:migration buat_table_anggota

NB. apabila ada kesalahan dan ingin remove migration : https://stackoverflow.com/a/17830269
```
hasilnya bisa dilihat di database/migrations

- modifikasi sesuai kebutuhan 2018_03_24_015832_buat_table_anggota.php
- ketik di cli : php artisan migrate

## error yang mungkin ditemui
Laravel 5.4: Specified key was too long error : https://laravel-news.com/laravel-5-4-key-too-long-error

# Eloquent Model
```
php artisan make:model Anggota
```
Hasilnya : app/Anggota.php (Model)
# check Version
http://www.elcoderino.com/check-laravel-version/

# macam-macam error
- Laravel 5.4: Specified key was too long error : https://laravel-news.com/laravel-5-4-key-too-long-error

# cari tau
baseurl
uri segment
.htaccess
pagination
