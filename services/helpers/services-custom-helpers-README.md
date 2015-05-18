# 自定義輔助方法

Laravel 中有提供許多的輔助方法（Helpers），但有時候我們會想要自訂自己的輔助方法，我們可以這樣做

## 加入自定義引用的 `Helpers.php` 檔案到 `/app/Support/Helpers/Helpers.php` 路徑下

```php
<?php
// /app/Support/Helpers/Helpers.php

// Helper 檔案路徑
$helpers = [
    'CustomHelper.php'
];

// 載入 Helper 檔案
foreach ($helpers as $helperFileName) {
    include __DIR__ . '/' .$helperFileName;
}
```

以後若有其他的 Helper 需要加入，僅需要加到 `Helpers.php` 檔案中的 `$helpers` 變數當中即可

## 在 `composer.json` 中自動載入加入該 `Helper.php`

```json
/*composer.json*/
{
  "autoload": {
		"classmap": [
			"database"
		],
		"psr-4": {
			"App\\": "app/"
		},
		"files": [
			"app/Support/Helpers/helpers.php"
		]
	}
}
```

## 重新編譯 `autoload.php`

```shell
$ composer dump-autoload
Generating autoload files
```

這樣我們就可以自動的載入我們自定義的 Helper 函式了！！

* [Best practices for custom helpers on Laravel 5](http://laravel.io/forum/02-03-2015-best-practices-for-custom-helpers-on-laravel-5)
