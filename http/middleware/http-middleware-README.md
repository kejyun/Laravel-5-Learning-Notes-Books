# 中介層（Middleware）

這裏會介紹如何在 Laravel 5 使用中介層處理資料，Middleware 在 Laravel 4 叫做 Filter，他可以在處理資料之前，先過濾條件判斷，符合條件的再繼續處理之後的  Http 請求。

就像實作一個部落格，使用者發表文章的時候，一定要登入，否則就會被導到登入頁（或首頁），判斷登入條件的部分在 Laravel 5 可以用中介層去實現。


## 檢視中介層類別

我們可以看看內建的驗證使用者是否有登入的 Authenticate 中介層


```php
<?php namespace App\Http\Middleware;

// app\Http\Middleware\Authenticate.php

use Closure;
use Illuminate\Contracts\Auth\Guard;

class Authenticate {

    protected $auth;

    /**
     * Create a new filter instance.
     * 建立過濾器實例，建構時注入 Guard 類別並存到 auth 變數
     *
     * @param  Guard  $auth
     * @return void
     */
    public function __construct(Guard $auth)
    {
        $this->auth = $auth;
    }

    /**
     * Handle an incoming request.
     * 處理 request
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  \Closure  $next
     * @return mixed
     */
    public function handle($request, Closure $next)
    {
        if ($this->auth->guest())
        {
            if ($request->ajax())
            {
                return response('Unauthorized.', 401);
            }
            else
            {
                return redirect()->guest('auth/login');
            }
        }

        return $next($request);
    }

}

```

有設定 Http Request 中介層時，所有的請求都會丟給中介層的 `handle()` 函式做處理，第一個變數傳入的是 Request 本身，第二個變數是若檢查驗證成功之後要執行的函數，並把 Request 丟給下一層處理 `$next($request)`。

## 註冊中介層變數

中介層設定好之後，必須要到 `app\Http\Kernel.php` 去註冊你的中介層

```php
<?php namespace App\Http;

// app\Http\Kernel.php

use Illuminate\Foundation\Http\Kernel as HttpKernel;

class Kernel extends HttpKernel {

    /**
     * The application's global HTTP middleware stack.
     * 全域中介層堆疊
     *
     * @var array
     */
    protected $middleware = [
        'Illuminate\Foundation\Http\Middleware\CheckForMaintenanceMode',  // 檢查應用程式是不是維護中
        'Illuminate\Cookie\Middleware\EncryptCookies',                    // 加密 Cookies
        'Illuminate\Cookie\Middleware\AddQueuedCookiesToResponse',        // 加入 Queued Cookies 到 Response
        'Illuminate\Session\Middleware\StartSession',                     // 開啟 Session
        'Illuminate\View\Middleware\ShareErrorsFromSession',              // 從 Session 中共享錯誤資訊
        'App\Http\Middleware\VerifyCsrfToken',                            // 驗證 CSRF Token
    ];

    /**
     * The application's route middleware.
     * 路由中介層
     *
     * @var array
     */
    protected $routeMiddleware = [
        'auth' => 'App\Http\Middleware\Authenticate',
        'auth.basic' => 'Illuminate\Auth\Middleware\AuthenticateWithBasicAuth',
        'guest' => 'App\Http\Middleware\RedirectIfAuthenticated',
    ];

}

```

在 `app\Http\Kernel.php` 類別中，`$middleware` 變數是設定全域中介層堆疊清單，每一個 Http Request 都會依序經過 `$middleware` 所有的中介層做判斷

`$middleware` 中介層判斷完後都沒問題，才丟給路由中介層 `$routeMiddleware` 做處理（若路由有設定要使用哪個中介層的話，沒有設定則略過）。

當我們有自己的中介層，我們可以依自己需求看要將中介層設定加到哪一個變數設定中，如果需要每一個 Request 都做檢查的話，則將中介層設定到 `$middleware`，否則設定在。 `$routeMiddleware` 並指定中介層名稱即可。

## 在 Controller 使用中介層

我們可以強制設定，當使用者要存取文章的資源時都必須要登入，所以在 ArticleController 控制器的建構子，我們可以用 `$this->middleware('auth');` 設定全部 ArticleController 中的函式皆使用 `auth` 中介層。

> `auth` 中介層名稱是參照 `app\Http\Kernel.php` 中的 `$routeMiddleware` 變數設定


```php
class ArticleController extends Controller {

    public function __construct() {
        $this->middleware('auth');
    }

    // 新增文章
    public function store(Requests $request)
    {

    }

}
```

我們也可以使用 `only` 方式，指定中介層只有在指定的函式中才使用，或是使用 `except` 方式指定除了某些函式不使用中介層外，其他都要使用中介層當作過濾

```php
// 只有設定的函式使用中介層
$this->middleware('auth', ['only'=>'create']);

// 只有設定的函式"不要"使用中介層
$this->middleware('auth', ['except'=>'index']);
```


## 在 Route 使用中介層

```php
// app\Http\routes.php

Route::get('about', [
    'middleware'  => 'auth',
    'uses'=> 'HomeController@about'
]);
```

## 建立自己的中介層

我們可以使用指令建立自己的 Middleware，假如我建立一個 `KeJyunMiddleware` 為名稱的中介層，可以在命令列輸入：

```shell
$ php artisan make:middleware KeJyunMiddleware
```

建立的中介層會放在 `app\Http\Middleware\KeJyunMiddleware.php` 中


```php
<?php namespace App\Http\Middleware;

// app\Http\Middleware\KeJyunMiddleware.php

use Closure;

class KeJyunMiddleware {

    public function handle($request, Closure $next)
    {
        return $next($request);
    }

}
```

建立好自訂的中介層之後，到 `app\Http\Kernel.php` 註冊中介層後即可使用


```php
<?php namespace App\Http;

// app\Http\Kernel.php

class Kernel extends HttpKernel {

    protected $routeMiddleware = [
        'kejyun' => 'App\Http\Middleware\KeJyunMiddleware',
    ];

}

```

## 參考資料
* [Ogres Are Like Middleware - Laracasts](https://laracasts.com/series/laravel-5-fundamentals/episodes/16)
