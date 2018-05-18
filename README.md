# Laravel & Lumen API Query Builder Package

composer require gfcd/laravel-query-builder-api

### Intro

Laravel library based on [selahattinunlu library](https://github.com/selahattinunlu/laravel-api-query-builder.)

This library added some modifications from selahattinunlu library.

### Functionalities Added

1. It was added the FILTER parameter on query string, so the columns are encapsulated inside a filter property.

```
/api/users?filter={name=se*&age!=18}&order_by=age,asc&limit=2&columns=name,age,city_id&includes=city
```

2. On QueryBuilder.php, a new property was inserted. This property is called by $extraParameters, and receive an array of model's relationships. This will do eager load.

```
class User {
    
    public function addressList()
    {
        return $this->hasMany(Address::class);
    }
}
```

```
class UserController {
     public function index(Request $request)
    {
        $queryBuilder = new QueryBuilder(new User, $request);
    
        return response()->json([
            'data' => $queryBuilder->build()->paginate()
        ]);
    }
}
```

```
class UserFilter extender QueryBuilder {
    $relationMethods = ['addressList']
}
```

3. On QueryBuilder.php, a new property was inserted. This property is called by $relationMethods, and receive an array of three properties:

```
$extraParameters = [
    [
        "column" => "name of column that you want add on server side, not on client side."
        "operator" => "the operator that you want use."
        "value" => "the value you want insert".
    ]
]
```

This is helpful in cases where you have a project that uses User Auth, and you want to retrieve data from database based on user_id or whatever column that 
you want to add on query.

To use extraParameters, follow the steps on [Unlu/laravel-api-query-builder](https://github.com/selahattinunlu/laravel-api-query-builder/wiki/9.-How-do-exclude-parameters-from-queries%3F),
changing the $excludedParameters for $extraParameters.

The $extraParameters is also included on config.php, so you can utilize it if you have a default value from a column that in every query to database you want to insert.
.

### Service Provider
Add this line into config/app.php file's providers

```
'Gfcd\Laravel\Api\ApiQueryBuilderServiceProvider'.
```

### Publish config file

If you want to change default limit, orderBy and excludedParameters parameters, run this command on the terminal:

```
php artisan vendor:publish --provider="Gfcd\Laravel\Api\ApiQueryBuilderServiceProvider"
```

### Others

All configurations can be followed using [Unlu/laravel-api-query-builder](https://github.com/selahattinunlu/laravel-api-query-builder), just changing the 'Unlu' with 'Gfcd'.
