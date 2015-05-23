# 使用中繼模型繼承（extends）Eloquent 模型造成無法使用大量資料新增（Mess Assignment）

大部份的情況可能專案較小，所以我們會直接使用模型（Model）去新增資料，但若專案較大時，且不同的模型之間有共用的方法的話，我會會希望這些模型繼承同一個 Eloquent 模型的中繼類別物件，就像這樣：

***Eloquent 模型的中繼類別物件***

```php
class CustomBaseModel extends Model
{
    public $someVariable = null;

    public function doSomething()
    {
    }
}
```

***使用者模型繼承「Eloquent 模型的中繼類別物件」***

```php
class User extends CustomBaseModel {

    protected $fillable = ['first_name', 'last_name', 'email'];

}
```

使用這樣的中繼類別時，如果我們只有`設定變數`或`實作中繼模型類別方法`時，我們可以運作的很正常，但是如果我們需要實作中繼類別的`建構子__construct()`時，我們必須要時做原本 Eloquent Model 類別的建構子，否鑿會無法正常的運作原有的 Eloquent 模型

在 `vendor/laravel/framework/src/Illuminate/Database/Eloquent/Model.php` Eloquent 模型的檔案中，我們可以看到`建構子__construct()`有需要傳入資料表欄位的屬性值 `$attributes`。

```php
// vendor/laravel/framework/src/Illuminate/Database/Eloquent/Model.php

abstract class Model implements ArrayAccess, Arrayable, Jsonable, JsonSerializable, QueueableEntity, UrlRoutable {

    public function __construct(array $attributes = array())
    {
        $this->bootIfNotBooted();

        $this->syncOriginal();

        $this->fill($attributes);
    }
}
```

這個部分是用來做大量資料新增或異動時（Mass Assignment）需要用到的資料，所以如果我們在中繼類別沒有實作這個`建構子__construct()`，會讓我們的完整 Eloquent Model 出現問題

所以在 Eloquent 中繼類別中我們必須要時作的`建構子__construct()`會長的像這樣：

```php
class CustomBaseModel extends Model
{
    public $someVariable = null;

    function __construct(array $attributes = array())
    {
        parent::__construct($attributes);

        // 做中繼類別建構子想要做的事
        $this->someVariable = '5566';
    }
}
```

我們的中繼類別，需要傳入資料表欄位的屬性值 `$attributes`，並執行母類別 Eloquent Model 的建構子，這樣我們的 Eloquent 模型就能夠正常運作了！
