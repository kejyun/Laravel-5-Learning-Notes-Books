# 使用 Eloquent

## 新增資料

### 大量指定新增資料

```php
// 新增
\App\User::Create([
  'first_name'=> 'KeJyun',
  'last_name' => 'Hong',
  'email'     => 'kejyun@gmail.com',
]);
```

### 填入要新增的資料


```php
// 使用者的資料
$user_data = [
  'first_name'=> 'KeJyun',
  'last_name' => 'Hong',
  'email'     => 'kejyun@gmail.com',
];

$user = new \App\User;

// 填入要新增的資料
$user->fill($user_info);
// 儲存資料
$user->save();
```
