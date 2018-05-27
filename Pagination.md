# Pagination

controller
```
 public function pagination()
    {
        $users = DB::table('anggota')->paginate(3);
        //$data['success'] = "aselole";
        //$data['data'] = $users;
        //return $data;
        return view('paginate', ['users' => $users]);
    }
```

layout
```
<div class="container">
    @foreach ($users as $user)
        {{ $user->nama }} <br/>
    @endforeach
</div>
{{ $users->links() }}
```

$users->links() akan menggenerate paging view berbasis bootstrap, jika ingin custom paging view berbasis selain bootstrap

- php artisan vendor:publish --tag=laravel-pagination
- buka resources/views/vendor/pagination/bootstrap-4.blade.php
- custom (contoh: rubah class name dll.) sesuai kebutuhan

## ref : https://laravel.com/docs/5.6/pagination
