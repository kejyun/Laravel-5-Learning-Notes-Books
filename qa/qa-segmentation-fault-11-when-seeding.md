# Segmentation fault: 11 when seeding

在使用 Laravel 的 Pagination 的時候，因為畫面有些許異動需要改 Pagination 的結構樣式，所以需要使用 `artisan` 產生預設的樣板

```shell
php artisan vendor:publish --tag=laravel-pagination
```

在執行完指令後，跑出了 `Segmentation fault: 11` 的訊息

```
Segmentation fault: 11
```

經查詢後，發現這個是 PHP 版本的問題

因為我用 Homestead，專案目錄與 Homestead 同步，所以索性我在 OSX 下這個指令，但我 OSX 的 PHP 版本似乎過舊，所以就 ssh 到 Homestead 中去下這個命令就好了～

```
vagrant ssh
cd ~/Code/project
~/Code/project$ php artisan vendor:publish --tag=laravel-pagination

Copied Directory [/vendor/laravel/framework/src/Illuminate/Pagination/resources/views] To [/resources/views/vendor/pagination]
Publishing complete.
```

## 參考資料
* [Database: Pagination](https://laravel.com/docs/5.4/pagination)
* [Segmentation fault: 11 when seeding · Issue #3921 · laravel/framework](https://github.com/laravel/framework/issues/3921)

!INCLUDE "../kejyun/book/laravel-5-for-beginner.md"
