# Laravel Passport - Customized for Windows IIS Support 

[![Build Status](https://travis-ci.org/laravel/passport.svg)](https://travis-ci.org/laravel/passport)
[![Total Downloads](https://poser.pugx.org/laravel/passport/d/total.svg)](https://packagist.org/packages/laravel/passport)
[![Latest Stable Version](https://poser.pugx.org/laravel/passport/v/stable.svg)](https://packagist.org/packages/laravel/passport)
[![Latest Unstable Version](https://poser.pugx.org/laravel/passport/v/unstable.svg)](https://packagist.org/packages/laravel/passport)
[![License](https://poser.pugx.org/laravel/passport/license.svg)](https://packagist.org/packages/laravel/passport)

## Installation
```
   composer require jeremykenedy/passportiis
``` 
[packagist](https://packagist.org/packages/jeremykenedy/passportiis)

## File Modifications Made for IIS Support
1.  **File Modified:**
		```2016_06_01_000002_create_oauth_access_tokens_table.php```

	**Modification Description:**
		Add intremental primary key to table.
        
    **Modification Reason:**
        For other projects middlware to associate other relational data and better data structure.

2.  **Files Modified:**
		```Passport.php``` and ```PassportServiceProvider.php```

    **Modification Description:**
    	Updated all ```P100Y``` instances to ```P1Y``` to support Windows IIS.

    **Modification Reason:**
        Corrects date of token expiration on creation, otherwise it will expire at the same time is is created.

3.  **File Modified:**
		```PersonalAccessTokens.vue```

    **Modification Description:**
    	Added form fields and updated data() fields and store() function to handle fields:

	```
         api_client_id
		 api_application_id
		 api_token_type
    ```

    **Modification Reason:**
         For relational data.

4.  **File Modified:**
		```PersonalAccessTokenController.php```

    **Modification Description:**
    	Updated store() method to build token Relationship before returning token.
        
    **Modification Reason:**
        Create token relationship to carry additional data and join ```oauth_access_tokens``` table to ```relationship``` table.

5.  **File Modified:**
		```TokenGuard.php```

    **Modification Description:**
    	Updated validCsrf() method to support Windows IIS.
        
    **Modification Reason:**
        Better cfsr handling.

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
   -- WORKING / NEED TO ADD TO REPO --

3. Create Relationship Migration
   -- WORKING / NEED TO ADD TO REPO --
   
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
  
    Take Note: If a migration fails, you rollback changes, and a table still remains in the database, delete it manually then run a migration.
    
###### To Regenerate token issue certificates
  ```
      php artisan passport:install
  ```
  
###### To clean and rebuild the cache
  ```
      php artisan config:cache
  ```
  
    Take Important Note: This will in-validate any previously issued tokens.      
    
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
