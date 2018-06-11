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

maka pada foreignkey dibuat ->unsigned();
```
Schema::table('phones', function (Blueprint $table) {
     $table->foreign('user_id')->references('id')->on('users')->onDelete('cascade');
});
```
agar tipenya sama dan foreign key berhasil

## keterangan :
- signed = dapat bernilai positif dan negati
- unsigned = tidak dapat bernilai negatif tapi memiliki range lebih besar

## intinya : antara primary key dan foreign key harus memiliki detail tipe yang sama.
