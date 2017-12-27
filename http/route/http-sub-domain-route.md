# 子網域路由（Sub-Domain Route）

我們可能會因為有多個子網域，而我們希望各個不同的子網域有自己的路由設定，像是我們希望各個子網域的首頁能夠藍道不同的頁面，這個時候我們可以透過子網域路由去幫我們達成這樣的工作


## 加入您的子網域到 hosts 設定

如果是正式環境則不用做此設定，如果是測試環境也想要達到子網域路由的效果，則必須做此設定

開啟 `/etc/hosts` 檔案，並加入您需要的子網域

```sh
127.0.0.1   resume.kejyun.dev
127.0.0.1   book.kejyun.dev
```


## Homestead 加入此子網域的虛擬主機設定

```yaml
sites:
    - map: resume.kejyun.dev
      to: /home/vagrant/Code/KeJyunProject/public
    - map: book.kejyun.dev
      to: /home/vagrant/Code/KeJyunProject/public
```

## 重新讀取 Homestead 設定

若設定檔有修改要重新讀取，則可以使用下列指令重新讀取設定

```sh
vagrant provision
```

## 加入子網域路由

在 `route.php` 檔案中加入子網域路由

```php
Route::group(['domain' => 'resume.kejyun.dev'], function()
{
    Route::get('/', function() {
        return 'KeJyun Resume';
    });
});

Route::group(['domain' => 'book.kejyun.dev'], function()
{
    Route::get('/', function() {
        return 'KeJyun Book';
    });
});
```

這樣我們就可以在 http://resume.kejyun.dev 及 http://book.kejyun.dev 這兩個子網域看到不同的首頁了！

## 參考資料
* [Homestead and Subdomains](https://laracasts.com/discuss/channels/general-discussion/homestead-and-subdomains)

!INCLUDE "../../kejyun/book/laravel-5-for-beginner.md"
