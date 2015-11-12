# 雜湊

## 使用情境

使用者輸入的密碼，通常我們將其加密再存到資料庫中，但這類的資料我們通常不需要反解回來處理，所以我們不需要使用[加密](../encryption/services-encryption-README.md)的演算法去加密資料

因為加密演算法需要完整的解回原先的資料，所以若資料越長密文也會越長，但雜湊不需要解回原先的資料，只需要驗證原先的資料，經過再雜湊的檢查是相同的就好（輸入的密碼雜湊驗證與原先存在資料庫的雜湊資料相同），所以雜湊的資料可以有固定的長度，像是 md5 的雜湊資料長度固定為 32，而 Laravel 提供的 Hash 雜湊演算法，資料長度固定為 60。

## 設定

在 Laravel 做「雜湊」演算法時，會使用 `config/app.php` 中的 key 值去當作雜湊的 salt，自己的應用需要設定自己的 key 值，若沒有設定的話被加密過的值還是有可能被暴力破解出來，所以要記得去設定，而這個 key 值若變更了，雜湊的驗證也不會相同喔～

## 使用

### 雜湊

```php
// 雜湊
$original_password = '密碼明碼';
$hash_password = Hash::make($original_password);
```

### 驗證

```php
// 雜湊
$original_password = '密碼明碼';
$hash_password = Hash::make($original_password);
// 驗證
$check_result = Hash::check($original_password, $hash_password);
// true
var_dump($check_result);
```

## 備註

重複雜湊相同的資料得到的密文不會一樣，所以不要使用像 md5 的方式去比對密文資料是否相同

### 使用 md5 比較密文

```php
$original_password = '密碼明碼';
// 第 1 次使用 md5 加密的資料
$first_md5_hash_password = md5($original_password);
// 第 2 次使用 md5 加密的資料
$second_md5_hash_password = md5($original_password);
// 資料相同
// true
var_dump($first_md5_encrypt_password === $second_md5_encrypt_password);
```

雖然每次雜湊的結果都不一樣，但你可以放心的將任何一次雜湊的資料存放到資料庫中，因為雖然密文不同，但 Laravel 的雜湊演算法，還是可以比對出來是不是由相同的資料去做雜湊的

### 使用雜湊演算法比較資料是否相同

```php
$original_password = '密碼明碼';
// 第 1 次使用雜湊演算法雜湊的資料
$first_hash_password = Hash::make($original_password);
// 第 2 次使用雜湊演算法雜湊的資料
$second_hash_password = Hash::make($original_password);
// 資料不相同
// false
var_dump($first_encrypt_password === $second_encrypt_password);

// 驗證雜湊資料 true
$first_check_result = Hash::check($original_password, $first_hash_password);
$second_check_result = Hash::check($original_password, $second_hash_password);
```


## 參考資料
* [雜湊 - Laravel.tw](http://laravel.tw/docs/5.1/hashing)
