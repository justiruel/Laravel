## lazy loaded
Jika kita menggunakan $posts =  Post::all(); :
```
$posts =  Post::all(); 
foreach ($posts as $post) {
    $campur->print_r($post->judul);
    foreach ($post->comment as $comment) {
        $campur->print_r($comment->komentar);
    }
    
}
```
maka querynya :
```
$hasil = select * from `posts`
"select * from `comments` where `comments`.`post_id` = $hasil[0] and `comments`.`post_id` is not null"
"select * from `comments` where `comments`.`post_id` = $hasil[1] and `comments`.`post_id` is not null"
....
dan seterusnya tergantung seberapa banyak row dari $hasil
```

## Eager loaded
Jika kita menggunakan $posts = Post::with('comment')->get(); ket: 'commment' merupakan function yang ada di Model Post
```
$posts = Post::with('comment')->get();
foreach ($posts as $post) {
    $campur->print_r($post->judul);
    foreach ($post->comment as $comment) {
        $campur->print_r($comment->komentar);
    } 
}
```
maka querynya :
```
$hasil = select * from `posts`
select * from `comments` where `comments`.`post_id` in ($hasil[0], $hasil[1]);
```
## KESIMPULAN
Eager load lebih hemat query dari pada lazy loaded, tapi setiap cara pasti memiliki 'best use' masing-masing

## CARA MELIHAT QUERY APA SAJA YANG SUDAH DI EKSEKUSI OLEH FUNCTION
```
DB::enableQueryLog();
<your query or eloquent model>
dd(DB::getQueryLog()); // or print_r(DB::getQueryLog());
```
