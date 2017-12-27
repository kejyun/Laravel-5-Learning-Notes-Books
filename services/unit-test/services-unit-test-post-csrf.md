# POST CSRF 錯誤

當我們在使用 Unit test 做 POST 測試時，測試的程式可能像：

```php
<?php

class UserTest extends TestCase {

    /**
     * 測試註冊
     */
    public function testSignup()
    {
        $parameters = [
            'email'=>'kejyun@gmail.com',
            'name'=>'KeJyun'
        ];

        // 傳送參數
        $response = $this->call('POST', '/signup', $parameters);

        $this->assertEquals(200, $response->getStatusCode());
    }
}
```

在執行單元測試後，你會收到一個 `TokenMismatchException` 的例外錯誤，這個部分是 Middleware VerifyCsrfToken 的驗證錯誤

這是因為 Laravel 5 在所有的 `POST`、`PUT`、`DELETE` 的路由方法中，都會預設加入 CSRF Token 的檢查，他會檢查 POST 過來的資料中 `_token` 的資料值與 Session 中的 token 是否相符，或是驗證標頭中的 `X-CSRF-TOKEN` 是否相符。

所以在我們每一次做 `POST`、`PUT`、`DELETE` 的請求時，我們都必須要將 CSRF Token 帶入檢查，才能執行後面的程式動作，我們可以用這樣的方式帶入 CSRF Token：

```php
<?php

class UserTest extends TestCase {

    /**
     * 測試註冊
     */
    public function testSignup()
    {
        // 開啟 Session
        Session::start();

        // 參數加入 CSRF token
        $parameters = [
            '_token'=>csrf_token(),
            'email'=>'kejyun@gmail.com',
            'name'=>'KeJyun'
        ];

        // 傳送參數
        $response = $this->call('POST', '/signup', $parameters);

        $this->assertEquals(200, $response->getStatusCode());
    }
}
```

在使用 `csrf_token()` 方法時，都必須要先使用 `Session::start();` 將 Session 開啟，以紀錄當時的 CSRF Token 做驗證，並將 `_token` 當作參數傳送到 `POST`、`PUT`、`DELETE` 的路由當中，就可以正常執行單元測試了！


## 參考資料
* [HTTP 路由 - Laravel.tw](http://laravel.tw/docs/5.0/routing#csrf-protection)
* [Testing Laravel 5 Routes with CSRF Protection Using PHPUnit](http://davejustdave.com/2015/02/08/laravel-5-unit-testing-with-csrf-protection/)


!INCLUDE "../../kejyun/book/laravel-5-for-beginner.md"
