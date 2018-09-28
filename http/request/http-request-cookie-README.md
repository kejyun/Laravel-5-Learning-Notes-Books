# Cookie


## Cookie 跨網域存取


#### 設定 Cookie Domain

在 `config/session.php` 檔案中設定 session cookie 的網域，將網址前方的子網域去除，例如 `laravel5-book.kejyun.com` 及 `about.kejyun.com` 的網域，就僅保留 `.kejyun.com` 設定就好，這樣不同子網域的 cookie 就能夠互相存取。

```php
return [
    /*
    |--------------------------------------------------------------------------
    | Session Cookie Domain
    |--------------------------------------------------------------------------
    |
    | Here you may change the domain of the cookie used to identify a session
    | in your application. This will determine which domains the cookie is
    | available to in your application. A sensible default has been set.
    |
    */

    'domain' => env('SESSION_DOMAIN', '.kejyun.com'),
];
```

#### 存取透過 javascript 設定的 cookie

##### 移除 EncryptCookies Middleware

所有透過 Laravel 設定的 Cooke 都會透過 `APP_KEY` 去進行加密，但是 JavaScript 在設定 Cookie 時，並沒有經過加密的程序，所以若 Laravel 要取得 JavaScript 設定的 Cookie，經過解密後會無法正確解析。

可以在 `app/Http/Kernel.php` 檔案中，將 `\App\Http\Middleware\EncryptCookies::class` 的 Middleware 註解，這樣就可以讓 Laravel 的 Cookie 資料不需要加解密，這樣就可以正常的取的透過 JavaScript 設定的 Cookie 了。

```php
<?php

namespace App\Http;

use Illuminate\Foundation\Http\Kernel as HttpKernel;

class Kernel extends HttpKernel
{
    /**
     * The application's route middleware groups.
     *
     * @var array
     */
    protected $middlewareGroups = [
        'web' => [
            // \App\Http\Middleware\EncryptCookies::class,
            // *** 以下省略 ***
        ],
    ];
}
```

##### 排除 Cookie 加密鍵值

在 `\App\Http\Middleware\EncryptCookies::class` 檔案中，有 `$except` 變數，可以指定哪些鍵值不要加密，設定完後，就可以順利地讀取到未加密的 Cookie 資料了！


```php
class EncryptCookies extends BaseEncrypter
{
    /**
     * The names of the cookies that should not be encrypted.
     *
     * @var array
     */
    protected $except = [
        'js_cookie_key',
    ];
}
```

## 設定 Cookie

```php
cookie_minutes = 3;
Cookie::queue('cookie_name', 'cookie_value', $cookie_minutes);
```


## 取得 Cookie

```php
Cookie::get('cookie_name');
```


## Cookie 讀取不到問題

在設定完 Cookie 後，不可以預先在回傳 response 之前就使用 `echo`、`var_dump` 把資料印出來，因為這樣會預先送 header，導致 Cookie 無法正常設定。


!INCLUDE "../../kejyun/book/laravel-5-for-beginner.md"
