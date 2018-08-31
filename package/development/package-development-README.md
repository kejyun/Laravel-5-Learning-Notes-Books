# Laravel Packages Development 套件開發


## 建立本地端的套件資料夾

在本地端開發套件，可以在 Laravel 根目錄自行建立 `packages` 目錄，然後將套件放在下方路徑中

> `packages` > `<user_name>` > `<package_name>` > `<code_folder>`

```shell
app
packages            # 套件資料夾（原本沒有此資料，需要自行建立）
    kejyun          # <user_name> 作者名稱
        larapack    # <package_name> 套件名稱
            src     # <code_folder> 所有套件程式皆放在此目錄
```

所有套件相關的程式皆放在此路徑下

## 建立套件 composer.json

在建立的套件目錄下初始化此套件的 `composer.json`

```shell
~/laravel_project/packages/kejyun/larapack $ composer init

Package name (<vendor>/<name>) [vagrant/larapack]: kejyun/larapack
Description []: KJ Larapack
Author [, n to skip]: KeJyun <kejyun@gmail.com>
Minimum Stability []: dev
Package Type (e.g. library, project, metapackage, composer-plugin) []:
License []: MIT

Define your dependencies.

Would you like to define your dependencies (require) interactively [yes]? no
Would you like to define your dev dependencies (require-dev) interactively [yes]? no

{
    "name": "kejyun/larapack",
    "description": "KJ Larapack",
    "license": "MIT",
    "authors": [
        {
            "name": "KeJyun",
            "email": "kejyun@gmail.com"
        }
    ],
    "minimum-stability": "dev",
    "require": {}
}

Do you confirm generation [yes]?
```

建立完的套件 `composer.json` 會長的像這樣

```json
{
    "name": "kejyun/larapack",
    "description": "KJ Larapack",
    "license": "MIT",
    "authors": [
        {
            "name": "KeJyun",
            "email": "kejyun@gmail.com"
        }
    ],
    "minimum-stability": "dev",
    "require": {}
}
```

## 設定根目錄與套件目錄的 `psr-4` 自動載入程式

**1. 根目錄 composer.json**

在開發過程中，為了測試方便，需要能夠在本地端直接讀取到套件的資料，在根目錄的 `composer.json` 直接設定使用 `psr-4` 自動載入套件路徑的程式，並設定在此路徑下的命名空間

> 命名空間需與接下來要上傳到 Packagist 給大家使用的套件命名空間相同，若之後需要讓大家用 `<user_name>\<package_name>` 使用您的套件，這邊的命名空間就需預先設定好，在開發套件過程中，也可以直接用這樣的方式去存取套件

```json
/* composer.json */
{
    "autoload": {
        "classmap": [
            "database/seeds",
            "database/factories"
        ],
        "psr-4": {
            "App\\": "app/",
            "KeJyun\\Larapack\\" : "packages/kejyun/larapack/src"
        }
    },
}
```

**2. 套件目錄 composer.json**

在套件目錄設定套件命名空間的目錄為 `src` 目錄，在使用 `composer require` 安裝套件到 vendor 目錄時，autoload 會使用此命名空間去載入套件 `src` 目錄的程式

```json
/* packages/kejyun/larapack/composer.json */
{
    "autoload": {
        "psr-4": {
            "KeJyun\\Larapack\\": "src"
        }
    },
}
```

**3. 產生 autoload 檔案**

因為需要載入非 `vendor` 目錄的套件資料，是自行設定的自動載入目錄，所以需要在專案根目錄產生新的 autoload 檔案

```shell
~/laravel_project $ composer dump-autoload
Generating optimized autoload files
```

## 建立套件類別 Larapack

```shell
app
packages            # 套件資料夾（原本沒有此資料，需要自行建立）
    kejyun          # <user_name> 作者名稱
        larapack    # <package_name> 套件名稱
            src     # <code_folder> 所有套件程式皆放在此目錄
                Larapack.php                    # 套件主程式
```

我們套件程式的進入點為 `packages/kejyun/larapack/src/Larapack.php`，在裡面可以寫一個簡單的 dump 方法，等等可以測試是否可以正常呼叫

```php
<?php
// packages/kejyun/larapack/src/Larapack.php
namespace KeJyun\Larapack;


class Larapack {

    public function dump($message)
    {
        dump($message);
    }
}
```

## 建立套件 ServiceProvider 與 Facade

Packagist 是所有 PHP 相關的套件，不是只提供給 Laravel 做使用，所以若要讓套件可以支援 Laravel，則必須提供 ServiceProvider 與 Facade 讓我們能夠快速的在 Laravel 使用套件的方法

```shell
app
packages            # 套件資料夾（原本沒有此資料，需要自行建立）
    kejyun          # <user_name> 作者名稱
        larapack    # <package_name> 套件名稱
            src     # <code_folder> 所有套件程式皆放在此目錄
                Larapack.php                    # 套件主程式
                LarapackServiceProvider.php     # 套件 ServiceProvider(for laravel)
                LarapackFacade.php              # 套件 Facade(for laravel)
```

在 ServiceProvider 註冊一個名稱為 `larapack` 的類別，使用 `Larapack` 物件作為此類別


```php
<?php

namespace KeJyun\Larapack;

use Illuminate\Support\ServiceProvider;

class LarapackServiceProvider extends ServiceProvider
{
    public function boot()
    {

    }

    // 註冊套件函式
    public function register()
    {
        $this->app->singleton('larapack', function ($app) {
            return new Larapack();
        });
    }
}
```

使用在 ServiceProvider 註冊的 `larapack`類別，作為 Laravel 的 Facade 物件

```php
<?php

namespace KeJyun\Larapack;

use Illuminate\Support\Facades\Facade;

class LarapackFacade extends Facade
{
    protected static function getFacadeAccessor()
    {
        return 'larapack';
    }
}
```

## Laravel 套件設定

可以將剛剛建立的 ServiceProvider 及 Facade 設定到 `config/app.php` 檔案中，這樣 Laravel 就可以正常的存取這個套件

```php
// config/app.php
return [
    'providers' => [
        KeJyun\Larapack\LarapackServiceProvider::class,
    ],
    'aliases' => [
        'Larapack' => \KeJyun\Larapack\LarapackFacade::class,
    ],
];
```

## 呼叫此套件

建立一個套件控制器，可以試著呼叫剛剛建立的套件，就可以在畫面中順利的看到你的套件被正常呼叫了。

```php
<?php

namespace App\Http\Controllers;

use Larapack;

class PackageController extends Controller {

    public function larapack()
    {
        Larapack::dump('My Laravel Package');
    }
}
```




## 參考資料
* [如何開發自己的Package? | 點燈坊](https://oomusou.io/laravel/laravel-package-hello-world/)
* [Laravel 開發擴充套件包基本流程 - ITW01](https://itw01.com/GU2REOO.html)
* [模組化 套件 開發自己的Package - Alvin Chen Club](http://www.alvinchen.club/2018/05/04/%E6%A8%A1%E7%B5%84%E5%8C%96-%E5%A5%97%E4%BB%B6-%E9%96%8B%E7%99%BC%E8%87%AA%E5%B7%B1%E7%9A%84package/)
* [Laravel 5 套件開發設定](https://ronald.tw/laravel-5-package-development-setting/)


!INCLUDE "../../kejyun/book/laravel-5-for-beginner.md"
