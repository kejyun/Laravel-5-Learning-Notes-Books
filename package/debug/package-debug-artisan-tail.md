# Artisan tail

在 Laravel 4 我們常常用 `php artisan tail` 去即時顯示系統的 Log，以便我們進行除錯，但在 Laravel 5 預設是將這個套件拿到的，所以如果我們要繼續使用的話，必須要手動的將套件加回去。

## 安裝 Laravel tail 套件

首先在命令列執行 `composer require spatie/laravel-tail` 安裝 tail 套件

```shell
$ composer require spatie/laravel-tail
```

接著在 `config/app.php` 檔案中的 `providers` 加入 tail ServiceProvider

```php
// config/app.php

'providers' => [
    ...
    'Spatie\Tail\TailServiceProvider',
    ...
];
```

安裝完畢之後就可以繼續使用 `php artisan tail` 去觀看你的 Log 了！

```shell
$ php artisan tail
```

## tail 遠端主機 Log 檔

tail 命令可以讓我們即時觀看遠端的 Log 檔案，我們只要將遠端主機的相關設定加入即可，可以使用下列指令建立 `config/tail.php` 檔案


```shell
$ php artisan vendor:publish --provider="Spatie\Tail\TailServiceProvider"
```

之後只要在 tail 指令後面加入要觀看的主機名稱，即可立即觀看該主機的 Log 檔摟

```shell
$ php artisan tail production
```


## 參考資料
* [The missing tail command for Laravel 5](https://github.com/freekmurze/laravel-tail)

!INCLUDE "../../kejyun/book/laravel-5-for-beginner.md"
