# Eloquent 小技巧

## 取得主鍵名稱

User Eloquent 物件

```php
<?php

use Illuminate\Database\Eloquent\Model;

class User extends Model {
    protected $table = 'user';
    protected $primaryKey = 'user_id';
}
```

取得主鍵名稱

```php
<?php

$User = new User;

$primary_key_name = $User->getKeyName(); // user_id
```