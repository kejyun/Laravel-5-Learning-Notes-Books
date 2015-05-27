# 使用 Gmail 寄信


在測試機測試的時候，為了節省郵件服務的開銷，我們可以使用 Gmail 當作我們測試的郵件服務，所以我們來介紹如何使用 Gmail 寄信

## 設定 `config/mail.php`

> driver 設為 `smtp`

> host 設為 `smtp.gmail.com`

> port 設為 `587`

> username 設為你要用來寄信的 Gmail 帳號 `kejyun@gmail.com`

> password 設為 Gmail 帳號的密碼

> pretend 設為 `true`，這樣才可以正常使用 Gmail 寄送

設定完後會像這樣：

```php
// config/mail.php
return [
  'driver'     => 'smtp',
  'host'       => 'smtp.gmail.com',
  'port'       => 587,
  'from'       => ['address' => 'kejyun@gmail.com', 'name' => 'KeJyun'],
  'encryption' => 'tls',
  'username'   =>'kejyun@gmail.com',
  'password'   => 'abcdefghijklmnopqrstuvwxyz123456',
  'sendmail'   => '/usr/sbin/sendmail -bs',
  'pretend'    => false,
];
```

## 測試使用 Gmail 寄信

```php
Mail::raw('測試使用 Laravel 5 的 Gmail 寄信服務', function($message)
{
    $message->to('kejyun@gmail.com');
});
```

這樣我們就可以使用 Gmail 去當作我們的郵件寄送服務了！！！


## 參考資料
* [Attempting to get Email to work in Laravel 5](http://stackoverflow.com/questions/29083520/attempting-to-get-email-to-work-in-laravel-5)
* [郵件 - Laravel.tw](http://laravel.tw/docs/5.0/mail)
