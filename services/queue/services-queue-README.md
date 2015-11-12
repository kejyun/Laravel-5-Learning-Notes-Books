# 隊列（Queue）

這裏會介紹一些 Laravel 5 使用隊列（Queue）的方法

Laravel 隊列的設定檔在 `config/queue.php`，在這裡你可以設定你想要用什麼樣的隊列（Queue）服務去執行你的隊列，而 Laravel 預設有支援 database、[Beanstalkd](http://kr.github.com/beanstalkd)、[IronMQ](http://iron.io/)、[Amazon SQS](http://aws.amazon.com/sqs)、[Redis](http://redis.io/) 這幾種隊列的服務。

我們通常會將一些需要花比較久時間處理的工作丟給隊列去背景執行，讓使用者能夠快速的的到網站的回應，像是我們在寄送帳號認證信件時，因為透過郵件伺服器去寄送可能會花費比較久的時間，所以我們會將這類的工作丟到隊列去執行，所以使用者的認證信件就會延遲的發送到他們的信箱，但是使用者在瀏覽網站時卻可以有更好的體驗！


## 指令

在使用 Queue 去幫我們做工作的時候，我們在系統背景需要執行傾聽 Queue 是否有工作的指令，像是 `php artisan queue:listen`，這樣 Queue 中有新工作需要做，才能夠正常的去執行。

```shell
$ php artisan queue:listen
```

執行 Queue 指令有一些相關的參數，可以依照自己的環境去調校

```
$php artisan queue:listen [--queue[="..."]] [--delay[="..."]] [--memory[="..."]] [--timeout[="..."]] [--sleep[="..."]] [--tries[="...”]]
```

| 參數 | 說明  | 指令  |
|---|---|---|
| queue  | 設定優先執行的工作順序  | `php artisan queue:listen --queue=high,low`  |
| delay  | 在執行的工作發生錯誤時，要延遲多久重新執行（單位：秒），預設 0 秒  | `php artisan queue:listen --delay=10`  |
| memory  | 執行工作最多能夠使用的記憶體上限（單位：MB），預設 128 MB  | `php artisan queue:listen --memory=1024`  |
| timeout  | 執行的工作做長執行的時間是多長（單位：秒），預設 60 秒  |  `php artisan queue:listen --timeout=3600`  |
| sleep  | 在沒有找到可以做的工作時，需要間隔多少秒再去檢查有無新的工作（單位：秒），預設 3 秒 | `php artisan queue:listen --sleep=10`  |
| tries  | 工作執行失敗時，最多重新嘗試執行幾次（單位：次數），預設 0，無限制次數  | `php artisan queue:listen --tries=3`  |
