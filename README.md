# Laravel & Lumen API Query Builder Package

composer require gfcd/laravel-query-builder-api

### Intro

Laravel library based on [selahattinunlu library](https://github.com/selahattinunlu/laravel-api-query-builder.)

This library added some modifications from selahattinunlu library.

### Functionalities Added

1. It was added the FILTER parameter on query string, so the columns are encapsulated inside a filter property.

```
/api/users?filter={name=se*,age!=18}&order_by=age,asc&limit=2&columns=name,age,city_id&includes=city
```

2. On QueryBuilder.php, a new method was inserted. This method is called by applyDefaultRelationships(), and receive as parameter an array of model's relationships. This will do eager load.

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

    private $relationshipMethods = ["addressList"];

    public function index(Request $request)
    {
        $queryBuilder = new QueryBuilder(new User, $request);
    
        return response()->json([
            'data' => $queryBuilder
                      ->applyDefaultRelationships($this->relationshipMethods)
                      ->build()
                      ->paginate()
        ]);
    }
}
```

3. On QueryBuilder.php, a new method was inserted. This method is called by $applyDefaultFilters, and receive as parameter an array of three properties:

```

class UserController {

    //eq: you can ommit operator, by default is "=" 

    private $extraParameters = [
        [
            "column" => "name of column that you want add on server side, not on client side."
            "operator" => "the operator that you want use."
            "value" => "the value you want insert".
        ]
    ]

    public function index(Request $request)
    {
        $queryBuilder = new QueryBuilder(new User, $request);
    
        return response()->json([
            'data' => $queryBuilder
                      ->applyDefaultFilters($this->extraParameters)
                      ->build()
                      ->paginate()
        ]);
    }
}
```

This is helpful in cases where you have a project that uses User Auth, and you want to retrieve data from database based on user_id or whatever column that 
you want to add on query.


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

Others configurations can be followed using [Unlu/laravel-api-query-builder](https://github.com/selahattinunlu/laravel-api-query-builder), just changing the 'Unlu' for 'Gfcd'.
