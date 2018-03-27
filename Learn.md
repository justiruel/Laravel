# See Last Query
```
DB::enableQueryLog();
$anggota = Anggota::all();
dd(DB::getQueryLog()); // or print_r(DB::getQueryLog());   
```
