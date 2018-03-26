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
- Model (app/)
- Controller (app/Http/Controllers)

# Migration 
## Setting Database
- config/database.php (optional)
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

# Blade
- Untuk membuat suatu view bisa di isi dengan syntaxt blade, rename view name menjadi <b>*.blade.php</b>
- .blade view akan di-render menjadi php native untuk selanjutnya di eksekusi, lokasi render ada di <b>storage/framework/views/9316a91367544223c7eb6e4c3e30a8d44b85c1d9.php</b> 
- untuk menggunakan form helper semisal :
```
{{Form::textarea('notes')}}

hasil render ->
<textarea name="notes" cols="50" row="10"></textarea>
```
lakukan perintah : composer require "illuminate/html":"5.0.*"

# Controller 
Generate Controller baru
```
php artisan make:controller cobaController --resource
```
ket. --resource digunakan agar controller otomatis menggenerate method index(),create(),show(), dll.

# check Version
http://www.elcoderino.com/check-laravel-version/

# API
- php artisan make:resource coba   --> hasilnya akan muncul di App
- routes/api.php --> arahkan router
- contoh kasus cobaApiController
- cara akses : http://localhost:9000/project-latihan/public/api/cobaApi
- kenapa saat akses api harus ada prefix api? public/<b>API</b>/cobaApi, jawabannya coba buka RouteServiceProvider.php
```
protected function mapApiRoutes()
    {
        Route::prefix('api')
             ->middleware('api')
             ->namespace($this->namespace)
             ->group(base_path('routes/api.php'));
    }
```
jika tidak ingin ada prefix api maka rubah jadi 
```
protected function mapApiRoutes()
    {
        Route::middleware('api')
             ->namespace($this->namespace)
             ->group(base_path('routes/api.php'));
    }
```


# API AUTH
- https://www.youtube.com/watch?v=pW2SC0WFmVY

# Auth
- php artisan make:auth, maka login register forgot pasword akan otomatis jadi
- coba akses  http://localhost:9000/project-latihan/public/halamanKita/1 , cek di routes/web.php pada halaman tersebut method Showhalaman kena auth jadi misal di akses dan belum login akan terlempar ke login form
- coba akses http://localhost:9000/project-latihan/public/noAuth, buka routes/web.php untuk mengetahui link ini mengarah ke controller apa, di  menu ini tidak kena auth jadi walaupun tanpa login bisa diakses.

# macam-macam error
- Laravel 5.4: Specified key was too long error : https://laravel-news.com/laravel-5-4-key-too-long-error

# REF.
- https://laravel.com/docs/5.4/views#passing-data-to-views

# belajar API
- https://www.youtube.com/watch?v=4pc6cgisbKE

# Eksekusi laravel
# cara koneksi ke psotgree
- php artisan serve

# cara koneksi ke psotgree
- setting database di .ENV
- buka php.ini cari dengan keyword pgsql, aktifkan semuanya, ulangi perintah php artisan serve

# install app composer (like 'npm install xx --save')
```
composer require doctrine/dbal
```
hasil instalasi masuk di folder <b>vendor</b>

# Reinstall semua data yang ada di composer.json
Biasanya dilakukan bila folder vendor masuk ke gitignore, maka sesudah pull ketik perintah berikut :
```
composer update 
```


# cari tau
baseurl
uri segment
.htaccess
pagination
