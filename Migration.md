## Rollback Migration
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
