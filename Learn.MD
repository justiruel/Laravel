# See Last Query
DB::enableQueryLog();
$anggota = Anggota::all();
dd(DB::getQueryLog());  
