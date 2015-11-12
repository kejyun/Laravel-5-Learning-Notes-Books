# 使用 Elixir 合併 CSS 與 JS

Laravel CSS 與 JS 相關的資源預設是放在 `resources/assets/scss` 與 `resources/assets/js` 下，我們需要把我們的 CSS 與 JS 放在這個目錄下，才可以使用 Elixir 去編譯我們的 CSS 與 JS 檔案

> 使用 Elixir 之前請先安裝完所需要的軟體，詳情[請點這裡](services-elixir-README.md)

## 使用 Elixir 合併 CSS

### 撰寫 CSS

** resources/assets/scss/a.css **

```css
.a {
  color:green;
}
```

** resources/assets/scss/b.css **

```css
.b {
  color:blue;
}
```

### 合併 CSS 資源

在我們專案根目錄 `gulpfile.js` 檔案中，我們撰寫 `mix.styles(['a.css', 'b.css']);`，告訴 laravel-elixir 將我們的 CSS 資源整合起來

```js
// gulpfile.js
// 載入 laravel-elixir 套件
var elixir = require('laravel-elixir');

// 使用 laravel-elixir 套件合併 CSS
elixir(function(mix) {
    mix.styles(['a.css', 'b.css']);
});
```

### 執行 gulp 整合 CSS

在我們專案根目錄下執行 gulp 指令，使用 laravel-elixir 整合所有 CSS 資源，大概會像這樣：

```shell
kejyun@KeJyundeMBP:~/Code/KeJyunLaravel5Proj$ gulp
[22:33:04] Using gulpfile ~/Code/KeJyunLaravel5Proj/gulpfile.js
[22:33:04] Starting 'default'...
[22:33:04] Starting 'styles'...
[22:33:04] Merging: resources/assets/css/a.css, resources/assets/css/b.css
[22:33:04] Finished 'default' after 89 ms
[22:33:04] Finished 'styles' after 122 ms
```

預設會將 CSS 檔案整合至 `public/css/all.css` 檔案中，你在 `all.css` 會看到我們整合 CSS 檔案的結果，就像這樣

```css
.a {
  color:green;
}
.b {
  color:blue;
}
/*# sourceMappingURL=all.css.map */
```

我們也可以在 `gulpfile.js` 檔案中指定我們要編譯檔案的名稱目錄是什麼，就像下面我把 CSS 編譯的檔案名稱目錄指定為 `public/css/kejyun.css`，laravel-elixir 就會將編譯的檔案設為您指定的名稱目錄

```js
// gulpfile.js
// 載入 laravel-elixir 套件
var elixir = require('laravel-elixir');

// 使用 laravel-elixir 套件合併 CSS
elixir(function(mix) {
  mix.styles(['a.css', 'b.css'], 'public/css/kejyun.css');
});
```

## 使用 Elixir 合併 JS

### 撰寫 JS

** resources/assets/js/a.js **

```js
var a = 1;
```

** resources/assets/js/b.js **

```js
var b = 2;
```

### 合併 JS 資源

在我們專案根目錄 `gulpfile.js` 檔案中，我們撰寫 `mix.scripts(['a.js', 'b.js']);`，告訴 laravel-elixir 將我們的 JS 資源整合起來

```js
// gulpfile.js
// 載入 laravel-elixir 套件
var elixir = require('laravel-elixir');

// 使用 laravel-elixir 套件合併 JS
elixir(function(mix) {
    mix.scripts(['a.js', 'b.js']);
});
```

### 執行 gulp 整合 JS

在我們專案根目錄下執行 gulp 指令，使用 laravel-elixir 整合所有 CSS 資源，大概會像這樣：

```shell
kejyun@KeJyundeMBP:~/Code/KeJyunLaravel5Proj$ gulp
[22:47:21] Using gulpfile ~/Code/KeJyunLaravel5Proj/gulpfile.js
[22:47:21] Starting 'default'...
[22:47:21] Starting 'scripts'...
[22:47:21] Merging: resources/assets/js/a.js, resources/assets/js/b.js
[22:47:21] Finished 'default' after 86 ms
[22:47:21] Finished 'scripts' after 122 ms
```

預設會將 JS 檔案整合至 `public/js/all.js` 檔案中，你在 `all.js` 會看到我們整合 JS 檔案的結果，就像這樣

```js
var a = 1;
var b = 2;
//# sourceMappingURL=all.js.map
```

我們也可以在 `gulpfile.js` 檔案中指定我們要編譯檔案的名稱目錄是什麼，就像下面我把 JS 編譯的檔案名稱目錄指定為 `public/js/kejyun.js`，laravel-elixir 就會將編譯的檔案設為您指定的名稱目錄

```js
// gulpfile.js
// 載入 laravel-elixir 套件
var elixir = require('laravel-elixir');

// 使用 laravel-elixir 套件合併 JS
elixir(function(mix) {
  mix.scripts(['a.js', 'b.js'], 'public/js/kejyun.js');
});
```

## 使用 Elixir 同時合併 CSS 與 JS 檔案

我們可以透過 `Javascript Method Chaining` 的方式連續的指定我們要合併的資源，這樣我們執行 gulp 時，就可以同時整合這些資源

```js
// gulpfile.js
// 載入 laravel-elixir 套件
var elixir = require('laravel-elixir');

// 使用 laravel-elixir 套件合併資源
elixir(function(mix) {
  mix
    .scripts(['a.js', 'b.js'], 'public/js/kejyun.js')
    .styles(['a.css', 'b.css'], 'public/css/kejyun.css');
});
```

laravel-elixir 還可以整合其他的資源，像是 Less、SCSS、SASS、CoffeeScript...等等之類的資源，詳情請看 [Laravel Elixir - laravel.tw](http://laravel.tw/docs/5.1/elixir) 的說明即可！

## 參考資料
* [Elixir Improvements](https://laracasts.com/series/whats-new-in-laravel-5-1/episodes/3)
* [Laravel Elixir - laravel.tw](http://laravel.tw/docs/5.1/elixir)
