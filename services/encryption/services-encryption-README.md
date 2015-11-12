# 加密

## 使用情境

我們若需要在資料庫儲存一些敏感資料（像是信用卡的資料），但我們又為了避免資料庫遭到入侵，而導致所有使用者相關的敏感資料全都被竊取，我們可以使用 Laravel 提供的「加密與解密」演算法，將我們的敏感資料加密儲存到資料庫，待我們讀取資料的時候，再將其資料解密出來處理。

## 設定

在 Laravel 做「加密與解密」演算法時，會使用 `config/app.php` 中的 key 值去當作加解密的 salt，自己的應用需要設定自己的 key 值，若沒有設定的話被加密過的值還是有可能被暴力破解出來，所以要記得去設定，而這個 key 值若變更了，雜湊的驗證也不會相同喔～


## 使用

```php
// 加密
$original_data = '需要加密的資料';
$encrypt_data = Crypt::encrypt($original_data);
// 解密
$decrypted = Crypt::decrypt($encrypt_data);
```

## 備註

重複加密相同的資料得到的密文不會一樣，所以不要使用像 md5 的方式去比對密文資料是否相同

### 使用 md5 比較密文

```php
$original_data = '需要加密的資料';
// 第 1 次使用 md5 加密的資料
$first_md5_hash_data = md5($original_data);
// 第 2 次使用 md5 加密的資料
$second_md5_hash_data = md5($original_data);
// 資料相同
// true
var_dump($first_md5_hash_data === $second_md5_hash_data);
```

### 使用加密演算法比較密文

```php
$original_data = '需要加密的資料';
// 第 1 次使用加密演算法加密的資料
$first_encrypt_data = Crypt::encrypt($original_data);
// 第 2 次使用加密演算法加密的資料
$second_encrypt_data = Crypt::encrypt($original_data);
// 資料不相同
// false
var_dump($first_encrypt_data === $second_encrypt_data);
```

## 參考資料
* [加密 - Laravel.tw](http://laravel.tw/docs/5.1/encryption)
