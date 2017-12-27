# 非同步資料庫隊列（Async Database Queue）

在我們使用 Laravel 提供的資料庫隊列（Database Queue）時，我們需要在命令列執行 `php artisan queue:listen` 指令，持續的去監聽是否有需要執行的 Queue。

[barryvdh/laravel-async-queue](https://packagist.org/packages/barryvdh/laravel-async-queue) 隊列套件，可以讓我們不用持續的監聽隊列資料，並在使用隊列時，立即的使用 shell 在背景執行隊列的工作。

目前（2015-06-01） 套件 0.4.x 版本有支援 Laravel 5

## 安裝

```shell
$ composer require 'barryvdh/laravel-async-queue:0.4.*@dev'
```

## 加入 Service Provider

在 `config/app.php` 檔案中加入 `'Barryvdh\Queue\AsyncServiceProvider'`

```php
// config/app.php
return [
  'providers' => [
    'Barryvdh\Queue\AsyncServiceProvider',
  ]
];
```


## 產生隊列資料表

[barryvdh/laravel-async-queue](https://packagist.org/packages/barryvdh/laravel-async-queue) 隊列套件使用原生的資料庫隊列資料表（Database Queue）去時做的，所以我們可以使用 `php artisan queue:table` 指令去產生隊列的 Migration

```shell
$ php artisan queue:table
```

所以執行命令後，你可以找到像是 `database/migrations/2015_05_26_225627_create_queue_jobs_table.php` 這樣的隊列 Migration 檔案

> Migration 檔名日期 `2015_05_26_225627` 每個人皆不同，會依照你建立當時的時間去產生


產生的隊列 Migration 會長的像這樣：


```php
<?php

use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class CreateQueueJobsTable extends Migration {

    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('jobs', function(Blueprint $table)
        {
            $table->bigIncrements('id');
            $table->string('queue');
            $table->text('payload');
            $table->tinyInteger('attempts')->unsigned();
            $table->tinyInteger('reserved')->unsigned();
            $table->unsignedInteger('reserved_at')->nullable();
            $table->unsignedInteger('available_at');
            $table->unsignedInteger('created_at');
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::drop('jobs');
    }

}
```

## 建立隊列資料表

執行 `php artisan migrate` 將隊列資料表新增至資料庫

## 設定隊列驅動

在 `config/queue.php` 檔案中設定非同步資料庫隊列（Async Database Queue）驅動設定，設定如下：

```php
// config/queue.php
return [
    'default' => 'async',
    'connections' => [
        'async' => [
            'driver' => 'async',
            'table' => 'jobs',
            'queue' => 'default',
            'expire' => 60,
            'connection_name'=>'',
        ],
    ],
];
```

## 建立隊列工作

我們可以使用 `\Queue::push('App\Commands\SendEmail@fire', $queue_data);` 的方法去新增要執行的隊列

第一個參數是執行隊列需要呼叫的類別名稱位置（`App\Commands\SendEmail`）及方法（`fire`）

類別名稱需要正確的指定類別的命名空間（namespace），可以指定這個隊列要執行的類別方法，只要將方法使用 `@` 加在後方即可（`@customMethod`）

若沒有指定用哪個方法，Laravel 預設會執行 `fire` 的類別方法（`@fire`）

我們使用隊列來寄送 Email，設定隊列的方式大概像這樣：

```php
// 需要傳送給隊列處理的資料
$queue_data = [
  'email' => 'kejyun@gmail.com',
  'name'  => 'KeJyun',
];

// 建立隊列
$queue_id = \Queue::push('App\Commands\SendEmail@fire', $queue_data);
```

在 `App\Commands\Sendmail.php` 檔案大概會像這樣：

```php
<?php namespace App\Commands;

class SendEmail {

    /**
     * 執行隊列
     *
     * @return void
     */
    public function fire($job, $data)
    {
        // 寄送 Email
        \Mail::send('emails.welcome', [], function($message) use ($data)
        {
            $message->to($data['email'], $data['name'])->subject('歡迎使用 Laravel 5 資料庫隊列寄送 Email!!!');
        });
    }
}
```

這樣我們就可以正常的使用隊列去幫我們寄信摟！！

> 目前（2015-06-01） [barryvdh/laravel-async-queue](https://packagist.org/packages/barryvdh/laravel-async-queue) 在執行完隊列時，無法直接刪除隊列資料，待作者修復這個 bug


## 參考資料
* [隊列 - Laravel.tw](http://laravel.tw/docs/5.0/queues)
* [Queues in Laravel with Redis](https://www.youtube.com/watch?v=dsp_l65W8ck)
* [barryvdh/laravel-async-queue - packagist](https://packagist.org/packages/barryvdh/laravel-async-queue)
* [Laravel 5 Async Queue Driver - Github](https://github.com/barryvdh/laravel-async-queue/tree/0.4)


!INCLUDE "../../kejyun/book/laravel-5-for-beginner.md"
