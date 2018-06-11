# cara membuat helper

## Buat file app\Helpers\Helper.php
 
```
<?php // Code within app\Helpers\Helper.php

namespace App\Helpers;

class Helper
{
    public static function shout(string $string)
    {
        return strtoupper($string);
    }
}
```

## Tambahkan alias agar memangggilnya cukup (use Helper;) tidak perlu (use App\Helpers\Helper;)
```
<?php // Code within config/app.php

    'aliases' => [
     ...
        'Helper' => App\Helpers\Helper::class,
     ...
```

## Digunakan di blade
```
<!-- Code within resources/views/template.blade.php -->

{!! Helper::shout('this is how to use autoloading correctly!!') !!}
```

## Digunakan di Controller
```
<?php // Code within app/Http/Controllers/SomeController.php

namespace App\Http\Controllers;

use Helper;

class SomeController extends Controller
{

    public function __construct()
    {
        Helper::shout('now i\'m using my helper class in a controller!!');
    }
    ...
```

# Catatan
- public static function shout(string $string) --> tipe data tidak waajib ditulis --> public static function shout($string) 
- helper function tidak static :
```
pp\Helpers\Helper.php
public  function shout(string $string)
```
jika tidak static maka cara aksesnya di controller agak beda 
```
<?php // Code within app/Http/Controllers/SomeController.php

namespace App\Http\Controllers;

use Helper as Helper;

class SomeController extends Controller
{
    public function __construct()
    {
        $helper = new Helper();
        $helper::shout('now i\'m using my helper class in a controller!!');
    }
    ...
```
perbedaannya :
- use Helper as Helper;
- $helper = new Helper();
- $helper::shout('now i\'m using my helper class in a controller!!');

