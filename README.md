# Laravel & Lumen API Query Builder Package

### English

Laravel library based on https://github.com/selahattinunlu/laravel-api-query-builder.

This library added two modifications from selahattinunlu library.

1º:

It was added the FILTER parameter on query string, so the columns are encapsulated inside a filter property.

/api/users?filter={name=se*&age!=18}&order_by=age,asc&limit=2&columns=name,age,city_id&includes=city

2º:

On QueryBuilder.php, a new property was inserted. This property is called by $extraParameters, and receive an array of three properties:

$extraParameters = [
    [
        "column" => "name of column that you want add on server side, not on client side."
        "operator" => "the operator that you want use."
        "value" => "the value you want insert".
    ]
]

This is helpful in cases where you have a project that uses User Auth, and you want to retrieve data from database based on user_id or whatever column that 
you want to add on query.

To use extraParameters, follow the steps on https://github.com/selahattinunlu/laravel-api-query-builder/wiki/9.-How-do-exclude-parameters-from-queries%3F,
changing the $excludedParameters for $extraParameters.

The $extraParameters is also included on config.php, so you can utilize it if you have a default value from a column that in every query to database you want to insert.
.
