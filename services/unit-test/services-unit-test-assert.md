# Assert

## 狀態碼測試

### assertOk ： 測試回傳狀態碼為 200

### assertForbidden ： 測試回傳狀態碼為 403

### assertNotFound ： 測試回傳狀態碼為 404

### assertSuccessful ： 測試回傳狀態碼為 200 ~ 299


```php
<?php
$response = $this->json('GET', '/api/status-code');

// 測試狀態碼
$response->assertOk();          // 狀態碼 200
$response->assertNotFound();    // 狀態碼 404
$response->assertForbidden();   // 狀態碼 403
$response->assertSuccessful();   // 狀態碼 200 ~ 299
```

### assertStatus ： 測試指定狀態碼

```php
<?php
$response = $this->json('GET', '/api/status-code');

// 測試狀態碼
$response->assertStatus(201);   // 狀態碼 201
```

### assertRedirect ： 測試是否為重新導向狀態碼

> 重新導向狀態碼：201, 301, 302, 303, 307, 308

```php
<?php
$response = $this->call('GET', '/redirect-uri');

// 測試狀態碼
$response->assertRedirect($redirect_uri);
```

### 狀態測試

### assertLocation ： 測試目前網址位置

```php
<?php
$response = $this->call('GET', '/specific-location');

// 測試是否包含指定資料
$response->assertLocation('/specific-location');
```


## 資料測試

### assertSee ： 測試是否有包含指定資料

```php
<?php
$response = $this->call('GET', '/hello-world');

// 測試是否包含指定資料
$response->assertSee('<h1>Hello World</h1>');
```

### assertSeeInOrder ： 測試是否看到指定順序資料

```php
<?php
$response = $this->call('GET', '/hello-world');

// 測試是否沒有包含指定資料
$response->assertSeeInOrder([
    '<h1>',
    'Hello',
    'World',
    '</h1>',
]);
```

### assertSeeText ： 測試是否看到指定文字（不包含 html tag）

```php
<?php
$response = $this->call('GET', '/hello-world');

// 測試是否包含指定資料
$response->assertSeeText('Hello World');
```

### assertSeeTextInOrder ： 測試是否看到指定順序文字（不包含 html tag）

```php
<?php
$response = $this->call('GET', '/hello-world');

// 測試是否沒有包含指定資料
$response->assertSeeTextInOrder([
    'Hello',
    'World',
]);
```

### assertDontSee ： 測試是否沒有看到指定資料


```php
<?php
$response = $this->call('GET', '/hello-world');

// 測試是否沒有包含指定資料
$response->assertDontSee('<h1>No Hello World</h1>');
```

### assertDontSeeText ： 測試是否沒有看到指定文字


```php
<?php
$response = $this->call('GET', '/hello-world');

// 測試是否沒有包含指定資料
$response->assertDontSeeText('No Hello World');
```

## Cookie 測試

### assertPlainCookie ： 測試是否為未加密 Cookie

```php
<?php
$response = $this->call('GET', '/get-cookie');

// 測試是否包含指定資料
$response->assertPlainCookie('cookie-name');
```


### assertCookie ： 測試指定 Cookie 是否有包含指定鍵值資料

```php
<?php
$response = $this->call('GET', '/get-cookie');

// 測試是否包含指定資料
$response->assertCookie('cookie-name', 'cookie-value', $encrypted = true, $unserialize = false);
```

### assertCookieExpired ： 測試指定鍵值 cookie 是否失效

```php
<?php
$response = $this->call('GET', '/get-cookie');

// 測試是否包含指定資料
$response->assertCookieExpired('expired-cookie-name');
```


## JSON 測試

### assertJson ： 測試 JSON 子集合資訊

> 測試的資料，僅需為原始資料的子集合即可

#### *回傳資料*

```json
{
    "status" : true,
    "data": {
        "title": "Nam saepe earum molestias consequuntur et doloremque ea.",
        "body": "Consequatur iure omnis distinctio tempore accusamus...",
        "active": false
    }
}
```

#### *測試 JSON 子集合資訊*

```php
$response = $this->json('GET', '/api/json');

// 測試成功
$response->assertJsonFragment([
    'status' => true,
]);

// 測試成功
$response->assertJsonFragment([
    'status' => true,
    'data' => [
        "active"=> false
    ]
]);
```




### assertJsonFragment ： 測試 JSON 部分鍵值資訊

#### *回傳資料*

```json
{
    "status" : true,
    "data": {
        "title": "Nam saepe earum molestias consequuntur et doloremque ea.",
        "body": "Consequatur iure omnis distinctio tempore accusamus...",
        "active": false
    }
}
```

**測試成功**

```php
$response = $this->json('GET', '/api/json');

// 測試成功
$response->assertJsonFragment([
    'status' => true,
]);

// 測試成功
$response->assertJsonFragment([
    'status' => true,
    'data' => [
        "title" => "Nam saepe earum molestias consequuntur et doloremque ea.",
        "body"=> "Consequatur iure omnis distinctio tempore accusamus...",
        "active"=> false
    ]
]);
```

#### *測試 JSON 部分鍵值資訊*

> 若有要指定測試鍵值的資料，則測試的鍵值資料必須包含所有的資訊

**測試成功**

```php
$response = $this->json('GET', '/api/json');

// 測試成功
$response->assertJsonFragment([
    'status' => true,
]);

// 測試成功
$response->assertJsonFragment([
    'status' => true,
    'data' => [
        "title" => "Nam saepe earum molestias consequuntur et doloremque ea.",
        "body"=> "Consequatur iure omnis distinctio tempore accusamus...",
        "active"=> false
    ]
]);
```

**測試失敗**

```
// 測試失敗
$response->assertJsonFragment([
    'status' => true,
    'data' => [
        "title" => "Nam saepe earum molestias consequuntur et doloremque ea.",
    ]
]);
```



### assertJsonStructure ： 測試 JSON 結構

#### *回傳資料*

```json
{
    "status" : true,
    "data": {
        "title": "Nam saepe earum molestias consequuntur et doloremque ea.",
        "body": "Consequatur iure omnis distinctio tempore accusamus...",
        "active": false
    }
}
```

#### *測試 JSON 結構*

```php
$response = $this->json('GET', '/api/json');

$response->assertJsonStructure([
    'status',
    'data' => [
        'body',
        'title',
        'active'
    ]
])
```

### assertExactJson ： 測試 JSON 資料是否完全符合

#### *回傳資料*

```json
{
    "status" : true,
    "data": {
        "title": "Nam saepe earum molestias consequuntur et doloremque ea.",
        "body": "Consequatur iure omnis distinctio tempore accusamus...",
        "active": false
    }
}
```

#### *測試 JSON 結構*

```php
$response = $this->json('GET', '/api/json');

$response->assertExactJson([
    'status' => true,
    'data' => [
        "title" => "Nam saepe earum molestias consequuntur et doloremque ea.",
        "body"=> "Consequatur iure omnis distinctio tempore accusamus...",
        "active"=> false
    ]
])
```

## 參考資料
* [Testing JSON APIs, specifically: assertJsonStructure](https://laracasts.com/discuss/channels/testing/testing-json-apis-specifically-assertjsonstructure)
