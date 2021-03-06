# Swagger without using annotation
- pada dasarnya swagger menggenerate json dan param request tiap method API dari formRequest yang dipakai di tiap method
- saya coba library swagger ini hanya support untuk laravel >= 5.5
## Gunakan https://github.com/mtrajano/laravel-swagger
- jalankan perintah 
``````
composer require mtrajano/laravel-swagger
``````
- buka vendor/mtrajano/laravel-swagger/src/formatters/JsonFromatter.php
``````
//return json_encode($this->docs, JSON_PRETTY_PRINT);	        
$data =  json_encode($this->docs, JSON_PRETTY_PRINT);
$myfile = fopen("./public/swagger/swagger.json", "w") or die("Unable to open file!");
fwrite($myfile, $data);
fclose($myfile);
``````
- buka vendor/mtrajano/laravel-swagger/src/generator.php
``````
protected $route;
.....
foreach ($this->getAppRoutes() as $route) {
    ......
    $this->route = $route;
}
......
$baseInfo = [
    'swagger' => '2.0',
    "securityDefinitions"=> [
        "Bearer"=> [
          "type"=> "apiKey",
          "name"=> "Authorization",
          "in"=> "header"
        ]
    ]
    .....
];
......
protected function generatePath()
{
    .....
    $controllers = isset($this->route->action["controller"])?$this->route->action["controller"]:"-";
    $controllerWithMethod = explode("\\",$controllers);
    $controllerWithMethod = end($controllerWithMethod);
    $controllerWithMethod = explode("@",$controllerWithMethod);

    $this->docs['paths'][$this->uri][$this->method] = [
        'description' => "$methodDescription {$this->uri}",
        'responses' => [
            '200' => [
                'description' => 'OK'
            ]
        ],
        'tags'=>[$controllerWithMethod[0]]
    ];
    .....
}

``````
- buka .env edit APP_URL sesuai endpoint tanpa protokol (http://)
```````
APP_URL=localhost:8000
atau
APP_URL=
```````
- jalankan ini perubahan .env
```````
php artisan config:cache
```````
- download https://github.com/swagger-api/swagger-ui
- copy isi dari folder dist ke laravel-project/public/swagger
- rubah file laravel-project/public/swagger/index.html
```````
//url: "https://petstore.swagger.io/v2/swagger.json",
url:window.location.protocol + "//" + window.location.hostname +":"+ window.location.port +"/swagger/swagger.json",
``````````
- rename laravel-project/public/swagger/index.html ------> laravel-project/public/swagger/index.php
## Membuat formRequest untuk menentukan param tiap method
```````
php artisan make:request CobaRequest
````````
- buka app/http/requests/CobaRequest, isikan
````````
<?php

namespace App\Http\Requests;

use Illuminate\Http\JsonResponse;
use Illuminate\Validation\ValidationException;
use Illuminate\Contracts\Validation\Validator;
use Illuminate\Http\Exceptions\HttpResponseException;
use Illuminate\Foundation\Http\FormRequest;

class CobaRequest extends FormRequest
{
    /**
     * Determine if the user is authorized to make this request.
     *
     * @return bool
     */
    public function authorize()
    {
        return true;
    }

    /**
     * Get the validation rules that apply to the request.
     *
     * @return array
     */
    public function rules()
    {
        return [
            'name' => 'required|max:255',
            'state' => 'required|max:255',
            'phone' => 'required|numeric'

        ];
    }

    protected function failedValidation(Validator $validator)
    {
        $errors = (new ValidationException($validator))->errors();
        $error_key =  array_keys($errors);
        $data = [];
        foreach ($error_key as $value) {
            array_push($data,$errors[$value][0]);
        }
        throw new HttpResponseException(response()->json([
            "code" => "99",
            'errors' => $data
        ], JsonResponse::HTTP_UNPROCESSABLE_ENTITY));
    }
}
````````
jika method failedValidation error gunakan :
```````
    protected function failedValidation(Validator $validator)
    {
        //$errors = (new ValidationException($validator))->errors();
        $errors = $validator->errors();
        $errors = json_decode(json_encode($errors),TRUE);
        
        $error_key =  array_keys($errors);
        $data = [];
        foreach ($error_key as $value) {
            array_push($data,$errors[$value][0]);
        }
        throw new HttpResponseException(response()->json([
            "code" => "99",
            'errors' => $data
        ], JsonResponse::HTTP_UNPROCESSABLE_ENTITY));
    }
```````


method ini dipakai untuk redirect jika pada app/http/requests/CobaRequest method failedValidation, dijalankan (rule tidak terpenuhi)
- buka controller, instance object $request dari class CobaRequest 
```````
namespace App\Http\Controllers;

use Illuminate\Http\Request;
use App\Http\Requests\CobaRequest;

class CobaController extends Controller
{
    public function index(CobaRequest $request){
        return response()->json(['name' => $request->name, 'state'=> $request->state, 'phone'=>$request->phone]); 
    }
}

````````
- jalankan : php artisan laravel-swagger:generate
- buka http://localhost:8000/swagger/
- untuk membuat agar support auth maka tambahkan kode berikut di swagger.json
`````````
"swagger": "2.0",
"securityDefinitions": {
    "Bearer": {
      "type": "apiKey",
      "name": "Authorization",
      "in": "header"
    }
  },
```````
