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
