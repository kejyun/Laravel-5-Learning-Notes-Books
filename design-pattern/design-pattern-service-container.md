# 服務容器（Service Container）

Laravel 的服務容器就像 IoC 容器一樣，可以讓你很容易的反轉控制物件

假如我們沒有注入類別去進行反轉控制，則我們每次使用 Mail 類別去寄送郵件時都要去 new 它，如果這個 Mail 類別在裡面是會被很頻繁的使用時，這樣會讓我們很惱人。

```php
// 通知類別
class Notification {

    // 通知會員有新訊息
    public function noticeNewMessage() {
        $mail = new Mail();
        $mail->send();
    }

    // 通知會員有新文章
    public function noticeNewArticle() {
        $mail = new Mail();
        $mail->send();
    }
}
```

為了能夠讓通知類別 `Notification` 能夠隨時取用 `Mail` 類別，我們會希望將此類別直接注入，讓通知類別可以直接去進行反轉控制。

在我們使用反轉控制（IoC）時，我們時常需要在建構子 `__construct()` 或方法 `function()` 中注入需要反轉控制的類別，讓被注入的類別可以直接控制其物件，就像：

```php
// 通知類別
class Notification {
    public $mail;

    public function __construct (Mail $mail) {
        $this->mail = $mail;
    }

    // 通知會員有新訊息
    public function noticeNewMessage() {
        $this->mail->send();
    }

    // 通知會員有新文章
    public function noticeNewArticle() {
        $this->mail->send();
    }
}
```

我們在通知類別 `Notification` 建構子中注入 `Mail` 類別給內部 `mail` 變數，之後要使用此 Mail 類別時，就直接使用傳送信件 `send()` 的功能即可。

我們也可以在個別的函式中分別注入需要反轉控制的類別，就像：


```php
// 通知類別
class Notification {

    // 通知會員有新訊息
    public function noticeNewMessage(Mail $mail) {
        $mail->send();
    }

    // 通知會員有新文章
    public function noticeNewArticle(Mail $mail) {
        $mail->send();
    }
}
```

但是在 Laravel 4 要注入類別之前，必須先對類別名稱對 app 進行綁定，讓 Laravel 4 認得這個要注入的類別是什麼物件

```php
App::bind('Mail', function($app)
{
    return new SomeEmailService;
});
```


但是在 Laravel 5 除了可以用這樣手動綁定類別的方式，也有提供強大的自動綁定功能，你不需要在 app 內事先定義所有的類別，當 Laravel 5 在 app 內找不到該類別的時候，就會自動找所有引入（include）的類別中有沒有此類別，自動進行注入綁定！


## 參考資料
* [The Service Container - Laracast](https://laracasts.com/series/laravel-5-fundamentals/episodes/26)
* [服務容器 - Laravel.tw](http://laravel.tw/docs/5.0/container)
* [IoC 容器](http://laravel.tw/docs/4.2/ioc)
* [Dependency Injection with Laravel’s IoC](http://www.sitepoint.com/dependency-injection-laravels-ioc/)
