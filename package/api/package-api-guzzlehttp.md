# GuzzleHttp


## 設定 API 主網址

在參數設定 `base_uri` 即可設定主網址，之後所有的 Request 都會依照這個主網址去做請求

```php
use GuzzleHttp\Client;

$client = new Client([
    // Base URI is used with relative requests
    'base_uri' => 'http://httpbin.org',
]);
```

## 設定 Timeout 時間

API 為了避免請求時間過久影響到系統存取的效能，會設定 API 存取時間，可以在參數設定至 `timeout` 欄位限定 API 存取時間限制

```php
use GuzzleHttp\Client;

$client = new Client([
    // You can set any number of default request options.
    'timeout'  => 2.14,
]);
```

## 取得回傳資料

可以使用強制轉換 `$GuzzleHttpResponse->getBody()` 型別為 string 去取得回傳的資料

```php
$repsonse_content = (string) $GuzzleHttpResponse->getBody();
```

也可以使用 `$GuzzleHttpResponse->getBody()->getContents()` 去取得回傳的資料

但使用這個方法去取得資料後，下次再呼叫一次後會無法取得資料，若想要下次還能夠取得資料的話，可以用上面的，可以用上面的方式去轉成字串

```php
$repsonse_content = $GuzzleHttpResponse->getBody()->getContents();
```


## 參考資料
* [Quickstart — Guzzle Documentation](http://docs.guzzlephp.org/en/stable/quickstart.html)
* [Request and Response Messages — Guzzle Documentation](https://docs.guzzlephp.org/en/5.3/http-messages.html)
* [php - Guzzle 6: no more json() method for responses - Stack Overflow](https://stackoverflow.com/questions/30530172/guzzle-6-no-more-json-method-for-responses)
