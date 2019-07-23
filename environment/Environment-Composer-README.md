# Composer 安裝


## Carbon 1 is deprecated, see how to migrate to Carbon 2.

在安裝 Laravel 時會跑出 Carbon 的版本過舊問題

```
$ composer install
Loading composer repositories with package information
Installing dependencies (including require-dev) from lock file
Warning: The lock file is not up to date with the latest changes in composer.json. You may be getting outdated dependencies. Run update to update them.
Nothing to install or update
Package phpunit/phpunit-mock-objects is abandoned, you should avoid using it. No replacement was suggested.
Generating optimized autoload files
Carbon 1 is deprecated, see how to migrate to Carbon 2.
https://carbon.nesbot.com/docs/#api-carbon-2
    You can run './vendor/bin/upgrade-carbon' to get help in updating carbon and other frameworks and libraries that depend on it.
```

建議我們將 Carbon 升級到 Carbon 2，但因為 Carbon 2 至少要 Laravel 5.8，但線上專案因為是用 Laravel 5.5，無法升級，所以可以在 composer 加入此套件即可向下相容


```
{
    "require": {
        "nesbot/carbon": "2.21.3 as 1.34.0"
        "kylekatarnls/laravel-carbon-2": "^1.0.0"
    }
}
```




## 參考資料
* [陈华博客 | Laravel5.5执行composer update报错问题 - 陈华编程学院](http://www.ichenhua.cn/blog/post/43)
* [Carbon - A simple PHP API extension for DateTime.](https://carbon.nesbot.com/)
