# Laravel Passport - Customized for Windows IIS Support 

[![Build Status](https://travis-ci.org/laravel/passport.svg)](https://travis-ci.org/laravel/passport)
[![Total Downloads](https://poser.pugx.org/laravel/passport/d/total.svg)](https://packagist.org/packages/laravel/passport)
[![Latest Stable Version](https://poser.pugx.org/laravel/passport/v/stable.svg)](https://packagist.org/packages/laravel/passport)
[![Latest Unstable Version](https://poser.pugx.org/laravel/passport/v/unstable.svg)](https://packagist.org/packages/laravel/passport)
[![License](https://poser.pugx.org/laravel/passport/license.svg)](https://packagist.org/packages/laravel/passport)

## File Modifications for IIS Support
1.  File Modified:
		```2016_06_01_000002_create_oauth_access_tokens_table.php```

	Modification Description:
		Add intremental primary key to table.
        
    Modification Reason:
        For other projects middlware to associate other relational data and better data structure.

2.  Files Modified:
		```Passport.php``` and ```PassportServiceProvider.php```

    Modification Description:
    	Updated all ```P100Y``` instances to ```P1Y``` to support Windows IIS.

    Modification Reason: 
        Corrects date of token expiration on creation, otherwise it will expire at the same time is is created.

3.  File Modified:
		```PersonalAccessTokens.vue```

    Modification Description:
    	Added form fields and updated data() fields and store() function to handle fields:

	```
         api_client_id
		 api_application_id
		 api_token_type
    ```

    Modification Reason:
         For relational data.

4.  File Modified:
		```PersonalAccessTokenController.php```

    Modification Description:
    	Updated store() method to build token Relationship before returning token.

5.  File Modified:
		```TokenGuard.php```

    Modification Description:
    	Updated validCsrf() method to support Windows IIS.
        
    Modification Reason:
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
  * -- WORKING / NEED TO ADD TO REPO --
  * -- WORKING / NEED TO ADD TO REPO --
  * -- WORKING / NEED TO ADD TO REPO --
  * -- WORKING / NEED TO ADD TO REPO --
   
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
