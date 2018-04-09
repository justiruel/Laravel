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

# Encryption
### Saat register password di ecrypt
```
bcrypt($new_password)
```
### untuk mengecek password yang sudah di encrypt (dengan bcrypt) di database 
```
if (password_verify($old_password,$user_customer->password)){
    DB::table('user_customer')
        ->where('email', $email)
        ->update(['password' => bcrypt($new_password)]);

    return response()->json([
        "success"=>true,
        "message"=>"Successfully updated password"
    ]);
}else{
    return response()->json([
        "success"=>false,
        "message"=>"Old Password is Wrong"
    ]); 
}
```
Catatan : $user_customer adalah hasil query di database
