# See Last Query
```
DB::enableQueryLog();
$anggota = Anggota::all();
dd(DB::getQueryLog()); // or print_r(DB::getQueryLog());   
```

# Get timestamp
```
use Carbon\Carbon;
...
$current_time = Carbon::now()->toDateTimeString();
```
