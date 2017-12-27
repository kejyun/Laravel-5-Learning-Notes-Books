# Call to undefined method getCachedCompilePath()

我在使用 Laravel 5.0.x 時，使用 `composer update` 去更新目前的套件時，跳出了這樣的訊息：

（PS:也有人在執行 `php artisan clear-compiled` 出現這樣的狀況）

> PHP Fatal error:  Call to undefined method Illuminate\Foundation\Application::getCachedCompilePath()

這個是因為 Laravel 5 在執行時會把整個 Framework 編譯到 `storage/framework/compiled.php`，若這個檔案已產生，Laravel 5 在更新套件時執行一些相關 Laravel 的功能時，會預設執行 `compiled.php` 檔案中的類別函式，而更新的檔案中有 `getCachedCompilePath()` 這個方法，所以呼叫時 Laravel 會在舊的 `compiled.php` 找不到這個方法

***解決方式***

直接把 `storage/framework/compiled.php` 刪除即可，Laravel 5 會自動重新產生這個 `compiled.php` 檔案！


## 參考資料
* [RuntimeException on fresh install](https://laracasts.com/discuss/channels/general-discussion/runtimeexception-on-fresh-install?page=1)

!INCLUDE "../kejyun/book/laravel-5-for-beginner.md"
