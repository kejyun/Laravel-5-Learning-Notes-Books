# 隊列（Queue）

這裏會介紹一些 Laravel 5 使用隊列（Queue）的方法

Laravel 隊列的設定檔在 `config/queue.php`，在這裡你可以設定你想要用什麼樣的隊列（Queue）服務去執行你的隊列，而 Laravel 預設有支援 database、[Beanstalkd](http://kr.github.com/beanstalkd)、[IronMQ](http://iron.io/)、[Amazon SQS](http://aws.amazon.com/sqs)、[Redis](http://redis.io/) 這幾種隊列的服務。

我們通常會將一些需要花比較久時間處理的工作丟給隊列去背景執行，讓使用者能夠快速的的到網站的回應，像是我們在寄送帳號認證信件時，因為透過郵件伺服器去寄送可能會花費比較久的時間，所以我們會將這類的工作丟到隊列去執行，所以使用者的認證信件就會延遲的發送到他們的信箱，但是使用者在瀏覽網站時卻可以有更好的體驗！
