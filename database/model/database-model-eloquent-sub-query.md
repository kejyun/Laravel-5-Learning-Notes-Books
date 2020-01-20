# 子查詢

當我想要計算子查詢的數量時，會想要執行像下方的 SQL 查詢語法

```sql
SELECT count(*)
FROM (
    SELECT UID
    FROM `Posts`
    WHERE `status` = 1
    GROUP BY `user_id`
) sub
```

在 Eloquent 可以用下面方式達到子查詢的目的

```php
<?php

// Eloquent Builder instance
$SubQuery = Posts::where('status', 1)
    ->groupBy('user_id');

$count = DB::table( DB::raw("({$SubQuery->toSql()}) as sub") )
    ->mergeBindings($SubQuery->getQuery())
    ->count();
```

記得當你的子查詢結束後，若有更多的條件需要執行，則必須將查詢條件放在 `mergeBindings()` 方法後方，否則原本 SubQuery 的查詢資料順序會綁定錯誤

```php
$count = DB::table( DB::raw("({$SubQuery->toSql()}) as sub") )
    // ->where(..) 這裡會出錯
    ->mergeBindings($SubQuery->getQuery())
    // ->where(..) 正確
    ->count();
```



# 參考資料
* [sql - How to select from subquery using Laravel Query Builder? - Stack Overflow](https://stackoverflow.com/questions/24823915/how-to-select-from-subquery-using-laravel-query-builder)
