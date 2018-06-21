# 讓編輯器識別 Facade 類別


## barryvdh/laravel-ide-helper: Laravel IDE Helper


**1.安裝套件（--dev only）**

```shell
composer require barryvdh/laravel-ide-helper --dev
```

**2.加入設定到 `config/app.php`**


```shell
return [
    'providers' => [
        Barryvdh\LaravelIdeHelper\IdeHelperServiceProvider::class,
    ],
];
```

**3.執行 artisan 指令**

```shell
php artisan ide-helper:generate
```

## 參考資料
* [barryvdh/laravel-ide-helper: Laravel IDE Helper](https://github.com/barryvdh/laravel-ide-helper)
