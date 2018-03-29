# 1
```
composer require darkaonline/l5-swagger
```
# 2
You need to register the L5SwaggerService provider in config/app.php using the line 
```
L5Swagger\L5SwaggerServiceProvider::class
```
# 3
```
php artisan l5-swagger:publish
```
# 4.  Letakkan comment berikut di atas controller
```
/**
 * @SWG\Swagger(
 *   basePath="/calculate-rates",
 *   @SWG\Info(
 *     title="Customer rate calculator API",
 *     version="1.0.0"
 *   )
 * )
 */
class UserController extends Controller
{
```
# 5. Letakkan comment berikut diatas method
```
/**
 * @SWG\Get(
 *   path="/customer/{customerId}/rate",
 *   summary="List customer rates",
 *   operationId="getCustomerRates",
 *   @SWG\Parameter(
 *     name="customerId",
 *     in="path",
 *     description="Target customer.",
 *     required=true,
 *     type="integer"
 *   ),
 *   @SWG\Parameter(
 *     name="filter",
 *     in="query",
 *     description="Filter results based on query string value.",
 *     required=false,
 *     enum={"active", "expired", "scheduled"},
 *     type="string"
 *   ),
 *   @SWG\Response(response=200, description="successful operation"),
 *   @SWG\Response(response=406, description="not acceptable"),
 *   @SWG\Response(response=500, description="internal server error")
 * )
 *
 */
    public function login(){
```
# 6. Tambah kedalam .ENV
```
L5_SWAGGER_GENERATE_ALWAYS=true
```
or
```
php artisan l5-swagger:generate
```
# 7. Eksekusi
```
 http://service-url/api/documentation 
```
