# Laravel Passport Customized for Windows IIS Support - v1.3.0

[![Total Downloads](https://poser.pugx.org/jeremykenedy/passportiis/d/total.svg)](https://packagist.org/packages/jeremykenedy/passportiis)
[![Latest Stable Version](https://poser.pugx.org/jeremykenedy/passportiis/v/stable.svg)](https://packagist.org/packages/jeremykenedy/passportiis)
[![License](https://poser.pugx.org/jeremykenedy/passportiis/license.svg)](https://packagist.org/packages/jeremykenedy/passportiis)

## Installation
```
   composer require jeremykenedy/passportiis
```
[packagist](https://packagist.org/packages/jeremykenedy/passportiis)

## File Modifications Made for IIS Support
1.  **File Modified:**
		[```2016_06_01_000002_create_oauth_access_tokens_table.php```](https://github.com/jeremykenedy/passportiis/blob/master/database/migrations/2016_06_01_000002_create_oauth_access_tokens_table.php)

	**Modification Description:**
		Add incremental primary key to table. Lines 17-18

    **Modification Reason:**
        For other projects middlware to associate other relational data and better data structure.

2.  **Files Modified:**
		[```Passport.php```](https://github.com/jeremykenedy/passportiis/blob/master/src/Passport.php) and [```PassportServiceProvider.php```](https://github.com/jeremykenedy/passportiis/blob/master/src/PassportServiceProvider.php)

    **Modification Description:**
    	Updated all ```P100Y``` instances to ```P1Y``` to support Windows IIS. [```Passport.php```](https://github.com/jeremykenedy/passportiis/blob/master/src/Passport.php) lines 188 and 207, [```PassportServiceProvider.php```](https://github.com/jeremykenedy/passportiis/blob/master/src/PassportServiceProvider.php) Line 107

    **Modification Reason:**
        Corrects date of token expiration on creation, otherwise it will expire at the same time is is created.

3.  **File Modified:**
		[```PersonalAccessTokens.vue```](https://github.com/jeremykenedy/passportiis/blob/master/resources/assets/js/components/PersonalAccessTokens.vue)

    **Modification Description:**
    	Added form fields, updated data() fields and store() function to handle fields, and added minor styling. Lines 9-19, 51-55, 76-87, 149-174, 254-256, and 327-329

    ```
        api_client_id
        api_application_id
        api_token_type
    ```
    **Modification Description:**
         For relational data.

4.  **File Modified:**
		[```PersonalAccessTokenController.php```](https://github.com/jeremykenedy/passportiis/blob/master/src/Http/Controllers/PersonalAccessTokenController.php)

    **Modification Description:**
    	Updated store() method to build token Relationship before returning token. Lines 6, and 67-79

    **Modification Reason:**
        Create token relationship to carry additional data and join ```oauth_access_tokens``` table to ```relationship``` table.

5.  **File Modified:**
		[```TokenGuard.php```](https://github.com/jeremykenedy/passportiis/blob/master/src/Guards/TokenGuard.php)

    **Modification Description:**
    	Updated validCsrf() method to support Windows IIS. Lines 203-209

    **Modification Reason:**
        Better cfsr handling.

6.  **File Modified:**
        [```AccessTokenRepository.php```](https://github.com/jeremykenedy/passportiis/blob/master/src/Bridge/AccessTokenRepository.php)

    **Modification Description:**
    	Updated to match Laravel/Passport master.

    **Modification Reason:**
        Fix $database not found issue in file AccessTokenRepository.php on api call. Lines 61-74

## Additional Requirements

1. 	Modify the `Kernel.php`

	In order for scoped routes to work you will need update
	your `api` with `auth:api` inside `middlewaregroups` in `app\Http\kernel.php`.
	It should look like the following:

	```
		'api' => [
			'throttle:60,1',
			'bindings',
			'auth:api',
		],
	```

2. Create Relationship Model
  Command:
  ```php artisan make:model Relationship```

  Model Contents:
  ```
    <?php

    namespace App;

    use Illuminate\Database\Eloquent\Model;

    class Relationship extends Model
    {
        /**
         * The table associated with the model.
         *
         * @var string
         */
        protected $table    = 'application_token_relationships';
        protected $connection   = 'sqlsrv';
        public $timestamps    = false;

      /**
       * Fillable fields for a Profile
       *
       * @var array
       */
      protected $fillable = [
        'token_key',
        'api_token_type',
        'api_client_id',
        'api_application_id'
      ];

    }

  ```

3. Create Relationship Migration
  Command:
    ```php artisan make:migration application_relationships```

  Migration Content:

  ```
    <?php

    use Illuminate\Support\Facades\Schema;
    use Illuminate\Database\Schema\Blueprint;
    use Illuminate\Database\Migrations\Migration;

    class ApplicationRelationships extends Migration
    {
        /**
         * Run the migrations.
         *
         * @return void
         */
        public function up()
        {
            Schema::create('application_token_relationships', function (Blueprint $table) {

                $table->increments('id');
                $table->integer('token_key')->unsigned();
                $table->foreign('token_key')->references('primary_key')->on('oauth_access_tokens')->onDelete('cascade');
                $table->enum('api_token_type', ['uber', 'dev']);
                $table->string('api_client_id');
                $table->string('api_application_id');

            });
        }

        /**
         * Reverse the migrations.
         *
         * @return void
         */
        public function down()
        {
            Schema::drop('application_token_relationships');
        }
    }


  ```

## Helpful Commands
###### To publish the Passport Vue components
  ```
      php artisan vendor:publish --tag=passport-components
  ```

###### To Migrate Database
  ```
      php artisan migrate
  ```

###### To Rollback a migration
  ```
      php artisan migrate:rollback
  ```

###### To Reset the Migration table and Rollback migrations
  ```
      php artisan migrate:reset
  ```
**Take Note:** If a migration fails, you rollback changes, and a table still remains in the database, ***delete it manually*** then run a migration.

###### To Regenerate token issue certificates
  ```
      php artisan passport:install
  ```

###### To clean and rebuild the cache
  ```
      php artisan config:cache
  ```
**Take Important Note:** This will ***in-validate*** any previously issued tokens.

###### To install assets and update autoloader
  ```
      composer update
  ```

###### To recompile vue assets
  ```
      gulp
  ```

## References Used:

- https://laracasts.com/discuss/channels/laravel/laravel-53-passport-api-unauthenticated-in-postman-using-personal-access-tokens
- https://github.com/laravel/passport/issues/47

---

## Introduction

Laravel Passport is an OAuth2 server and API authentication package that is simple and enjoyable to use.

## Official Documentation

Documentation for Passport can be found on the [Laravel website](http://laravel.com/docs/master/passport).

## License

Laravel Passport is open-sourced software licensed under the [MIT license](http://opensource.org/licenses/MIT)
