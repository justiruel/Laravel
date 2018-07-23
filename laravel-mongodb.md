# Sample App (D:\PROJECT\RISET\Laravel\laravel-mongo)
# setting laravel-mongoDB
1. http://pecl.php.net/package/mongodb/1.5.1/windows
   download yang versi 32/x86
2. ikuti petunjuk http://php.net/manual/en/mongodb.installation.windows.php
3. cara instalasi mongo : https://github.com/justiruel/others/blob/master/MONGODB.MD
4. ikuti http://www.wakamage.com/tutorial-setup-laravel-dengan-mongodb
5. buat controller baru untuk test insert 
6. pada eloquentModel 
```
use Illuminate\Foundation\Auth\User as Authenticatable;
```
rubah menjadi
```
use Jenssegers\Mongodb\Auth\User as Authenticatable;
```
