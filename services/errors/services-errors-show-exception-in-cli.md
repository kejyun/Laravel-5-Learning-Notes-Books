# 在單元測試顯示例外（Show Exception in CLI）

在 Laravel 5 做單元測試時，使用 try catch 丟出例外時，Laravel 5 會自動地將例外的錯誤訊息處理成網頁的樣式呈現，這樣的好處是在網頁中做操作發生例外狀況時，可以直接看到例外的錯誤訊息，但是在寫單元測試 (Unit test) 時，他只會將這些錯誤先記錄在 log 檔案裡面（`storage/log/laravel-yyyy-mm-dd.log`），我們要看到這些錯誤的狀況必須要再另開終端機去執行 `php artisan tail` 去觀看這些 例外 Log 的狀況，這樣在做測試的時候是相當麻煩的。

在 Laravel 5 中所有的例外（Exception）都會被丟到 `app/Exceptions/Handler.php` 中的 `render()` 去處理

```php
<?php
// app/Exceptions/Handler.php

class Handler extends ExceptionHandler {

    /**
     * Render an exception into an HTTP response.
     * 將例外錯誤轉為 HTTP 回應
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  \Exception  $e
     * @return \Illuminate\Http\Response
     */
    public function render($request, Exception $e)
    {
        return parent::render($request, $e);
    }

}
```

我們如果需要在 CLI 就顯示例外錯誤訊息的話，必須修改 `render()` 函式，而我想要保有原本在網頁做除錯的彈性，所以我在環境變數 `.env` 檔案中加入例外處理（Exception）顯示的開關，當有設定開啟時，才直接顯示例外訊息。

```
<!-- .env -->
EXCEPTION_DISPLAY_SWITCH=true
```

`.env` 設定好後，就將 `render()` 函式修改為這樣


```php
<?php
// app/Exceptions/Handler.php

class Handler extends ExceptionHandler {

    /**
     * Render an exception into an HTTP response.
     * 將例外錯誤轉為 HTTP 回應
     *
     * @param  \Illuminate\Http\Request  $request
     * @param  \Exception  $e
     * @return \Illuminate\Http\Response
     */
    public function render($request, Exception $e)
    {
        // 預設不直接顯示例外訊息
        $exception_display_switch = env('EXCEPTION_DISPLAY_SWITCH', false);

        if ($exception_display_switch) {
            throw $e;
        }

        return parent::render($request, $e);
    }

}
```

這樣設定完後，也保留原本系統處理例外狀況的彈性，也可以讓我在單元測試（Unit test）中可以正常的去顯示例外訊息了！！


## 參考資料
* [錯誤與日誌 - laravel.tw](http://laravel.tw/docs/5.0/errors)
* [how do I create custom error page in laravel 5](http://stackoverflow.com/questions/28689629/how-do-i-create-custom-error-page-in-laravel-5)
* [Laravel 5.0 - Custom Error Pages](https://mattstauffer.co/blog/laravel-5.0-custom-error-pages)
