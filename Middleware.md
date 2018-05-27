## Middleware akan di eksekusi sebelum controller tujuan, dengan middleware kita juga dapat memodifikasi request $_post & $_get, dll. sebelum digunakan oleh controller tujuan
- php artisan make:middleware Coba
- buka app/Http/Kernel.php, ketik :
```
protected $routeMiddleware = [
    .....
	  'coba' => \App\Http\Middleware\Coba::class,
];

```
- buka app/Http/Middleware/Coba.php, isi kode, contoh :
```
if ($request->input('name') == "ronaldo"){
	redirect('http://google.com');
}
```      
- buka router, ketik :
```
Route::group(['middleware' => 'coba'], function(){
    Route::get('/pagination','WelcomeController@pagination');
});
```
