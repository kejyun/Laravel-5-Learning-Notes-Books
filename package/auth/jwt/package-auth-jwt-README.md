# JWT（JSON Web Token）Authentication

## 使用套件

> 套件名稱：[tymondesigns/jwt-auth](https://github.com/tymondesigns/jwt-auth)

> 套件版本：0.5.*

## 安裝

### 使用 composer 安裝套件

在 `composer.json` 加入 `""tymon/jwt-auth": "0.5.*"` 並執行 `composer update` 安裝套件

```json
/*composer.json*/
"require": {
  "tymon/jwt-auth": "0.5.*"
}
```

```shell
$ composer update
```


### 設定套件

開啟`config/app.php`檔案，並將系列套件資訊加入 `providers` 與 `aliases`

```php
'providers' => [
    Tymon\JWTAuth\Providers\JWTAuthServiceProvider::class,
],
'aliases'=>[
    'JWTAuth' => Tymon\JWTAuth\Facades\JWTAuth::class,
    'JWTFactory' => Tymon\JWTAuth\Facades\JWTFactory::class,
]
```

### 產生套件設定

執行 `php artisan vendor:publish --provider="Tymon\JWTAuth\Providers\JWTAuthServiceProvider"` 複製套件的 `config/jwt.php` 設定檔到你應用程式的設定檔目錄


```shell
$ php artisan vendor:publish --provider="Tymon\JWTAuth\Providers\JWTAuthServiceProvider"
```

### 產生 JWT Secret Key

每個應用應該有屬於自己的 JWT Secret Key 用於加密並驗證你的資料

```shell
$ php artisan jwt:generate
```

產生的 JWT Secret Key 會設定在 `config/jwt.php` 中的 `secret` 中

```php
// config/jwt.php
[
    'secret' => env('JWT_SECRET', 'your_personal_jwt_secret_key')
]
```


## 設定檔說明

| 參數名稱  | 說明  | 資料類型  |  預設值 |
|---|---|---|---|
| secret | 密鑰 | String | 無 |
| ttl | JWT token 有效時間（單位：分鐘） | Integer | 60 |
| refresh_ttl | token 允許重新更新時間（單位：分鐘） | Integer | 20160（2 週） |
| algo | 加密演算法 | String | HS256 |
| user | 使用者模型類別位置 | String | App\User |
| identifier | 使用者資料主鍵欄位 | String | id |
| required_claims | 必須包含在 token payload 的資料，若驗證時無這些資料則會拋出 `TokenInvalidException` 例外 | Array | ['iss', 'iat', 'exp', 'nbf', 'sub', 'jti'] |
| blacklist_enabled | 開啟黑名單清單 | Boolean | true |
| providers.user | 找尋使用者類別物件 | String | Tymon\JWTAuth\Providers\ User\EloquentUserAdapter |
| providers.jwt | 用於加解密 JWT Token 的類別物件 | String | Tymon\JWTAuth\Providers\ JWT\NamshiAdapter |
| providers.auth | 取得受驗證使用者的類別物件 | Closure Function | Tymon\JWTAuth\Providers\ Auth\IlluminateAuthAdapter |
| providers.storage | 儲存黑名單 token 的快取物件，token 將會儲存到它過期為止 | Closure Function | Tymon\JWTAuth\Providers\ Storage\IlluminateCacheAdapter |

## 參考資料
* [tymondesigns/jwt-auth - Installation](https://github.com/tymondesigns/jwt-auth/wiki/Installation)
* [tymondesigns/jwt-auth - Configuration](https://github.com/tymondesigns/jwt-auth/wiki/Configuration)
