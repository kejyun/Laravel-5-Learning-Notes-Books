# Laravel 5 認證支援 Laravel 4 Session

## Step1. Laravel 4 加密方式支援 AES

Laravel 4 因為加密方式支援 `rijndael-128`，不支援 `AES`，所以無法使用 `php 7.1` 執行 Laravel 4 專案，在 `Laravel 4` 中的 `app/config/app.php` 檔案可以看到金鑰及加密方式的設定：

```php
<?php
// Laravel 4
// app/config/app.php
return [
    'key' => 'randomStringLength32xxxxxxxxxxxx'
    'cipher' => MCRYPT_RIJNDAEL_128,
];
```

*Laravel 4 安裝 [tomgrohl/laravel4-php71-encrypter](https://github.com/tomgrohl/laravel4-php71-encrypter) 套件支援 AES 加密*

```
composer require tomgrohl/laravel4-php71-encrypter
```

*修改 Nginx 設定使用 PHP 7.1*

```
server {
    root "/home/kejyun/web/laravel-4-project/public";

    location ~ [^/]\.php(/|$) {
        fastcgi_pass unix:/run/php/php7.1-fpm.sock;
    }
}
```

*修改 Laravel 4 加密方式支援 AES-256*

`AES-256` 金鑰需要長度 `32` 的字串，`AES-128` 需要長度 `16` 的字串，我們支援 `AES-256` 所以需要將金鑰長度設定為 `32`

```php
<?php
// Laravel 4
// app/config/app.php
return [
    'key' => 'randomStringLength32xxxxxxxxxxxx'
    'cipher' => 'AES-256-CBC',
];
```

## Step2. 修改 Laravel 5 金鑰格式

在使用 `php artisan key:generate` 產生 `APP_KEY` 時會產生 base64 的金鑰，但這樣格式的金鑰在 Laravel 4 是不支援的

```shell
# .env
APP_KEY=base64:z+ldCPR/vzxMBzZ6k+mptblu82qbeSM+kK5ZVKOUdFg=
```

所以需要將 Laravel 4 的金鑰直接貼到 `.env` 檔案，不要經過 base64 的加密

```shell
# .env
APP_KEY=randomStringLength32xxxxxxxxxxxx
```

## Step3. 修改 Laravel 5 Cookie 加密方式

由於 `Laravel 4` 會將 Cookie 的資料經過 *serialize*，但 `Laravel 5` 不會將 Cookie *serialize*，所以為了讓 `Laravel 5` 可以正常讀取 Laravel 4 的 Cookie，需要將 `Laravel 5` *serialize* 的功能打開。

在 `app/Http/Middleware/EncryptCookies.php` 檔案中可以設定靜態變數 `$serialize` 為 `true` 即可。

```php
<?php
// app/Http/Middleware/EncryptCookies.php
namespace App\Http\Middleware;

use Illuminate\Cookie\Middleware\EncryptCookies as Middleware;

class EncryptCookies extends Middleware
{
    protected static $serialize = true;
}
```

## Step4. 修改 Laravel 5 Auth 驗證方式

`Laravel 4`的驗證 `Auth` 方法與 `Laravel 5` 有些許不同，主要是在取得 `session 名稱` 的方法有點不同，在 `Laravel 4` 的 `vendor/laravel/framework/src/Illuminate/Auth/Guard.php` 可以看到 `getName()` 與 `getRecallerName()` 函式與 `Laravel 5` 不同

*Laravel 4 Guard*

> vendor/laravel/framework/src/Illuminate/Auth/Guard.php


```php
<?php namespace Illuminate\Auth;

// Laravel 4
// vendor/laravel/framework/src/Illuminate/Auth/Guard.php

use Illuminate\Cookie\CookieJar;
use Illuminate\Events\Dispatcher;
use Symfony\Component\HttpFoundation\Request;
use Illuminate\Session\Store as SessionStore;
use Symfony\Component\HttpFoundation\Response;

class Guard {
    /**
	 * Get a unique identifier for the auth session value.
	 *
	 * @return string
	 */
	public function getName()
	{
		return 'login_'.md5(get_class($this));
	}

	/**
	 * Get the name of the cookie used to store the "recaller".
	 *
	 * @return string
	 */
	public function getRecallerName()
	{
		return 'remember_'.md5(get_class($this));
	}
}
```


*Laravel 5 Session Guard*

> vendor/laravel/framework/src/Illuminate/Auth/SessionGuard.php

```php
<?php
// Laravel 5
// vendor/laravel/framework/src/Illuminate/Auth/SessionGuard.php

namespace Illuminate\Auth;

class SessionGuard implements StatefulGuard, SupportsBasicAuth
{
    /**
     * Get a unique identifier for the auth session value.
     *
     * @return string
     */
    public function getName()
    {
        return 'login_'.$this->name.'_'.sha1(static::class);
    }

    /**
     * Get the name of the cookie used to store the "recaller".
     *
     * @return string
     */
    public function getRecallerName()
    {
        return 'remember_'.$this->name.'_'.sha1(static::class);
    }
}
```

為了讓 `Laravel 5` 支援 `Laravel 4` 的 Session，則必須要將取得 `Session 名稱` 方法修改為 `Laravel 4` 的方式

`Laravel 5` 目錄建立 `app/Auth/Laravel4SessionGuard.php` 檔案，繼承原本 `Illuminate\Auth\SessionGuard` 的方法，並加入 `getName()` 及 `getRecallerName()` 方法修改為 `Laravel 4` 的方法

```php
<?php
namespace App\Auth;

use Auth;
use Illuminate\Auth\SessionGuard;
use Illuminate\Contracts\Auth\Guard;

class Laravel4SessionGuard extends SessionGuard implements Guard {
    protected $laravel4_guard_class_name = 'Illuminate\Auth\Guard';
    /**
     * Get a unique identifier for the auth session value.
     *
     * @return string
     */
    public function getName()
    {
        return 'login_'.md5($this->laravel4_guard_class_name);
    }

    /**
     * Get the name of the cookie used to store the "recaller".
     *
     * @return string
     */
    public function getRecallerName()
    {
        return 'remember_'.md5($this->laravel4_guard_class_name);
    }

    /**
     * Extend Auth for Laravel 4
     */
    public static function AuthExtend()
    {
        // vendor/laravel/framework/src/Illuminate/Auth/AuthManager.php
        // - createSessionDriver
        Auth::extend('laravel4Session', function ($app, $name, array $config) {

            $provider = Auth::createUserProvider($config['provider']);
            $guard = new Laravel4SessionGuard($name, $provider, $app['session.store']);

            // When using the remember me functionality of the authentication services we
            // will need to be set the encryption instance of the guard, which allows
            // secure, encrypted cookie values to get generated for those cookies.
            if (method_exists($guard, 'setCookieJar')) {
                $guard->setCookieJar($app['cookie']);
            }

            if (method_exists($guard, 'setDispatcher')) {
                $guard->setDispatcher($app['events']);
            }

            if (method_exists($guard, 'setRequest')) {
                $guard->setRequest($app->refresh('request', $guard, 'setRequest'));
            }

            return $guard;
        });
    }
}
```

*修改 `config/auth.php` 設定檔案*

```php
<?php

return [
    'defaults' => [
        // 'guard' => 'web',
        'guard' => 'laravel4_web',
        'passwords' => 'users',
    ],
    'guards' => [
        'laravel4_web' => [
            'driver' => 'laravel4Session',
            'provider' => 'users',
        ],
    ],
];
```

*Laravel 5 使用自訂驗證方法*

在 `Laravel 5` 中的 `app/Providers/AuthServiceProvider.php` 檔案中呼叫 `Laravel4SessionGuard::AuthExtend();` 方法，之後即可讀取到 `Laravel 4` 產生的 Session 驗證資料

```php
<?php
// Laravel 5
// app/Providers/AuthServiceProvider.php

namespace App\Providers;

use App\Auth\Laravel4SessionGuard;
use Illuminate\Support\Facades\Gate;
use Illuminate\Foundation\Support\Providers\AuthServiceProvider as ServiceProvider;

class AuthServiceProvider extends ServiceProvider
{
    /**
     * Register any authentication / authorization services.
     *
     * @return void
     */
    public function boot()
    {
        $this->registerPolicies();

        // 設定 Laravel 4 Auth
        Laravel4SessionGuard::AuthExtend();
    }
}
```


## 參考資料
* [tomgrohl/laravel4-php71-encrypter: Laravel 4.2 Encrypter for PHP 7.1+](https://github.com/tomgrohl/laravel4-php71-encrypter)
* [The only supported ciphers are AES-128-CBC and AES-256-CBC · Issue #9080 · laravel/framework](https://github.com/laravel/framework/issues/9080)
* [Laravel 5.6.30 breaks passport · Issue #795 · laravel/passport](https://github.com/laravel/passport/issues/795)
* [Authentication - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/5.8/authentication#adding-custom-guards)
* [How to Create a Custom Authentication Guard in Laravel](https://code.tutsplus.com/tutorials/how-to-create-a-custom-authentication-guard-in-laravel--cms-29667)
