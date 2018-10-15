# Swagger without using annotation
- pada dasarnya swagger menggenerate json dan param request tiap method API dari formRequest yang dipakai di tiap method
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
$this->docs['paths'][$this->uri][$this->method] = [
    'description' => "$methodDescription {$this->uri}",
    ......
    'tags'=>[isset($this->route->action['middleware'][1])?$this->route->action['middleware'][1]:$this->route->action['middleware'][0]] //untuk menangkap middleware ke 1 dijadikan tag, jika index 1 tidak ada maka gunakan middleware index ke 0
];

protected function generatePath()
{
    .....
    ** $controllers = isset($this->route->action["controller"])?$this->route->action["controller"]:"-";
    $controllerWithMethod = explode("\\",$controllers);
    $controllerWithMethod = end($controllerWithMethod);
    $controllerWithMethod = explode("@",$controllerWithMethod); **

    $this->docs['paths'][$this->uri][$this->method] = [
        'description' => "$methodDescription {$this->uri}",
        'responses' => [
            '200' => [
                'description' => 'OK'
            ]
        ],
        __ 'tags'=>[$controllerWithMethod[0]] __
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
        throw new HttpResponseException(response()->json(['errors' => $errors
        ], JsonResponse::HTTP_UNPROCESSABLE_ENTITY));
    }
}
````````
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
- buka app/Exceptions/Handler.php, tambahkan
```````
    public function render($request, Exception $e)
    {
        if ($request->ajax() || $request->wantsJson())
        {
            $json = [
                'success' => false,
                'error' => [
                    'code' => $e->getCode(),
                    'message' => $e->getMessage(),
                ],
            ];
            return response()->json($json, 400);
        }
        return parent::render($request, $e);
    }
````````
method ini dipakai untuk redirect jika pada app/http/requests/CobaRequest method failedValidation, dijalankan (rule tidak terpenuhi)
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
