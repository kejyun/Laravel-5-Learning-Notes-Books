# 加密

## 使用情境

我們若需要在資料庫儲存一些敏感資料（像是信用卡的資料），但我們又為了避免資料庫遭到入侵，而導致所有使用者相關的敏感資料全都被竊取，我們可以使用 Laravel 提供的「加密與解密」演算法，將我們的敏感資料加密儲存到資料庫，待我們讀取資料的時候，再將其資料解密出來處理。

## 設定

在 Laravel 做「加密與解密」演算法時，會使用 `config/app.php` 中的 key 值去當作加解密的 salt，自己的應用需要設定自己的 key 值，若沒有設定的話被加密過的值還是有可能被暴力破解出來，所以要記得去設定喔～


## 使用

```php
// 加密
$original_data = '需要加密的資料';
$encrypt_data = Crypt::encrypt($original_data);
// 解密
$decrypted = Crypt::decrypt($encrypt_data);
```

## 參考資料
* [加密 - Laravel.tw](http://laravel.tw/docs/5.1/encryption)
