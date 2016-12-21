# 任務排程

我們通常會把一些每小時、每 6 小時、每日、每週、每月等等之類固定時間要做的工作丟到 Linux 系統的 crontab 中去執行，通常像是每日要統計昨天網站的活動資訊做數據分析之類的工作，這類的工作通常會花費比較久的時間

## 在 Linux 設定排程工作

我們通常會在命令列用 `$ crontab -e` 的方式去編輯排程工作

```shell
$ crontab -e
```

在用到 crontab 的時候，我們需要瞭解怎麼設定排程工作的執行時間，整個的 crontab 的設定可能會像這樣：

```shell
# 每天凌晨 3 點統計昨天的 Pageview
0 3 * * * /usr/bin/php /home/kejyun/laravel4/artisan cronjob:statisticYesterdayPageview
```

在前方可以看到有 5 個數字可以做設定，依序分別代表的意思為：

 * 分鐘 (0-59)
 * 小時 (0-23)
 * 每個月第幾天 (1-31)
 * 月份 (1-12)
 * 每週的第幾天 (0-6)
  * 0：星期日
  * 1：星期一
  * 2：星期二
  * 3：星期三
  * 4：星期四
  * 5：星期五
  * 6：星期六

這 5 個參數之間用空白隔開，每個參數除了設定單一個數字，也可以用逗號(,)去隔開設定相同單位的時間設定，像是：

```shell
# 每天凌晨 4 點及 16 點寄送廣告信
0 4,16 * * * /usr/bin/php /home/kejyun/laravel4/artisan cronjob:sendCommercialMail
```

這裏有一些相關的設定範例可以當作參考：

```shell
# 每小時的第 18 分鐘執行
18 * * * *

# 8 點 10 分執行
10 8 * * *

# 8 點的每分鐘執行一次（共執行 60 次）
* 8 * * *

# 在每個禮拜二每小時的第 18 分鐘執行
18 * * * 2

# 你也可以每隔一段時間去執行 crontab
# 如果我們每 15 分鐘要去執行，你可以用這樣的格式 */15
# 這樣的意思是將分鐘數，切割成（除以）每 15 分鐘執行
*/15 * * * *

# 每 2 小時執行
0 */2 * * *

# 每 2 小時又 20 分鐘執行
*/20 */2 * * *
```
**小提醒**

> 系統的 crontab 運作方式是`每分鐘`會到設定的 crontab 找看看有沒有符合現在這個時間的排程工作，所以像是 `* 8 * * *` 這樣的設定，因為沒有明確指定分鐘，在 8 點每分鐘檢查的 60 次都符合條件，所以會執行 60 次，若僅要 8 點時執行一次，請明確設定要執行的分鐘條件，像是 `0 8 * * *`


## 使用 Laravel 5.2 Scheduling 設定排程工作

因為 Linux 每分鐘都會去檢查當時是否有排程需要執行的工作，符合條件的時間就會執行，Laravel 利用這個特性，告訴排程每分鐘都要執行 Laravel 的 `schedule:run` 的指令

```shell
* * * * * php /path/to/artisan schedule:run >> /dev/null 2>&1
```

之後 Laravel 每分鐘就會執行 `app/Console/Command/Kernal@schedule` 的程式，Laravel 會依照 `schedule` 裡面的設定時間，執行符合條件的排程工作

### 排程範例

假如我們有一個排程每分鐘都會紀錄他執行的時間，程式碼會放在 `app/Console/Command/TestLog.php`，程式碼會像是：

```php
<?php

namespace App\Console\Commands;

use Illuminate\Console\Command;
use File;

class TestLog extends Command
{
    // 命令名稱
    protected $signature = 'test:Log';

    // 說明文字
    protected $description = '[測試] Log 檔案';

    public function __construct()
    {
        parent::__construct();
    }

    // Console 執行的程式
    public function handle()
    {
        // 檔案紀錄在 storage/test.log
        $log_file_path = storage_path('test.log');

        // 記錄當時的時間
        $log_info = [
            'date'=>date('Y-m-d H:i:s')
        ];

        // 記錄 JSON 字串
        $log_info_json = json_encode($log_info) . "\r\n";

        // 記錄 Log
        File::append($log_file_path, $log_info_json);
    }
}
```

以上排程的命令是 `php artisan test:Log`，執行之後就會記錄當時執行的時間

排程程式建立好之後，在 `app/Console/Command/Kernal.php` 定義此排程的工作，並設定每分鐘執行一次，程式碼會像是：

```php
<?php

namespace App\Console;

use Illuminate\Console\Scheduling\Schedule;
use Illuminate\Foundation\Console\Kernel as ConsoleKernel;

class Kernel extends ConsoleKernel
{
    // 定義應用程式的 Artisan 指令
    protected $commands = [
        \App\Console\Commands\TestLog::class,
    ];


    // 定義應用程式的排程
    protected function schedule(Schedule $schedule)
    {
        // 每分鐘執行 Artisan 命令 test:Log
        $schedule->command('test:Log')->everyMinute();
    }
}
```

這樣你就可以在 `storage/test.log` 每分鐘看到像這樣的紀錄，這樣就表示你的排程設定成功了！！

```
{"date":"2016-01-11 22:12:05"}
{"date":"2016-01-11 22:13:05"}
{"date":"2016-01-11 22:14:05"}
{"date":"2016-01-11 22:15:05"}
{"date":"2016-01-11 22:16:05"}
{"date":"2016-01-11 22:17:07"}
{"date":"2016-01-11 22:18:09"}
{"date":"2016-01-11 22:19:05"}
```

你也可以使用其他 Laravel 提供的時間方法去定義要執行的時間

| 方法 | 說明 |
|---|---|
| ->cron('* * * * * *'); | 	自訂 Cron 排成時間 |
| ->everyMinute(); | 	每分鐘執行 |
| ->everyFiveMinutes(); | 	每 5 分鐘執行 |
| ->everyTenMinutes(); | 	每 10 分鐘執行 |
| ->everyThirtyMinutes(); | 	每 30 分鐘執行 |
| ->hourly(); | 	每小時執行 |
| ->daily(); | 	每天執行 |
| ->dailyAt('13:00'); | 	每天 13:00 執行 |
| ->twiceDaily(1, 13); | 	每天 1:00 及 13:00 執行 |
| ->weekly(); | 	每週執行 |
| ->monthly(); | 	每月執行 |
| ->yearly(); | 	每年執行 |
| ->sundays() |	每週日執行 |
| ->mondays() |	每週一執行 |
| ->tuesdays() |	每週二執行 |
| ->wednesdays() |	每週三執行 |
| ->thursdays() |	每週四執行 |
| ->fridays() |	每週五執行 |
| ->saturdays() |	每週六執行 |
| ->when(Closure) |	每當符合條件就執行（return true） |

### 避免重複執行排程

排程預設每次符合條件就要執行，但若我們執行一個需要跑很久的程式，在下一次符合條件的時間若上一個同樣的工作還沒有執行完，我們就不執行的話，我們可以用 `->withoutOverlapping()` 方法去避免排程程式重複執行，像是

```php
$schedule->command('emails:send')->withoutOverlapping();
```

### 輸出執行結果

我們的在執行 Artisan 指令時，我們通常會在畫面上列印一些執行狀態，像是 `$this->info('把我顯示在畫面上');`，如果我們想要知道排程執行時，這些顯示在畫面的文字記錄下來，我們可以用

| 方法 | 說明 |
|---|---|
| `->sendOutputTo($filePath);`  |  將結果輸出到檔案（複寫該檔案） |
| `->appendOutputTo($filePath);`  | 將結果附加在檔案後面（不複寫檔案） |
| `->emailOutputTo('foo@example.com');`  | 將結果寄送到指定 Email |

*** 將結果輸出到檔案（複寫該檔案） ***

```php
$schedule->command('emails:send')
         ->daily()
         ->sendOutputTo($filePath);
```

*** 將結果附加在檔案後面（不複寫檔案） ***

```php
$schedule->command('emails:send')
         ->daily()
         ->appendOutputTo($filePath);
```

*** 將結果寄送到指定 Email ***

```php
$schedule->command('foo')
         ->daily()
         ->sendOutputTo($filePath)
         ->emailOutputTo('foo@example.com');
```

### 排程觸發

我們可以在排程執行前後，分別使用 `->before()` 或 `->after()` 去執行排程其他的工作

```php
$schedule->command('emails:send')
         ->daily()
         ->before(function () {
             // 在排程執行前觸發
         })
         ->after(function () {
             // 在排程執行完成後觸發
         });
```


## 參考資料
* [crontab.guru - the cron schedule expression editor](https://crontab.guru/)
* [A visual crontab editor](http://www.corntab.com/pages/crontab-gui)
* [Task Scheduling - Laravel 5.2](https://laravel.com/docs/5.2/scheduling)
