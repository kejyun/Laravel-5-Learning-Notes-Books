# Elixir

這裏會介紹 Laravel 5 使用 NPM 套件 Elixir 管理資源的相關說明


## 使用需求

因為 Laravel 的 Elixir 是透過 Node.js 的套件，去將 Laravel 目錄結構下的資源做整合的工具，所以我們需要安裝 Node.js 的相關套件，需要安裝的有下列：

* NPM (Node Package Manager)
* NPM gulp 套件
* NPM laravel-elixir 套件

### 安裝 NPM

NPM 是 Node.js 的套件管理工具，Node.js 在 0.6.3 版本開始內建 npm，你可以透過安裝 Node.js 的方式去安裝 NPM，或是透過 NPM 官方網站提供的安裝指令去安裝 NPM

```shell
$ curl -L https://www.npmjs.com/install.sh | sh
```

安裝完 NPM 後，我們就可以使用 NPM 去安裝相關的套件了！


### 安裝 NPM gulp 套件

> gulp 是 Node.js 的自動建構的工具，可以透過它整合並建立資源

我們要到我們 Laravel 專案的根目錄，然後執行 `npm install gulp` 去安裝 gulp

```shell
kejyun@KeJyundeMBP:~/Code/KeJyunLaravel5Proj$ npm install gulp
```

> 所有 NPM 安裝的套件會在專案跟目錄下的 `node_modules` 目錄下，注意！這個目錄不應該在專案的版本控制當中～

### 安裝 NPM laravel-elixir 套件

一樣在我們 Laravel 專案的根目錄，然後執行 `npm install laravel-elixir` 去安裝 laravel-elixir

```shell
$ npm install laravel-elixir
```

這些套件安裝完後，我們就可以開始使用 Elixir 的開發好處了！！


## 參考資料
* [Node.js](https://nodejs.org/)
* [npm](https://www.npmjs.com/)
* [NPM 套件管理工具](https://github.com/nodejs-tw/nodejs-wiki-book/blob/master/zh-tw/node_npm.rst)
* [gulp.js - the streaming build system](http://gulpjs.com/)
* [Laravel Elixir - laravel.tw](http://laravel.tw/docs/5.1/elixir)


!INCLUDE "../../kejyun/book/laravel-5-for-beginner.md"
