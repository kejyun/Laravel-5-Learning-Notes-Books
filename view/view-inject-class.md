# 將 Class 綁定到視圖

```php
<?php

namespace App;

class Shop {
    // 顯示商店名稱
    public function name() {
        return 'KJ Shop';
    }
}
```

## View Composer

```php
View::composer('shop', function($view){
    $view->with('shop', app(\App\Shop::class));
});
```


## Blade Inject

```html
@inject('shop', \App\Shop::class)

<h1>{{ $shop->name() }}</h1>
```
