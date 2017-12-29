# ETag Middleware

在我們的網站若資料未變更，我們會希望告訴請求資源的使用者，本資源未修改(304 Not Modified)，所以不用重複讀取資料，這樣可以節省我們傳輸資料頻寬。

我可以用 Middleware 來達到 ETag 的效果

## 建立 ETag Middleware

> `App\Http\Middleware\ETagMiddleware.php`

```php
<?php

namespace App\Http\Middleware;

use Closure;

class ETagMiddleware {
    /**
     * Implement Etag support
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  \Closure  $next
     * @return mixed
     */
    public function handle($request, Closure $next)
    {
        // Get response
        $response = $next($request);
        // 如果是 get request
        if ($request->isMethod('get')) {
            // 產生回應內容的 etag
            $etag = md5($response->getContent());
            $requestEtag = str_replace('"', '', $request->getETags());
            // 檢查 etag 是否變更
            if($requestEtag AND ($requestEtag[0] == $etag OR $requestEtag[0] == 'W/'.$etag)) {
                // 若 etag 相同，設定表頭為資料未修改
                $response->setNotModified();
            }
            // 設定 etag
            $response->setEtag($etag);
        }
        // 傳送回應
        return $response;
    }
}
```

## 設定 Etag Middleware

> `app/Http/Kernel.php`

```php
<?php

namespace App\Http;

use Illuminate\Foundation\Http\Kernel as HttpKernel;

class Kernel extends HttpKernel
{
    protected $middlewareGroups = [
        'web' => [
            \App\Http\Middleware\ETagMiddleware::class,
        ],
    ];
```

這樣我們就完成 Etag Middleware 的設定了!


## 參考資料
* [Setting Etags in Laravel 5 - Matthew Daly's Blog](http://matthewdaly.co.uk/blog/2015/06/14/setting-etags-in-laravel-5/)



!INCLUDE "../../kejyun/book/laravel-5-for-beginner.md"
