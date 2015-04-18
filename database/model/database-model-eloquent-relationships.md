# Eloquent 關聯


假如我們有兩個模型，「文章（Article）」及「使用者（Users）」，假設一個情境，1 個使用者可以寫多篇的文章，但 1 篇文章只能被 1 個使用者發表

如果我們想要透過關聯關係，從使用者模型去取得使用者的文章，就像：

```php
// 取得使用者編號 1 的物件
$user = \App\Users::find(1);

// 取得使用者的所有發表的文章
$user->articles();
```

我們會想要使用者模型內設定這樣的關聯關係，就像：


```php
class Users extends Model {

    // 設定使用者擁有許多文章
    public funciton articles() {
        return $this->hasMany('App\Article');
    }

}
```

如果我們想透過關聯關係，從文章模型去取得是哪一個使用者發表文章，就像：

```php
// 取得文章編號 1 的物件
$article = \App\Article::find(1);

// 取得發表文章的使用者資訊
$user = $article->user();
```

```php
class Article extends Model {

    // 設定文章屬於某一的使用者
    public funciton user() {
        return $this->belongsTo('App\User');
    }

    public funciton owner() {
        return $this->belongsTo('App\User');
    }

    public funciton writer() {
        return $this->belongsTo('App\User');
    }

}
```

設定關聯屬性的函式名稱可以自訂，看自己覺得什麼樣的名稱適合自己就可以了，自訂完後一樣可以使用關聯的方式，撈取出發表文章使用者的資訊

```php
// 取得文章編號 1 的物件
$article = \App\Article::find(1);

// 取得發表文章的使用者資訊
$owner_user = $article->owner();

$writer_user = $article->writer();
```

設定完之後，必須確定文章（Article）資料表有使用者編號（user_id）的外來鍵欄位

```php
<?php
// database/migrations/2015_04_13_154720_create_article_table.php

use Illuminate\Database\Schema\Blueprint;
use Illuminate\Database\Migrations\Migration;

class CreateArticleTable extends Migration {

    public function up()
    {
        Schema::table('article', function(Blueprint $table)
        {
            // 發表文章使用者編號
            $table->integer('user_id')->unsigned();

            // 設定外來鍵
            $table->foreign('user_id')
                  ->references('id')
                  ->on('users')
                  ->onDelete('cascade');
        });
    }

    public function down()
    {
        Schema::drop('users');
    }

}

```

## 參考資料
* [Eloquent Relationships - Laracasts](https://laracasts.com/series/laravel-5-fundamentals/episodes/14)
