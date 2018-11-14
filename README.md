# catatan 
```
use App\Reports\Country\SalesByCountry; -->link by namespace not by real path
laravel 5.5 keatas harus pakai PHP >= 5.64
```

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
Hasilnya : app/Anggota.php (Model)<br/>
Bisa juga hasilnya diletakkan di folder Models caranya :
```
php artisan make:model Models/Anggota
```
Hasilnya : app/Models/Anggota.php (Model)<br/>
### Catatan : Model Anggota secara otomatis akan terkoneksi ke table anggotas, kecuali didefinisikan lain di dalam Anggota.php
# Blade
- Untuk membuat suatu view bisa di isi dengan syntaxt blade, rename view name menjadi <b>*.blade.php</b>
- .blade view akan di-render menjadi php native untuk selanjutnya di eksekusi, lokasi render ada di <b>storage/framework/views/9316a91367544223c7eb6e4c3e30a8d44b85c1d9.php</b> 
- untuk menggunakan form helper semisal :
```
{{Form::textarea('notes')}}

hasil render ->
<textarea name="notes" cols="50" row="10"></textarea>
```
lakukan perintah : 
```
composer require "illuminate/html":"5.0.*"
composer require laravelcollective/html
```
# Controller 
Generate Controller baru
```
php artisan make:controller cobaController --resource
```
ket. --resource digunakan agar controller otomatis menggenerate method index(),create(),show(), dll.

Untuk routingnya misal kita tidak ingin ribet mengetik satu2 routing untuk index(), create(), dll. kita bisa mengetikkan di routes/web.php atau routes/api.php
```
Route::resource('/home', 'HomeController');
```
catatan. untuk method yang kita tambahkan sendiri (tidak iku ke generate saat make:controller), maka kita juga harus buat routenya secara manual (tidak ikut kegenerate)


untuk mengecek list route yang sudah tercipta ketik 
```
php artisan route:list
```

# check Version
http://www.elcoderino.com/check-laravel-version/

# API
- php artisan make:resource coba   --> hasilnya akan muncul di App
- routes/api.php --> arahkan router
- contoh kasus cobaApiController
- cara akses : http://localhost:9000/project-latihan/public/api/cobaApi
- kenapa saat akses api harus ada prefix api (public/<b>API</b>/cobaApi)?, jawabannya coba buka RouteServiceProvider.php
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
- https://www.codepolitan.com/api-otentikasi-menggunakan-passport-laravel-59fc1153796b9
### Tambahan  
- Terakhir sebelum coba running di postman, ketik "php artisan make:auth"
- Ketika coba di postman saat akses http://127.0.0.1:8000/api/details jika tidak menyertakan token alias unauthorized, dia akan redirent ke halaman web login seperti ini :
```
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <!-- CSRF Token -->
        <meta name="csrf-token" content="sph4rbYcbJi9KWUPgHFaR4YvNMHsalgmrAP4LAzS">
        <title>Laravel</title>
        <!-- Scripts -->
        <script src="http://127.0.0.1:8000/js/app.js" defer></script>
```
Agar outputnya berupa json seperti : 
```
{
    "message": "Unauthenticated."
}
```
pada Header tambahkan 
```
Accept:application/json
Content-Type:application/json
Authorization:Bearer <token>
```
### Catatan
Table User dapat diganti, caranya :
- buat table customerkus
- buat eloquent model,  php artisan make:model Customerku --> model ini akan mereferensi ke table customerkus
- config/auth.php, jadikan seperti ini :
```
'providers' => [
        'users' => [
            'driver' => 'eloquent',
            'model' => App\Customerku::class,
        ],
    ],
```
- Pada register, sesuaikan eloquent model, rubah dari User jadi Customerku :
```
public function register(Request $request)
    {
        ......
        $input = $request->all();
        $input['password'] = bcrypt($input['password']);
        $user = Customerku::create($input);
        .....
```
- contoh dapat dilihat disini : D:\PROJECT\RISET\Laravel\auth-api
### jika ingin auth mereferensi ke beberapa table contoh : user dan guest table
```
https://github.com/sfelix-martins/passport-multiauth
```

### Mengatur token expired
app/providers/AuthServiceProvider.php
```
public function boot()
{
    .....
  Passport::tokensExpireIn(now()->addDays(15));
  Passport::refreshTokensExpireIn(now()->addDays(30));
}
```
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

# menghilangkan /public saat akses url
```
Rename server.php in your Laravel root folder to index.php
Copy the .htaccess file from /public directory to your Laravel root folder.
```
untuk run harus pakai : 
```
php -S localhost:8000
cara akses : http://localhost:8000/
```
tidak bisa pakai :
```
php artisan serve
```
Cara lain : taruh di dalam xampp/htdocs, cara akses http://localhost:9000/ParkingSystemWeb/

# Akses value dari .ENV

.env
```
...
TRY_ENV="iam just trying"
...
```
jalankan perintah 
```
php artisan config:cache   --> harus di eksekusi setiap memodifikasi file .env
```
controller
```
echo env('TRY_ENV','')."<br/>";
```

# Akses request parameter (URL/?name=jjj)

```
use Illuminate\Http\Request;
....
echo \Request::input('name');
atau
echo $request->input('planet');
atau
echo $request->input('planet');

```     
# Akses URI SEGMENT
```
contoh : http://localhost:8000/get_env
echo request()->segment(1);
//result : get_env
```

# BASE URL
## BY URL
```
$var = 12;
echo url("/posts/{$var}");
result : http://localhost:8000/posts/12
```
versi blade 
```
{{url('/segment')}}
result : http://localhost:8000/segment
```
## BY ROUTE NAME
Router
```
Route::get('/NamingRoute', ['uses' => 'WelcomeController@NamingRoute', 'as' => 'admin.home']);
```
Controller
```
echo route('admin.home')
result : http://localhost:8000/NamingRoute
```
versi blade
```
{{ route('admin.home') }}
result : http://localhost:8000/NamingRoute
```

# Akses data di config/app.php
config/app.php
```
'coba' => 'http://google.com'
```
Jalankan
```
php artisan config:cache   --> harus di eksekusi setiap memodifikasi file config
```
controller
```
echo config('app.coba'); atau Config::get('app.coba');
result : http://google.com
```
# DEBUG MODE
masuk .env
```
APP_DEBUG=false
```
catatan : pada production level pastikan APP_DEBUG = false, agar data sensitif tidak tampil di layar saat terjadi error

# ERROR LOG
- Buka config/logging.php disitu ada "storage_path('logs/laravel.log')" A.K.A "storage/logs/laravel.log"
- Setiap terjadi error, error message akan tercetak di laravel.log
- kamu dapat merubah path atau nama dari logging file

# Redirect jika terjadi error
- buka app/Exceptions/Handler.php
```
....
public function render($request, Exception $exception)
{
       if ($exception instanceof MaintenanceModeException){
            return parent::render($request, $exception);      
        }else{
            return response()->view('errors', [], 500);
        }
        //return parent::render($request, $exception);
}
....
```
perhatikan kode diatas, saya menambahkan 
```
return response()->view('errors', [], 500);
```
artinya jika terjadi error apaapun maka error message tidak akan muncul, sebagai gantinya halaman akan di redirect ke errors.blade.php<br/>
untuk menentukan aksi berbeda setiap jenis exception, ikuti tutorial berikut : https://scotch.io/tutorials/creating-a-laravel-404-page-using-custom-exception-handlers

#### tambahan
```
if ($exception instanceof MaintenanceModeException){
```
digunakan agar jika dalam maintenance mode ("php artisan down") aplikasi tidak ter-redirect ke  errors.blade.php, sehingga maintenance mode memiliki tampilannya sendiri

# Maintenance mode

```
php artisan down --> maintenance mode
php artisan up --> up mode
```
catatan : kita dapat membuat tampilan custom untuk maintenance mode caranya buat file : resources/views/errors/503.blade.php
isi terserah contoh :
```
@extends('errors::layout')
@section('title', 'Service Unavailable')
@section('message', 'Be right back.')
<h1 style="background-color: aqua">Asek asek</h1>
```

# html hanya dapat menerima methodc POST dan GET, untuk method lain didalam form tag tambahkan
```
<input name="_method" type="hidden" value="PUT">
<input name="_token" type="hidden" value="yeVnhaht6XSi4EpAFfUIDKpFIblwNGcB9DYkpVsZ">
atau
{{ Form::hidden('_method', 'PUT') }}
{{  Form::hidden('_token', csrf_token()) }}
```
# cari tau
- .htaccess
- Faker Generator
- send mail
- success flash message
