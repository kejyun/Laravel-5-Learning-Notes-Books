# Migration

這裏會介紹如何在 Laravel 5 使用 Migration 管理資料庫

## Migration 指令

### 建立 Migration

```shell
$ php artisan make:migration create_users_table --create="users"
```

Migration 建立之後的檔案會放在 `database/migrations/2015_04_11_134630_create_users_table.php`

> Migration 檔案最前面的日期會依照你建立 Migration 的時間自動產生，所以每個人看到的檔名皆會不同
> 在後面加了 `--create` 的參數可以告訴 Migration，我們要做建立 `user` 資料表的動作，檔案內容會像這樣：

```php
<?php
// database/migrations/2015_04_11_134630_create_users_table.php

use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class CreateUsersTable extends Migration {

    /**
     * Run the migrations.
     *
     * @return void
     */ㄒ
    public function up()
    {
        Schema::create('users', function(Blueprint $table)
        {
            $table->increments('id');
            $table->timestamp();
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::drop('users');
    }

}

```

### 異動資料表欄位資料

如果我們要在 `users` 資料表中加欄位（或是其他改變資料表結構），我們可以用這樣的指令去建立 Migration

```shell
$ php artisan make:migration add_email_to_users_table --table="users"
```

> 在後面加了 `--table` 的參數可以告訴 Migration，我們要做異動 `user` 資料表的動作，檔案內容會像這樣：

```php
<?php
// database/migrations/2015_04_12_154720_add_email_to_users_table.php

use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class AddEmailToUsersTable extends Migration {

    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::table('users', function(Blueprint $table)
        {
            //
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::table('users', function(Blueprint $table)
        {
            //
        });
    }

}

```

> 原先的建立資料表會用 `Schema::create()`，而異動資料表則會用 `Schema::table()` 去做異動

### 列出目前所有 Migration 狀態

```shell
$ php artisan migrate:status
```

### 執行 Migration

```shell
$ php artisan migrate
```

### 恢復上一版本的 Migration

```shell
$ php artisan migrate:rollback
```

### 清除所有版本的 Migration

```shell
$ php artisan migrate:reset
```

### 清除所有版本的 Migration 並重新執行

```shell
$ php artisan migrate:refresh
```

## 備註

在剛開始開發產品的時候，有時候資料表有做小小的修改或異動，為了圖方便，我們常常會使用 `migrate:reset` 或 `migrate:refresh` 去清空我們的資料，重建資料表。

但如果產品已經上線了，這個指令就會是一個非常危險的指令，企業產品最重要的資產就是`資料`，這個指令會導致所有的資料都被清除，所以請上線後小心謹慎去使用。

## 參考資料
* [遷移和資料填充 - Laravel.tw](http://laravel.tw/docs/5.0/migrations)
* [Migrations - Laracasts](https://laracasts.com/series/laravel-5-fundamentals/episodes/7)
