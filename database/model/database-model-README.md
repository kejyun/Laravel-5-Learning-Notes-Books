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

## 預先處理被異動的欄位資料

在使用 Eloquent 新增或異動資料時，我們可能想要對輸入的資料做預先的處理，我們可以使用 Laravel 提供的魔術函式 `setNameAttribute()` 去預先處理欄位資料。

如果我們要預先處理文章模型（Article）的發布時間欄位（published_at），我們的魔術函式就會是像：

```php
class Article extends Model {
  
    public function setPublishedAtAttribute($date) {
        // 將傳入的 Y-m-d 時間設為 datetime 格式的現在時間
        $this->attributes['published_at'] = Carbon::createFromFormat('Y-m-d', $date);

        // 將傳入的 Y-m-d 時間設為 datetime 格式的凌晨零時 00:00:00
        $this->attributes['published_at'] = Carbon::parse($date);
    }
}
```

> 魔術函式 `setNameAttribute()` 中，若遇到欄位名稱有底線的狀況，則將名稱設為駝峰式大小寫（Camel-Case），像是 `published_at` 則變成 `PublishedAt`

## 自定義 query 處理函式

假如我們要讀取發表的文章，但是發表的時間 `published_at` 必須過去的時間，設定於未來發表時間的文章不能被撈取出來，我們可以用這樣的方式去撈取：

```php
// 取得已經發表的文章，依照發表時間降冪（Desc）排序
$article = \App\Article::latest('published_at')
    ->where('published_at', '<=', Carbon::now())
    ->get();
```

我們可以簡化這個 query，把它寫在 Model 用函式的方式做處理，這樣我們就可以用這樣去取得以發表的文章：

```php
// 取得已經發表的文章，依照發表時間降冪（Desc）排序
$article = \App\Article::latest('published_at')
    ->published()
    ->get();
```

而 Model 裡面我們用 `scopeName` 魔術函式的方式去設定 `published()`：

```php
class Article extends Model {

    public function scopePublished($query) {
        $query->where('published_at', '<=', Carbon::now());
    }

}
```

若我們想要取得尚未被發表的文章資訊，我們模式函式也可以設定成：

```php
class Article extends Model {

    public function scopeUnpublished($query) {
        $query->where('published_at', '>', Carbon::now());
    }

}
```

這樣我們就可以用 `unpublished()` 去設定取得文章資訊了

```php
// 取得已經發表的文章，依照發表時間降冪（Desc）排序
$article = \App\Article::latest('published_at')
    ->unpublished()
    ->get();
```


這樣的優點是:

1. 簡化程式的長度
2. 讓我們在不同的地方不需要寫同樣落落長的查詢
3. 讓查詢的可讀性增加，`published()` 與 `unpublish()` 我們不需要看查詢的語法條件就可以知道這個地方是要做什麼樣的查詢了

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


## 參考資料
* [Eloquent 101 - Laracast](https://laracasts.com/series/laravel-5-fundamentals/episodes/8)
* [Dates, Mutators, and Scopes - Laracast](https://laracasts.com/series/laravel-5-fundamentals/episodes/11)
* [Eloquent ORM - Laravel.tw](http://laravel.tw/docs/5.0/eloquent)
