# 資料庫隊列（Database Queue）

我們可以使用 `database` 的隊列設定，在自己的資料庫建立隊列資料表

## 產生隊列資料表

我們可以使用 `php artisan queue:table` 指令去產生隊列的 Migration

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

在 `config/queue.php` 檔案中設定資料庫隊列驅動設定，設定如下：

```php
// config/queue.php
return [
    'default' => 'database',
    'connections' => [
        'database' => [
            'driver' => 'database',
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

        // 執行成功，刪除隊列
        $job->delete();
    }
}
```

`fire` 方法中的 `$job` 變數會接受該隊列的實例，`$data` 變數會接收建立隊列時傳入的資料

像我要使用隊列寄送 Email，則會將使用者的相關資訊傳送到這個隊列來，讓隊列能正確的發送正確的 Email 資訊給使用者！

在隊列執行完成無誤後，我們必須要使用 `$job->delete();` 將隊列資料刪除，若沒有刪除 Laravel 下次再出理到該資料時，則會視為隊列處理失敗，進而嘗試重新處理


## 監聽隊列工作

我們會在 shell 中執行 `php artisan queue:listen` 去持續的監聽隊列資料的狀況，若有新增隊列到資料表時，Laravel 則會開始處理隊列的資料

```shell
$ php artisan queue:listen
```

這樣我們就可以正常的使用隊列去幫我們寄信摟！！

## 參考資料
* [隊列 - Laravel.tw](http://laravel.tw/docs/5.0/queues)
* [Queues in Laravel with Redis](https://www.youtube.com/watch?v=dsp_l65W8ck)
