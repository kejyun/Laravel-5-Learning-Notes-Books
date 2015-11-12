# 資料庫常見問題：無法取得查詢 Log

在 Laravel 4 為了要確定下的 SQL 語法有符合我們預期，我們常常在做完資料庫查詢後，使用 `DB::getQueryLog();` 去取得做資料庫查詢的 Qeury Log，但因為 Laravel 會把這些 Log 都記錄在記憶體中，如果做了大量的新增的查詢，記憶體會使用過多可能會造成系統 Crash。

所以 Laravel 5 預設把記錄 Query Log 的機制關閉，若需要做 Query Debug，需要自行打開 Qeury Log 功能

```php
<?php
// 啟用 Query Log 功能
DB::connection()->enableQueryLog();
```

這樣我們就可以使用 `DB::getQueryLog();` 去取得做資料庫查詢的 Qeury Log 摟！！
要得到執行過的查詢紀錄陣列，你可以使用 getQueryLog 方法：

```php
<?php
// 取得資料庫查詢的 Qeury Log
$queries = DB::getQueryLog();

var_dump($queries);
```


## 參考資料
* [資料庫使用基礎 查詢日誌記錄 - Laravel.tw](http://laravel.tw/docs/5.0/database#query-logging)
* [How to get the query executed in Laravel 5 ? DB::getQueryLog returning empty array](http://stackoverflow.com/questions/27753868/how-to-get-the-query-executed-in-laravel-5-dbgetquerylog-returning-empty-arr)
