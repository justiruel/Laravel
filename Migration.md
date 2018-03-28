## Rollback Migration
Me-rollback migrasi terakhir, artinya misal dalam migrasi add drop column maka drop column akan kembali eksis begitu juga sebaliknya
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
}
```
