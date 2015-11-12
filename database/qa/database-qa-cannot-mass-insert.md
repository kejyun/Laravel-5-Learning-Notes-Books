# 使用大量資料的方式新增（Mass Assignment）時無法新增

在 Laravel 若沒有在模型（Model）中`同時`設定「可以新增的欄位變數 `$fillable`」及「需要保護的欄位變數 `$guarded`」時，為了安全性著想，在做大量的新增或異動資料時（Mass Assignment），會無法正確的去新增或異動資料。

## 設定「可以新增的欄位變數 `$fillable`」

設定你覺得允許做大量新增的欄位名稱

```php
class User extends Model {

    protected $fillable = ['first_name', 'last_name', 'email'];

}
```


## 設定「需要保護的欄位變數 `$guarded`」

我們可以指定某些欄位，不能被使用大量新增或異動，去變更欄位的資料值

```php
class User extends Model {

    protected $guarded = ['id', 'password'];

}
```

若我們想要讓模型（Model）可以被大量新增，且我們沒有需要保護的欄位時，我們還是需要設定 `$guarded` 變數為空陣列 `[]`，否則 Laravel 預會保護所有的欄位資料，讓你無法進行大量的新增或異動資料

```php
class User extends Model {

    protected $fillable = ['id', 'password', 'first_name', 'last_name', 'email'];
    protected $guarded = [];

}
```

## 參考資料
* [Eloquent ORM 新增、更新、刪除 - Laravel.tw](http://laravel.tw/docs/5.0/eloquent#insert-update-delete)
* [Laravel Eloquent Save to DB Using Create - Unhelpful Error](http://stackoverflow.com/questions/22338149/laravel-eloquent-save-to-db-using-create-unhelpful-error)
* [Eloquent Create Method - Always inserts blank entries.](https://laracasts.com/discuss/channels/general-discussion/eloquent-create-method-always-inserts-blank-entries)
* [Unable to create a model with Eloquent create method. Error telling MassAssignMentException](http://stackoverflow.com/questions/18699866/unable-to-create-a-model-with-eloquent-create-method-error-telling-massassignme)
