# 驗證 Access Token

## 路由設定 OAuth middleware

在我們想要保護的資源中，我們可以加入 `oauth` 的 middleware 中介層，這樣

```php
// 保護的資源設定 oauth middleware
Route::get('/user-private-resource', ['middleware'=>'oauth', function()
{
    // 取得資源擁有者編號，若經過 Password Grant 驗證的 Token 則為使用者的編號
    $user_id = Authorizer::getResourceOwnerId();
    $data = [
        'user_id'=> $user_id
    ];
    // 回傳受保護的資源
    return Response::json($data);
}]);
```

## 透過 Access Token 取的受保護的資源

在我們取得受保護資源的請求 `Header` 中填入，這樣 OAuth2 就會取得我們傳入的 Access Token，並驗證我們有沒有存取權限，若有存取權限，則會直接回傳該受保護的資源

| 欄位名稱 | 資料 |
|---|---|
| Authorization | Bearer rXXoWMg5UXzQpxAynCnYYOQiZQ0xDcAjT0ywGvke |

![透過 Access Token 取的受保護的資源](./images/oauth2-get-protected-resource-by-access-token.png)


## 路由設定包含 Scope 的 OAuth middleware

若我們限制此路由需要有特定 Scope 的存取權限的 Access Token 才可以存取，我們可以在 middleware 後面加上允許的 Scope 名稱，若同時需要擁有多個 Scope 權限，則用 `加號（+）` 把 Scope 連接起來即可

> e.g. 'oauth:user_profile+user_likes'

```php
// 保護的資源設定 oauth middleware
Route::get('/user-private-resource', ['middleware'=>'oauth:user_profile+user_likes', function()
{
}]);
```

這樣這個路由就會被指定的 Scope 所保護著了～


## 參考資料
* [Securing your API endpoints](https://github.com/lucadegasperi/oauth2-server-laravel/wiki/Securing-your-API-endpoints#defining-scopes)
