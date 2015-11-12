# Laravel 5.1 目錄結構異動

從 Laravel 5.0 升級到 Laravel 5.1 時，app 的目錄結構有做一些小異動，異動如下


* app/Command => app/Jobs
* app/Handlers => app/Listeners

根據官方說法，這樣的命名比較能夠識別出該目錄程式的作用是什麼

* 命令（Command） => 工作（Jobs）
* 處理器（Handlers） => 事件傾聽器（Listeners）

在從 5.0 升級至 5.1 時，記得將這幾個目錄做重新命名喔～


## 參考資料
* [Directory Changes - Laracast](https://laracasts.com/series/whats-new-in-laravel-5-1/episodes/7)
* [Release Notes - laravel.tw](http://laravel.tw/docs/5.1/releases)
