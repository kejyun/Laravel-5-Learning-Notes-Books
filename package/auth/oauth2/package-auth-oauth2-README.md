# OAuth2 認證套件

## 使用套件

> 套件名稱：[lucadegasperi/oauth2-server-laravel](https://github.com/lucadegasperi/oauth2-server-laravel)

> 套件版本：5.0.3

## 名詞解釋

| 名詞  | 描述  |
|---|---|
| Access token  |  存取標記，用於存取受保護資源的標記 |
| Authorization code  | 授權碼，使用者授權 Client 的中介標記，Client 可以透過此授權碼去取得 Access token  |
| Authorization server  | 授權伺服器，使用者授權 Client 後，用於發送 Access token 的伺服器 |
| Client  | 被授權客戶，存取屬於使用者的受保護資源的應用程式（Application）， 像是 Server、手機或其他裝置 |
| Grant  | 授權方法，存取 Access token 的方法  |
| Resource server   | 資源伺服器，屬於使用者的受保護資源（像是文章、照片、個人隱私資料...等等）  |
| Scope   |  資源存取範圍，Access token 允許存取的權限 |

> ref： [lucadegasperi/oauth2-server-laravel - Terminology](https://github.com/lucadegasperi/oauth2-server-laravel/wiki/Terminology)

## 安裝

### 使用 composer 安裝套件

在 `composer.json` 加入 `"lucadegasperi/oauth2-server-laravel": "5.0.*"` 並執行 `composer update` 安裝套件

```json
/*composer.json*/
"require": {
  "lucadegasperi/oauth2-server-laravel": "5.0.*"
}
```

```shell
$ composer update
```

### 設定套件

開啟`config/app.php`檔案，並將系列套件資訊加入 `providers` 與 `aliases`

```php
'providers' => [
    LucaDegasperi\OAuth2Server\Storage\FluentStorageServiceProvider::class,
    LucaDegasperi\OAuth2Server\OAuth2ServerServiceProvider::class,
],
'aliases'=>[
    'Authorizer' => LucaDegasperi\OAuth2Server\Facades\Authorizer::class,
]
```

### 設定 middleware

開啟 `app/Http/Kernel.php`，將系列設定加入 $middleware 與 $routeMiddleware 中

並將原本在 $middleware 的 `\App\Http\Middleware\VerifyCsrfToken::class` 移至 $routeMiddleware，並命名為 csrf

> 注意，原本 Laravel 會針對每個非 Get 的 Request 做 CSRF 的過濾，因為 OAuth 已經取得授權 Access token，不需要 CSRF 驗證，所以如果你需要做 CSRF 驗證的話，您可以在 Route 加入 `csrf` 的 middleware 去做驗證

```php
<?php

class Kernel extends HttpKernel
{
    protected $middleware = [
        \LucaDegasperi\OAuth2Server\Middleware\OAuthExceptionHandlerMiddleware::class
    ];

    protected $routeMiddleware = [
        // CSRF
        'csrf' => \App\Http\Middleware\VerifyCsrfToken::class,
        // OAuth2
        'oauth' => \LucaDegasperi\OAuth2Server\Middleware\OAuthMiddleware::class,
        'oauth-user' => \LucaDegasperi\OAuth2Server\Middleware\OAuthUserOwnerMiddleware::class,
        'oauth-client' => \LucaDegasperi\OAuth2Server\Middleware\OAuthClientOwnerMiddleware::class,
        'check-authorization-params' => \LucaDegasperi\OAuth2Server\Middleware\CheckAuthCodeRequestMiddleware::class
    ];
}

```

### 產生套件設定及 Migration

執行 `php artisan vendor:publish` 複製套件的 `migration` 與 `config/oauth.php` 設定檔到你應用程式的目錄，並執行 migration 建立 OAuth2 需要的資料表

```shell
$ php artisan vendor:publish
$ php artisan migrate
```

### 設定檔名詞解釋

| 參數名稱  | 說明  | 資料類型  |  預設值 |
|---|---|---|---|
|  grant_types | 授權類型設定  | Array  | 無  |
|  token_type | token 類型  | String  | League\OAuth2\Server\ TokenType\Bearer  |
|  state_param | 狀態參數，如果為 `true` 的話，在請求 Access token 時候則必須帶入 `&state=隨機字串`，在透過授權後 Authorization server 會回覆相同的 state 字串  | Boolean  |  false |
|  scope_param  | 存取權限參數，若為 `true`，則在每一次的 Request 都需要帶入該資源的存取參數  | Boolean  | false  |
|  scope_delimiter | 存取權限區隔字串，在不同的存取權限使用什麼字元去區隔（scope1,scope2）  | String  |  , |
|  default_scope | 預設存取權限  | String  |  null |
|  access_token_ttl | Access token 存活時間（單位：秒）  |  Integer | 3600  |
|  limit_clients_to_grants | 是否限制 Client 允許使用的 Grant Type，可以在 `oauth_client_grants` 資料表設定  | Boolean  | false  |
|  limit_clients_to_scopes | 限制 Client 允許的 Scope，可以在 `oauth_client_scopes` 資料表設定  | Boolean  | false  |
|  limit_scopes_to_grants | 限制 Scope 允許在哪個 Grant Type 存取，可以在 `oauth_grant_scopes` 資料表設定  | Boolean  | false  |
|  http_headers_only |  是否只使用 Header 做 Access token 的檢查 | Boolean  |  false  |

### OAuth2 Access token 取得的方式

OAuth2 總共有下列 4 種 Access token 取得的方式

| Access token 取得方式 | 說明 |
|---|---|---|
| [Client Credentials](https://github.com/lucadegasperi/oauth2-server-laravel/wiki/Implementing-an-Authorization-Server-With-the-Client-Credentials-Grant)  | 純 Client 資料身份驗證，僅需要 client_id 與 client_secret 正確即可取得 Access token |
| [Password](https://github.com/lucadegasperi/oauth2-server-laravel/wiki/Implementing-an-Authorization-Server-with-the-Password-Grant) | 資源擁有者須登入帳號密碼，確認可以讓 Client 取得 Access token |
| [Auth Code Grant](https://github.com/lucadegasperi/oauth2-server-laravel/wiki/Implementing-an-Authorization-Server-with-the-Auth-Code-Grant) | 資源擁有者授權 Client 可以存取指定 Scope 的資源並給予授權碼，Client 透過此授權碼取得該 Scope 的 Access token |
| [Refresh Token Grant](https://github.com/lucadegasperi/oauth2-server-laravel/wiki/Implementing-an-Authorization-Server-with-the-Refresh-Token-Grant) | 授權 Client 在取得 Access token 時也一併取得 Refresh token，在 Access token 過期時，Client 可以用此 Refresh token 取得新的相同權限的 Access token |


### 設定存取 Access token 路由

不管是何種 OAuth2 Access token 取得的方式，都須透過`相同的路由`去取得 Access token，OAuth2 會根據每個取得方式參數的不同做不一樣的驗證，所以我們只有設定下列路由即可

```php
Route::post('oauth/access_token', function() {
    return Response::json(Authorizer::issueAccessToken());
});
```

### 設定 client_id 與 client_secret

每一個 Client 應用程式都會有自己的 `client_id` 與 `client_secret`，你可以自己設定你要核發給 Client 授權 client_id 與 client_secret 的頁面，並將資料分別存放在 `oauth_clients` 資料表的 id 與 secret 欄位中

```php
Schema::create('oauth_clients', function (BluePrint $table) {
    $table->string('id', 40)->primary();    // client_id
    $table->string('secret', 40);           // client_secret
    $table->string('name');                 // Client 名稱
    $table->timestamps();
    $table->unique(['id', 'secret']);
});
```

Client 資料新增完成之後，之後 Client 可以透過這個 client_id 與 client_secret 去要求取得 Access token，我們這裡以新增 id 為 `KeKyun` 及 secret 為 `KeJyunSecret` 作為之後的範例

```sql
INSERT INTO "oauth_clients" ("id", "secret", "name", "created_at", "updated_at")
VALUES ('KeJyun', 'KeJyunSecret', 'KeJyun Client', now(), now());
```

> client_id 與 client_secret 欄位長度皆為 40，在產生資料時請自行產生長度 40 的亂數字串，這裡僅用於示範使用非亂數的 client_id 與 client_secret

## 參考資料
* [lucadegasperi/oauth2-server-laravel](https://github.com/lucadegasperi/oauth2-server-laravel)
* [OAuth 2.0 筆記 (1) 世界觀](https://blog.yorkxin.org/posts/2013/09/30/oauth2-1-introduction/)
* [OAuth 2.0 筆記 (2) Client 的註冊與認證](https://blog.yorkxin.org/posts/2013/09/30/oauth2-2-cilent-registration/)
* [OAuth 2.0 筆記 (3) Endpoints 的規格](https://blog.yorkxin.org/posts/2013/09/30/oauth2-3-endpoints/)
* [OAuth 2.0 筆記 (4.1) Authorization Code Grant Flow 細節](https://blog.yorkxin.org/posts/2013/09/30/oauth2-4-1-auth-code-grant-flow/)
* [OAuth 2.0 筆記 (4.2) Implicit Grant Flow 細節](https://blog.yorkxin.org/posts/2013/09/30/oauth2-4-2-implicit-grant-flow/)
* [OAuth 2.0 筆記 (4.3) Resource Owner Password Credentials Grant Flow 細節](https://blog.yorkxin.org/posts/2013/09/30/oauth2-4-3-resource-owner-credentials-grant-flow/)
* [OAuth 2.0 筆記 (4.4) Client Credentials Grant Flow 細節](https://blog.yorkxin.org/posts/2013/09/30/oauth2-4-4-client-credentials-grant-flow/)
* [OAuth 2.0 筆記 (5) 核發與換發 Access Token](https://blog.yorkxin.org/posts/2013/09/30/oauth2-5-issuing-tokens/)
* [OAuth 2.0 筆記 (6) Bearer Token 的使用方法](https://blog.yorkxin.org/posts/2013/09/30/oauth2-6-bearer-token/)
* [OAuth 2.0 筆記 (7) 安全性問題](https://blog.yorkxin.org/posts/2013/09/30/oauth2-7-security-considerations/)
* [各大網站 OAuth 2.0 實作差異](https://blog.yorkxin.org/posts/2013/09/30/oauth2-implementation-differences-among-famous-sites/)
* [Introduction to authentication with OAuth2](https://www.youtube.com/watch?v=rw_zSCbzRRA)
* [Laravel 4 OAuth2](https://www.youtube.com/watch?v=viQY_s-M0fM)
* [Testing OAuth2 in Laravel 5](https://www.youtube.com/watch?v=nITbqFwFMG8)
