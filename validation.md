# Controller
```
use Illuminate\Http\Request;
use Illuminate\Contracts\Session\Session;
use Illuminate\Support\Facades\Validator;
use Illuminate\Validation\ValidationException;
.....

$email      = $request->input('email');
$password   = $request->input('password');

$request->validate([
    "email"                 => "required|email",
    "password"              => "required|min:1",
    "password_confirmation" => "same:password",
]);

if ($email == "root@gmail.com" && $password == "root"){
    \Session::put('user', 'coba');
    return redirect('/home');
}else{
    $validator = Validator::make([], []); 
    $validator->errors()->add('login_fail', 'Email or password is wrong.');
    throw new ValidationException($validator);
}
```

# View
```
@if ($errors->any())
    <div class="alert alert-danger">
        <ul>
            @foreach ($errors->all() as $error)
                <li>{{ $error }}</li>
            @endforeach
        </ul>
    </div>
@endif
```
