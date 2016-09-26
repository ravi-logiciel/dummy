# JWT Auth #

A JSON Web Token, or JWT, is used to send information that can be verified and trusted by means of a digital signature. It comprises a compact and URL-safe JSON object, which is cryptographically signed to verify its authenticity, and which can also be encrypted if the payload contains sensitive information.

Because of it’s compact structure, JWT is usually used in HTTP Authorization headers or URL query parameters.

**We use 0.5.* version of JWT.**

To configure JWT Auth on your system , please go to https://github.com/tymondesigns/jwt-auth/wiki and follow the instructions.

**Note:** Please put full address of  'users' table model in jwt.php (in config).
```
    /*
    |--------------------------------------------------------------------------
    | User Model namespace
    |--------------------------------------------------------------------------
    |
    | Specify the full namespace to your User model.
    | e.g. 'Acme\Entities\User'
    |
    */

    'user' => 'App\Domains\User\UserEloquent',
```

**To access routes with jwt auth, please put the routes within 'jwt.auth' middleware.**
```
// group all the routes inside auth middleware.
Route::group(['middleware' => ['jwt.auth']], function() {
	//put here routes
});
```
###**.env file elements**###
JWT_SECRET={sercret_token_string}
JWT_BLACKLIST_ENABLED=true

###We can pass token in headers or directly in url.####
####In Headers####
|Name |Value
---| ---|
|Authorization | bearer (token}

#### In Url####
|Name |Value
---| ---|
|token | (token}

### Routes that are accessed with JWT auth: ###
##Login ##

#### Endpoint:
POST -  /login

#### Request Param:
|Name |Value
---| ---|
|email |(required) user account email|
|password |(required) user account password

#### Response:
Body
```
{
  "status": true,
  "message": "User has been logged in successfully.",
  "data": {
    "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOjEwLCJpc3MiOiJodHRwOlwvXC9sb2NhbGhvc3Q6ODA4MFwvYXBpXC92MVwvbG9naW4iLCJpYXQiOjE0NzQ4NzExNTAsImV4cCI6MTQ3NDg3NDc1MCwibmJmIjoxNDc0ODcxMTUwLCJqdGkiOiIxMzRkZDYxMzdiYzdhMzBiN2E4YzAyZGI3OTRjMzM2NSJ9.7wotMWXjXgetowVv1CBuxe9NSCx318-PI2rhhsRc_Y8",
    "user": {
      "id": 10,
      "first_name": "Name",
      "last_name": "Klein",
      "email": "lwhite@denesik.com",
      "auth_type": 1,
      "team_id": 2,
      "created_at": "2016-09-22 15:45:32",
      "updated_at": "2016-09-22 15:45:32",
      "deleted_at": null,
      "status": 1
    }
  }
}
```

## Logout ##

#### Endpoint:
GET -  /logout

#### Request Param in Headers:
|Name |Value
---| ---|
|Authorization | bearer (token}

#### Response:
Body
```
{
  "status": true,
  "message": "User has been logged out successfully.",
  "data": []
}
```


# Elastic Search #
Elasticsearch is a search engine based on Lucene. It provides a distributed, multitenant-capable full-text search engine with an HTTP web interface and schema-free JSON documents. Elasticsearch is developed in Java and is released as open source under the terms of the Apache License. Elasticsearch is the most popular enterprise search engine followed by Apache Solr, also based on Lucene.

**Note: Java and Java version of Elastic Search must install on your system to use Elastic Search.**

To install them, please go to https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-elasticsearch-on-ubuntu-14-04 and follow the instructions.

**We use  'Elasticquent' package in laravel for elastic search.**

Elasticquent makes working with Elasticsearch and Eloquent models easier by mapping them to Elasticsearch types. You can use the default settings or define how Elasticsearch should index and search your Eloquent models right in the model.

To install this package, go to https://github.com/elasticquent/Elasticquent and follow the instructions

Set Elsatic Search custom host:
```
    /*
    |--------------------------------------------------------------------------
    | Custom Elasticsearch Client Configuration
    |--------------------------------------------------------------------------
    |
    | This array will be passed to the Elasticsearch client.
    | See configuration options here:
    |
    | http://www.elasticsearch.org/guide/en/elasticsearch/client/php-api/current/_configuration.html
    */

    'config' => [
        'hosts'     => explode('|',env('ELASTIC_HOSTS', '172.0.0.1:9200')),
        'retries'   => 1,
    ],

```

Set ELastic Search  custom index name:
```
    /*
    |--------------------------------------------------------------------------
    | Default Index Name
    |--------------------------------------------------------------------------
    |
    | This is the index name that Elasticquent will use for all
    | Elasticquent models.
    */

    'default_index' => env('ELASTIC_INDEX', 'my_custom_index'),
```
###**.env file elements**###
ELASTIC_HOSTS=172.17.0.3:9200|172.17.0.3:9200
ELASTIC_INDEX=index_name

###  Elastic Search Re-Indexing ###

#### Endpoint:
GET -  /search/reindex

#### Request Param:
|Name |Value
---| ---|
|includes |(arary), array of table names that you want to re-index |

#### Response:
Body
```
{
  "status": true,
  "message": "Success",
  "data": "Rebuilt indexes."
}
```

###  Searching With Elastic Search ###

#### Endpoint:
GET -  /search

#### Request Param:
|Name |Value
---| ---|
|q | text that you want to search
|page['limit'] | limit to get records (default:10)|
|include | table names from which you want to search data (Example: persons,tasks,deals)
|fields[]| field names of table that  you want to search data (Example:fields[persons]=first_name,last_name)
|sort |field name to sort records list (default is id : ASC)

#### Response:
Body
```
{
  "status": true,
  "message": "Success",
  "data": {
    "persons": [
      {
        "last_name": "Daugherty",
        "first_name": "Candace",
        "id": 14
      },
    ],
    "tasks": [],
    "deals": []
  }
}
```
