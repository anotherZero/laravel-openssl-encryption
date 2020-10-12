laravel-openssl-encryption
==========================

Laravel 4 encryption package that uses the PHP **openssl** extension.

It can replace the default Laravel 4 encryption core package that uses the PHP **mcrypt** extension.

It has been created to run Laravel 4 apps on the [Google App Engine for PHP](https://developers.google.com/appengine/docs/php/) platform that currently (may 2013) does not support the mcrypt extension.

Installation
------------
Add the anotherzero/laravel-openssl-encryption package to your composer.json file.

    "require": {
    	"laravel/framework": "4.0.*",
    	"anotherzero/laravel-openssl-encryption-4x": "1.0.*"
    },

Install the package.

    $ php composer.phar install

In the `app/config/app.php` file, register the `LaravelOpensslEncryptionServiceProvider` and comment the default `EncryptionServiceProvider`.

    'providers' => array(
    
    	...
    	//'Illuminate\Encryption\EncryptionServiceProvider',
    	'Neoxia\LaravelOpensslEncryption\LaravelOpensslEncryptionServiceProvider',
    	...

One more thing ...

Currently, Laravel 4 checks if the PHP **mcrypt** extension is loaded and die if it is not !  
So, to complete the installation, we have to bypass this check. Add the following code to your `composer.json` file to let the `post-install-cmd` handle this:
```json
"scripts": {
	"post-install-cmd": [
		"sed -i.bak 's/MCRYPT_RIJNDAEL_128/null/' vendor/laravel/framework/src/Illuminate/Encryption/Encrypter.php",
		"sed -i.bak 's/MCRYPT_MODE_CBC/null/' vendor/laravel/framework/src/Illuminate/Encryption/Encrypter.php",
		"sed -i.bak 's/echo/\\/\\/echo/' vendor/laravel/framework/src/Illuminate/Foundation/start.php",
		"sed -i.bak 's/exit/\\/\\/exit/' vendor/laravel/framework/src/Illuminate/Foundation/start.php",
		"php artisan optimize"
	],
	...
},
```