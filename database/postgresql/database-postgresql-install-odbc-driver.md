# 安裝 PostgreSQL ODBC driver

> 環境：OS X
> 日期：2015-03-29
> PHP：5.6

在我們在 Laravel 使用 PostgreSQL 去做 Migration 的時候，我們會看到像下面這樣的錯誤訊息：

```shell
$ php artisan migrate

  [PDOException]
  could not find driver

$
```

這表示我們沒有相關的連線驅動程式去連線到 PostgreSQL，所以我們需要安裝我們所需要的驅動程式

在 OS X 的 PHP 相關環境我是用 brew 去安裝的，如果你也是用 brew 去安裝，可以先看看自己的套件是用哪一個版本的 PHP

```shell
$ brew list
autoconf  git       libpng      mhash      php56          readline   zlib
freetype  icu4c     libtool     nvm        php56-mcrypt   unixodbc
gettext   jpeg      mcrypt      openssl    postgresql     wget
```

然後搜尋現在 brew 支援的 PostgreSQL 驅動程式

```shell
$ brew  search pgsql
osm2pgsql	 php54-pdo-pgsql  php55-pdo-pgsql  php56-pdo-pgsql
```

我們找到我們 php 5.6 版本的驅動程式了，可以用下面的指令去安裝

```shell
$ brew install php56-pdo-pgsql
```

安裝完成後就可以正常的使用 Migration 或相關的 DB 指令去存取 PostgreSQL 了～～！！

## 參考資料
* [Laravel: Error [PDOException]: Could not Find Driver in PostgreSQL](http://stackoverflow.com/questions/25329302/laravel-error-pdoexception-could-not-find-driver-in-postgresql)

!INCLUDE "../../kejyun/book/laravel-5-for-beginner.md"
