# Eloquent 設定


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


## 設定欄位為時間資料欄位

我們可以很簡單的使用 Carbon 去做時間的資料處理，預設的 `created_at` 與 `updated_at` 是使用 Carbon 當作儲存的資料格式

```php
$article = \App\Article::find(1);

// created_at = '2014-03-18 23:59:59'

// 取得 created_at 年份
// 2014
dd($article->created_at->year);

// 取得 created_at 月份
// 03
dd($article->created_at->month);

// 取得 created_at 6 天後的時間
// 2014-03-24 23:59:59
dd($article->created_at->addDays(6));

// 取得 created_at 格式化為 Y-m 的時間
// 2014-03
dd($article->created_at->format('Y-m'));

// 取得 created_at 格式化為人可閱讀的時間
// 1 year ago
dd($article->created_at->diffForHumans());
```

若我們自己新增了時間的欄位像是 `published_at`，則 Model 沒有自動將此欄位的資料設為 Carbon 的資料格式

我們可以在 Model 中設定 `$dates` 欄位中的資料，可以指定欄位資料格式為 Carbon 的資料格式


```php
class Article extends Model {

    protected $dates = ['published_at'];
}
```

## 設定資料庫的連線

我們也可以使用 `$connection` 指定模型需要用哪個資料庫連線去做查詢

```php
class User extends Model {

    protected $connection = 'custom_connection_name';

}
```

## 設定主鍵不要自動新增

使用 Eloquent 去建立模型（Model）時，預設主鍵會使用自動新增（Auto-increment）的方式去新增，若要自行定義主鍵時，則要設定 `$incrementing` 為 `false`，將自動新增的功能關閉～


```php
class User extends Model {

  public $incrementing = false;

}
```

## 設定主鍵欄位名稱

使用 Eloquent 去建立模型（Model）時，預設會將主鍵欄位名稱設為 `id`，若有需要異動主鍵欄位名稱的話，則要設定 `$primaryKey` 變數，設為自行定義的欄位名稱

```php
class User extends Model {

  protected $primaryKey = 'my_primary_column_name';

}
```


## 參考資料
* [Eloquent 101 - Laracast](https://laracasts.com/series/laravel-5-fundamentals/episodes/8)
* [Dates, Mutators, and Scopes - Laracast](https://laracasts.com/series/laravel-5-fundamentals/episodes/11)
* [Eloquent ORM - Laravel.tw](http://laravel.tw/docs/5.0/eloquent)
