# 回應 Response


## 強制回應 JSON

建立 Middleware


```php
namespace App\Http\Middleware;

use Closure;

class ForceJsonMiddleware
{
    /**
     * Handle an incoming request.
     *
     * @param  \Illuminate\Http\Request $request
     * @param  \Closure $next
     * @return mixed
     */
    public function handle($request, Closure $next)
    {
        $request->headers->set('accept', 'application/json');

        return $next($request);
    }
}
```

在 `Kernel.php` 加入此 Middleware

```php
'force-json-response' => \App\Http\Middleware\ForceJsonMiddleware::class,
```

設定使用 Middleware

```php
Route::group(['middleware' => ['force-json-response', 'auth:api']], function () {

});
```

## 參考資料
* [Laravel 响应：永远返回 JSON 响应 | Laravel China 社区](https://learnku.com/laravel/wikis/16069#7ade84)
