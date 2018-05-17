# Intervention Image 圖片套件

## 安裝

```
$ php composer require intervention/image
```

***設定 config/app.php***

```php
'providers' => [
	Intervention\Image\ImageServiceProvider::class,
],
'aliases' => [
	'Image' => Intervention\Image\Facades\Image::class,
],
```

***設定檔***


```php
php artisan vendor:publish --provider="Intervention\Image\ImageServiceProviderLaravel5"
```

## config/image.php 設定檔

```
return [
    // 套件支援 "gd", "imagick" 圖片處理驅動
    'driver' => 'gd'
];
```

## 記憶體

圖片處理非常消耗記憶體，必須確保 PHP 能夠有權限使用較多的記憶體資源，從 3000 x 2000 像素的圖片 resize 到 300 x 200 像素，可能需要大約 32 MB 的記憶體。

* [memory_limit](http://www.php.net/manual/en/ini.core.php#ini.memory-limit)
* [upload_max_filesize](http://www.php.net/manual/en/ini.core.php#ini.upload-max-filesize)


## URL 圖片自動操作

Intervention 可以讓你自訂義對指定資料夾的圖片做 Filter 的功能，在 `config/imagecache.php` 設定檔中，可以設定虛擬的 *route*

```php
// config/imagecache.php
return [
	'route' => 'img',
];
```

在 *path* 可以指定虛擬的 *route* 要存取哪些資料夾的圖片資料，設定完成後，所有透過 `http://yourhost.com/img/` 網址存取的圖片都會到 `public/upload` 及 `public/images` 資料夾去做圖片的存取。

```php
// config/imagecache.php
return [
	'paths' => array(
        public_path('upload'),
        public_path('images')
    ),
];
```

在 *templates* 中可以設定圖片的 filter 種類

```php
return [
	'templates' => array(
        'small' => 'Intervention\Image\Templates\Small',
        'medium' => 'Intervention\Image\Templates\Medium',
        'large' => 'Intervention\Image\Templates\Large',
    ),
];
```

可以看到 `Intervention\Image\Templates\Small` 檔案中有 Small filter 的程式，會自動將圖片 fit 到 120x90 的大小

```php
<?php

namespace Intervention\Image\Templates;

use Intervention\Image\Image;
use Intervention\Image\Filters\FilterInterface;

class Small implements FilterInterface
{
    public function applyFilter(Image $image)
    {
        return $image->fit(120, 90);
    }
}
```

網址的格式是

```
http://yourhost.com/{route-name}/{template-name}/{file-name}
```

所以當網址輸入 `http://yourhost.com/img/small/xxx.jpg` 就會自動去將 `public/upload` 及 `public/images` 資料夾下的 xxx.jpg 去做 Small filter  處理，並存在快取中，快取時間可以在 *lifetime* 中設定。

```php
// config/imagecache.php
return [
	'lifetime' => 43200,
]
```

Intervention 有提供 `original` 及 `download` 這兩個 filter，如果用 `http://yourhost.com/img/original/xxx.jpg` 可以存取到原始圖片，而用 `http://yourhost.com/img/download/xxx.jpg` 則可以下載圖片，這是 Intervention 提供的 filter。



## 建立圖片方式

```php
// create a new image resource from file
$img = Image::make('public/foo.jpg');

// or create a new image resource from binary data
$img = Image::make(file_get_contents('public/foo.jpg'));

// create a new image from gd resource
$img = Image::make(imagecreatefromjpeg('public/foo.jpg'));

// create a new image directly from an url
$img = Image::make('http://example.com/example.jpg');

// create a new image directly from Laravel file upload
$img = Image::make(Input::file('photo'));
```



## 參考資料
* [Intervention Image - Introduction](http://image.intervention.io/getting_started/introduction)
* [Intervention Image - Url](http://image.intervention.io/use/url)


!INCLUDE "../../../kejyun/book/laravel-5-for-beginner.md"
