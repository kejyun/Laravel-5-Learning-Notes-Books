# 日誌記錄層級

在系統發生例外錯誤時，我們會希望紀錄當時的例外訊息，以便之後我我們好進行除錯，而 Log 紀錄的訊息會依照日期被放到像 `storage/logs/laravel-2015-06-06.log` 的地方

Log 記錄在 Laravel 有七個級別：debug、info、notice、warning、error、critical 和 alert，他們在 Log 檔紀錄的樣子會像：

```
[2015-06-06 16:22:00] testing.DEBUG: === Log 訊息 ===
[2015-06-06 16:22:00] testing.INFO: === Log 訊息 ===
[2015-06-06 16:22:00] testing.NOTICE: === Log 訊息 ===
[2015-06-06 16:22:00] testing.WARNING: === Log 訊息 ===
[2015-06-06 16:22:00] testing.ERROR: === Log 訊息 ===
[2015-06-06 16:22:00] testing.CRITICAL: === Log 訊息 ===
[2015-06-06 16:22:00] testing.ALERT: === Log 訊息 ===
```

我們透過不同的記錄層級，讓我們容易找到層級比較嚴重的 Bug 先進行修復


## 參考資料
* [錯誤與日誌 - Laravel.tw](http://laravel.tw/docs/5.0/errors)
