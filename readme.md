# Laravel Passport - Customized for Windows IIS support

[![Build Status](https://travis-ci.org/laravel/passport.svg)](https://travis-ci.org/laravel/passport)
[![Total Downloads](https://poser.pugx.org/laravel/passport/d/total.svg)](https://packagist.org/packages/laravel/passport)
[![Latest Stable Version](https://poser.pugx.org/laravel/passport/v/stable.svg)](https://packagist.org/packages/laravel/passport)
[![Latest Unstable Version](https://poser.pugx.org/laravel/passport/v/unstable.svg)](https://packagist.org/packages/laravel/passport)
[![License](https://poser.pugx.org/laravel/passport/license.svg)](https://packagist.org/packages/laravel/passport)

# Custom Passport Modifications
1.  File Modified:
		**2016_06_01_000002_create_oauth_access_tokens_table.php**

	Modification Description:
		Add intremental primary key to table.

2.  Files Modified:
		**Passport.php** and **PassportServiceProvider.php**

    Modification Description:
    	Updated all **P100Y** instances to **P1Y** to support Windows IIS.

3.  File Modified:
		**PersonalAccessTokens.vue**

    Modification Description:
    	Added form fields and updated data() fields and store() function to handle fields:

			* api_client_id
			* api_application_id
			* api_token_type

4.  File Modified:
		**PersonalAccessTokenController.php**

    Modification Description:
    	Updated store() method to build token Relationship before returning token.

5.  File Modified:
		**TokenGuard.php**

    Modification Description:
    	Updated validCsrf() method to support Windows IIS.

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
