# API


## throttle middleware

在 `app/Http/Kernel.php` 檔案中會看到 `api` 的 `$middlewareGroups` 有一個 `'throttle:60,1'`，這邊的意思是 **每 1 分鐘同個 ip 可以請求 60 次**，可以針對你的 API 請求需求，去限制 API 請求次數，已保護你的 API 不會被隨意無限制的存取


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
            \App\Http\Middleware\EncryptCookies::class,
            \Illuminate\Cookie\Middleware\AddQueuedCookiesToResponse::class,
            \Illuminate\Session\Middleware\StartSession::class,
            // \Illuminate\Session\Middleware\AuthenticateSession::class,
            \Illuminate\View\Middleware\ShareErrorsFromSession::class,
            \App\Http\Middleware\VerifyCsrfToken::class,
            \Illuminate\Routing\Middleware\SubstituteBindings::class,
        ],

        'api' => [
            'throttle:60,1',
            'bindings',
        ],
    ];
}
```

詳情可以參考 `Illuminate\Routing\Middleware\ThrottleRequests` 檔案

## Laravel 5.6 throttle middleware

在 Laravel 5.6 的 throttle middleware 提供動態的限制使用者存取數量，在 Middleware 可以設定 `throttle:rate_limit,1`

```php
Route:middleware('throttle:rate_limit,1')->get('/', function(){
    return 'Test Rate Limit';
});
```

然後在使用者 Model 加入 `public $rate_limit` 屬性，限制使用者最多可以存取多少次


```php
<?php

namespace App;

use Illuminate\Notifications\Notifiable;
use Illuminate\Contracts\Auth\MustVerifyEmail;
use Illuminate\Foundation\Auth\User as Authenticatable;

class User extends Authenticatable
{
    use Notifiable;
    public $rate_limit = 3;
}
```

這樣登入的使用者就可以按照設定的次數限制去限制請求次數了

> 沒有登入的使用者，將限制只能存取一次
