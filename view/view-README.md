# 視圖

這裏會介紹如何在 Laravel 5 處理視圖（View）

Laravel 的視圖是放在 `resource/views` 目錄內

## 建立共用的視圖

我們網頁常常會出現 header 跟 footer 在不同的視圖中為相同的狀況，唯一有變的只有中間的內容隨著不同的請求而有變動，如果有這樣的設計需求，我們可以替所有視圖建立共用的視圖，假設我們把這個共用的視圖放在 `resource/view/app.blade.php` 下，其內容可能是：

```html
<!-- resource/view/app.blade.php -->
<!doctype html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <title>我的網站</title>
</head>

<body>
    <div class="container">
        @yield('content')
    </div>

    @yield('other_info')
</body>

</html>
```

如果我們要顯示文章的資訊在 `content` 中，文章的說明在 `other_info` 中，我們可以在 blade 中這樣設定：


```html
<!-- resource/view/article.blade.php -->
@extend('app')

@section('content')
    <h1>文章標題</h1>
    <p>
        Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod
        tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam,
        quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo
        consequat.
    </p>
@stop


@section('other_info')
    其他資訊
@stop
```

這樣 Laravel 就會幫我們把相對應的資訊塞到 `app.blade.php` 當中相對應的位置


## 引入共用的視圖

假如我們要在特定的某幾頁使用 [Facebook 留言板](https://developers.facebook.com/docs/plugins/comments)，像是：

```html
<div id="fb-root"></div>
<script>(function(d, s, id) {
  var js, fjs = d.getElementsByTagName(s)[0];
  if (d.getElementById(id)) return;
  js = d.createElement(s); js.id = id;
  js.src = "//connect.facebook.net/zh_TW/sdk.js#xfbml=1&version=v2.3&appId=12345566";
  fjs.parentNode.insertBefore(js, fjs);
}(document, 'script', 'facebook-jssdk'));</script>

<div class="fb-comments" data-href="http://laravel5-book.kejyun.com/" data-numposts="5" data-colorscheme="light"></div>
```

但我們不想要這些相同的程式碼片段散落在各個不同的視圖中，我們可以把它整理在 `resource/views/vendor/_fbcomment.blade.php` 當中

然後在文章視圖當中我們就可以這樣去引入 [Facebook 留言板](https://developers.facebook.com/docs/plugins/comments)：

```html
<!-- resource/view/article.blade.php -->
@extend('app')

@section('content')
    <h1>文章標題</h1>
    <p>
        Lorem ipsum dolor sit amet
    </p>

    @include('vendor/_fbcomment')
    @include('vendor._fbcomment')
@stop


@section('other_info')
    其他資訊
@stop
```

## 傳入變數到引用的視圖當中

在我們新增與編輯文章的視圖當中，幾乎所有的視圖都是一樣的，不一樣的地方可能只有「表單處理的 action 不同（create & edit）」、「送出的按鈕文字不同（新增違章 & 編輯文章）」

但我們還是希望兩個視圖能夠一起被引用，把其他不同的地方當作變數傳入，就可以達到視圖重構的效果，避免類似的視圖重複出現在不同地方，像是：


```html
<!-- resource/view/partials/articles/_form.blade.php -->
{!! Form::label('title','標題') !!}
{!! Form::text('title', null) !!}

{!! Form::label('content','內文') !!}
{!! Form::text('content', null) !!}

{!! Form::submit($submitButtonText) !!}
```

當我們要引用表單的視圖，則必須把按鈕的文字傳送給表單，像是：


```html
<!-- resource/view/article.blade.php -->
@extend('app')

@section('content')
  @include('partials/articles/_form', ['submitButtonText' => '新增文章'])

  @include('partials/articles/_form', ['submitButtonText' => '編輯文章'])
@stop


@section('other_info')
    其他資訊
@stop
```

## 備註

### 引用或載入視圖路徑

在使用 blade 中的 `@extend()` 或 `@include()` 函數，他所參照的視圖相對位置是從 `resource/views/` 開始的

所以如果你的視圖是放在 `resource/views/partials/other.blade.php` 中，你要引用或載入的話則可以用 `.` 或 `/` 去指定相對的視圖位置，像是：

```html
<!-- 引用 -->
@extend('partials._other')
@extend('partials/_other')

<!-- 載入 -->
@include('partials._other')
@include('partials/_other')
```

### 設計模式

#### 樣板檔案名稱

通常若不是完整的視圖，僅是部分的視圖，通常會將檔案名稱最前面加上底線 `_`，用來告知團隊程式設計師這個 blade 樣板不是完整的樣板

## 參考資訊
* [View Partials and Form Reuse - Laracasts](https://laracasts.com/series/laravel-5-fundamentals/episodes/13)
