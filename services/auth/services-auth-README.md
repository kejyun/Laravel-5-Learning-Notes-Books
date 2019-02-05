# 認證登入（Auth）

## 設定

Laravel 內建認證的設定檔案放在 `config/auth.php` 中，預設會使用 `App\User` 的類別當作驗證的 Eloquent 模型

```php
[
    'model' => App\User::class,
]
```

如果我們用[Model 模型設計模式](../../design-pattern/design-pattern-model.md)去設計我們的程式架構，我們 User 實體模型的程式可能會放在 `App\KeJyunApp\User\Entities\User.php` 中，這時候我們的認證模型設定可以設定成像這樣（依照命名空間去設定）：

```php
[
    'model' => App\KeJyunApp\User\Entities\User::class,
]
```

這樣 Laravel 內建的認證就可以用我們指定的實體模型去進行認證了！！

## 手動登入認證

Laravel 內建的認證使用 Auth 去進行身份認證，如果我們要用使用者的「email」及「密碼」做登入，我們的登入程式可能會像：

```php
$email = 'kejyun@gmail.com';
$password = '1234';

if (Auth::attempt(['email' => $email, 'password' => $password]))
{
    // 已登入成功！！！
}
```

使用 `Auth:attempt()` 的方式去驗證使用者時，Laravel 會先到 User 資料表透過 Email 抓取使用者的資料，產生出來的 SQL 會像：

```sql
SELECT * FROM "users" WHERE "email" = 'kejyun@gmail.com' LIMIT 1;
```

抓取完使用者之料後再將 password 欄位用[雜湊](../hashing/services-hashing-README.md)的 `Hash::check()` 方式去比對驗證密碼是否正確，再記錄使用者的 SESSION 資料

如果我們的使用者有限制啟用帳號的人才可以登入，所以我們要用使用者的「email」、「密碼」及「啟用狀態」做登入，我們的登入程式可能會像：

```php
$email = 'kejyun@gmail.com';
$password = '1234';
$status = 'active';

if (Auth::attempt(['email' => $email, 'password' => $password, 'status' => $status]))
{
    // 已登入成功！！！
}
```

產生出來的 SQL 會像：

```sql
SELECT * FROM "users" WHERE "email" = 'kejyun@gmail.com' AND "status" = 'active' LIMIT 1;
```

Laravel 一樣是先抓取使用者資料後，再做密碼驗證的動作！

## 使用記住我的方式登入

```php
$email = 'kejyun@gmail.com';
$password = '1234';
$remember_me = true;

if (Auth::attempt(['email' => $email, 'password' => $password], $remember_me))
{
    // 已使用記住我登入成功！！！
}
```

> 如果你是自己建立自己 User 資料表的 Migration，記得在自己的 Migration 中加入 `$table->rememberToken();` 的設定，加入 `remember_token` 欄位，這個欄位可以讓 Laravel 使用 `記住我` 的方式去記住使用者的 session token

## 判斷使用者是否已登入

我們可以使用 `Auth::check()` 的方式，判斷使用者是否已登入

```php
if (Auth::check()) {
  // 已登入
}
```

## 取得登入使用者的物件

```php
$user = Auth::user();
// KeJyun
var_dump($user->name);
```

## 使用特定使用者的 ID 登入

```php
$user_id = '1';
Auth::loginUsingId($user_id);
```


## 登出

```php
Auth::logout();
```


## 登出其他裝置使用者

> Laravel 5.6 or higher

```php
Auth::logoutOtherDevices($password);
```

## 參考資料
* [認證 - Laravel.tw](http://laravel.tw/docs/5.0/authentication)
* [Authentication - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/5.6/authentication#invalidating-sessions-on-other-devices)


!INCLUDE "../../kejyun/book/laravel-5-for-beginner.md"
