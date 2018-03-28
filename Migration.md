## Rollback Migration
Me-rollback migrasi terakhir
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
