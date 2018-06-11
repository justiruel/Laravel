# Rollback Migration
Me-rollback migrasi terakhir, ketika menjalankan perintah rollback, sejatinya kita mengeksekusi function down() pada file migration yang dituju
```
php artisan migrate:rollback
```

## Edit table add field
```
public function up()
    {
        Schema::table('anggota', function (Blueprint $table) {
            //Drop Column/field
            //$table->dropColumn('email');
            
            //Add Column/field
            $table->string('votes')->nullable(); 
        });
        
        public function down()
    {
        Schema::table('anggota', function (Blueprint $table) {
            $table->dropColumn('votes');
        });
    }
}
```

# Foreign key terjadi error

jika primary key berupa ->increment()
```
Schema::create('users', function (Blueprint $table) {
$table->increments('id');
...
```

maka pada foreignkey dibuat ->unsigned()
```
Schema::create('phones', function (Blueprint $table) {
    ...
    $table->integer('user_id')->unsigned(); //base on <model_name>_id
    ...
});
        
Schema::table('phones', function (Blueprint $table) {
     $table->foreign('user_id')->references('id')->on('users')->onDelete('cascade');  //atau ->onDelete('restrict');
});
```
agar tipenya sama dan foreign key berhasil


## intinya : antara primary key dan foreign key harus memiliki detail tipe yang sama.

# Menghapus Foreign key 
```
$table->dropForeign('phones_user_id_foreign');
```
## format foreign key 
```
<nama table>_<nama field>_foreign
```
atau untuk memastikan cek aja di database




# istilah umum :
- signed = dapat bernilai positif dan negati
- unsigned = tidak dapat bernilai negatif tapi memiliki range lebih besar
- cascade = jika record pada primary_key di hapus maka semua record pada table foreign_key ikut terhapus
- restrict = record pada primary_key tidak dapat dihapus selama masih dipakai di foreign_key



# Perintah paling sering digunakan 
- php artisan make:migration create_users_table
- php artisan migrate 
- php artisan migrate:rollback
