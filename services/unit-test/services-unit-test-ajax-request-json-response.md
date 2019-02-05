# Ajax Request & JSON Response

在 Unit test 需要模擬 Ajax 請求時，可以在 `$server` 參數設定下列設定

```
$server = [
    'HTTP_X-Requested-With' => 'XMLHttpRequest',    // Ajax Request
    'HTTP_ACCEPT'=> 'application/json',             // 請求 JSON Response
];
```

```php
<?php
class ServerTest {
    public function testAjaxRequestAndJsonResponse() {
        $uri = '/test/ajax';
        $server = [
            'HTTP_X-Requested-With' => 'XMLHttpRequest',    // Ajax Request
            'HTTP_ACCEPT'=> 'application/json',             // 請求 JSON Response
        ];
        $res = $this->call('POST', $uri, $parameters, $cookies, $files, $server);
    }
}
```

當設定完 `HTTP_X-Requested-With` 為 `XMLHttpRequest` 時，Laravel 會把這個請求視為 Ajax 請求，所以在呼叫 `request()->ajax()` 方法時會回傳 `true`

```php
request()->ajax();  // true
```

當設定完 `HTTP_ACCEPT` 為 `application/json` 時，Laravel 會把這個請求需要回傳的資訊視為需要 JSON 格式資料，所以在呼叫 `request()->wantsJson()` 方法時會回傳 `true`


```php
request()->wantsJson();  // true
```

## 參考資料
* [How to simulate xmlHttpRequests in a laravel testcase? - Stack Overflow](https://stackoverflow.com/questions/20093897/how-to-simulate-xmlhttprequests-in-a-laravel-testcase)
