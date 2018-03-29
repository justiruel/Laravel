pasang diatas controller
```
/**
 * @SWG\Swagger(
 *   basePath="/api",
 *   @SWG\Info(
 *     title="Uji Coba Swagger",
 *     version="1.0.0"
 *   )
 * )
 */
```

pasang diatas method
```
 /**
     * @SWG\Get(
     *   path="/customer/{customerId}/rate",
     *   summary="List customer rates",
     *   operationId="getCustomerRates",
     * @SWG\Parameter(
     *     name="Authorization",
     *     in="header",
     *     description="Authorization",
     *     type="string",
     *   ),
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
     *   @SWG\Response(response=200, 
     *     description="successful operation",
     *     @SWG\Schema(
     *          title="halo",
     *          type="array",
     *          @SWG\Items(
     *              title="data",
     *              type="object",
     *              @SWG\Property(property="user_id", type="string", description=""),
     *          )
     *     )
     *   ),
     *   @SWG\Response(response=406, description="not acceptable"),
     *   @SWG\Response(response=500, description="internal server error")
     * )
     *
     */
```
