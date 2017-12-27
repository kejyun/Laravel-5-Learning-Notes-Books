# Eloquent 魔術函式


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

我們可以簡化這個 query，把它寫在 Model 用函式的方式做處理，這樣我們就可以用這樣去取得已發表的文章：

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


## 參考資料
* [Eloquent 101 - Laracast](https://laracasts.com/series/laravel-5-fundamentals/episodes/8)
* [Dates, Mutators, and Scopes - Laracast](https://laracasts.com/series/laravel-5-fundamentals/episodes/11)
* [Eloquent ORM - Laravel.tw](http://laravel.tw/docs/5.0/eloquent)

!INCLUDE "../../kejyun/book/laravel-5-for-beginner.md"
