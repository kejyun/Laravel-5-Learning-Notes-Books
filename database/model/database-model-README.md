# Eloquent Model

這裏會介紹如何在 Laravel 5 使用 Eloquent Model 管理資料庫

## 設定可以大量新增的欄位

Eloquent 為了避免特定欄位（像是 id, created_at ...）被使用者故意傳入大量（Mass）資料去進行修改，所以 Eloquent 會自動保護欄位不被大量異動（Mass Assignment），像是：

```php
// 新增
App\User::Create([
  'first_name'=> 'KeJyun',
  'last_name' => 'Hong',
  'email'     => 'kejyun@gmail.com',
]);


// 更新
$user = App\User::find('1');

$user->update([
  'email'     => 'hello@gmail.com',
]);
```

如果我們需要異動這些欄位，需要在 Model 裡面設定 `$fillable` 的欄位，這樣就可以使用大量資料的方式，去新增或異動資料表欄位資料。

```php
class User extends Model {

    protected $fillable = ['first_name', 'last_name', 'email'];

}
```

## 設定需要被保護的欄位

我們也可以使用 `$guarded` 指定某些欄位需要被保護，能被大量新增或異動

```php
class User extends Model {

    protected $guarded = ['id', 'password'];

}
```

我們也可以設定所有欄位都不能被大量新增或異動

```php
class User extends Model {

    protected $guarded = ['*'];

}
```
