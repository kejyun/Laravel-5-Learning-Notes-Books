# 使用 Mailgun 寄信

Mailgun 對於初期的產品是一個不錯的郵件服務，每個月可以免費寄送 10000 封信，對於初期的應用應該是綽綽有餘，而且 Laravel 5 預設有支援 Mailgun 的服務，所以我們來介紹如何使用 Mailgun 寄信

## 設定 `config/mail.php`

> driver 設為 `mailgun`

> host 設為 `smtp.mailgun.org`

> port 設為 `587`

> username 設為 `postmaster@mailgun.kejyun.com`，這個帳號可以登入後到 Domains 頁選擇你設定的 Domains，找到 `Default SMTP Login` 就可以看到這個帳號

> password 設為你自己的密碼，Mailgun 顯次的欄位為 `Default Password`，密碼長度為 32 碼

> pretend 設為 `true`，這樣才可以正常使用 Mailgun 寄送

設定完後會像這樣：

```php
// config/mail.php
return [
  'driver'     => 'mailgun',
  'host'       => 'smtp.mailgun.org',
  'port'       => 587,
  'from'       => ['address' => 'kejyun@gmail.com', 'name' => 'KeJyun'],
  'encryption' => 'tls',
  'username'   =>'postmaster@mailgun.kejyun.com',
  'password'   => 'abcdefghijklmnopqrstuvwxyz123456',
  'sendmail'   => '/usr/sbin/sendmail -bs',
  'pretend'    => false,
];
```

## 設定 `config/services.php`

> domain 設定為你自己定義的 domain，若沒有自己定義 domain 的話，可以使用 Mailgun 替你產生的 domain，可以看看 `Default SMTP Login` 後面的 `sandboxXXXXXX.mailgun.org`，這個為 Mailgun 產生的 domain

> secret 設為 Mailgun 提供的 `API Key`，會長的像 `key-abcdefghijklmnopqrstuvwxyz123456`

設定完後會像這樣：


```php
// config/services.php
[
    'mailgun' => [
        'domain' => 'mailgun.kejyun.com',
        'secret' => 'key-abcdefghijklmnopqrstuvwxyz123456',
    ],
]
```

## 測試使用 Mailgun 寄信

```php
Mail::raw('測試使用 Laravel 5 的 Mailgun 寄信服務', function($message)
{
    $message->to('kejyun@gmail.com');
});
```

這樣我們就可以使用 Mailgun 去當作我們的郵件寄送服務了！！！


## 參考資料
* [Mailgun](https://mailgun.com/)
* [Setting up Mailgun with Laravel 5](http://jamie.sh/tutorials/7/setting-up-mailgun-with-laravel-5)
* [郵件 - Laravel.tw](http://laravel.tw/docs/5.0/mail)
