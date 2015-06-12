# 變更專案目錄名稱導致 View 無法讀取

在想要變更原先 Laravel 5 的專案目錄時，Laravel 5 會告訴你你沒辦法讀取到 view 的目錄，看了錯誤的訊息發現這個 view 的目錄是在原先舊專案的目錄名稱下

錯誤訊息長得像這樣：

```
InvalidArgumentException in FileViewFinder.php line 137:
View [welcome] not found.
```


這個原因是 Laravel 5 會將設定檔作快取存下來到 `bootstrap/cache/config.php` 目錄下面，而這個快取的檔案是原本舊專案的設定，所以我們必須要清除這個快取，才可以讓 Laravel 5 可以讀取到新的設定，這時候我們可以在 artisan 下這些指令，清除原先專案的快取設定：

```php
$ php artisan config:clear
$ php artisan view:clear
```

執行清除這些快取資料後，我們就可以正常的的獨到新專案目錄下的 view 資料摟！
