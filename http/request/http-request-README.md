# HTTP 請求

這裏會介紹如何在 Laravel 5 驗證 HTTP 請求的資料

## 建立新的請求驗證

如果我們有文章（Article）的模型，我們在每次請求過程中想要驗證傳入的資料，我們可以使用系列指令建立要驗證的請求：

```shell
$ php artisan make:request CreateArticleRequest
```

請求驗證的檔案會被建立在 `app\Http\Requests` 目錄下，建立的檔案內容如下

```php
<?php namespace App\Http\Requests;

// app\Http\Requests\CreateArticleRequest.php
use App\Http\Requests\Request;

class CreateArticleRequest extends Request {

    /**
     * Determine if the user is authorized to make this request.
     * 驗證使用者是否要登入狀態
     *
     * @return bool
     */
    public function authorize()
    {
        return true;
    }

    /**
     * Get the validation rules that apply to the request.
     * 驗證請求的資料規則
     *
     * @return array
     */
    public function rules()
    {
        return [
          // 使用 | 設定驗證規則
          'title'         => 'required|min:3',
          'body'          => 'required|min:30',

          // 使用陣列設定驗證規則
          'published_at'  => [
            'required',
            'date',
          ],
        ];
    }

}
```

在驗證請求的 CreateArticleRequest 中的 rules() 函式，除了僅回傳驗證規則外，你也可以判斷不同的狀況去加入不同的規則再回傳，像是：

```php
<?php namespace App\Http\Requests;

// app\Http\Requests\CreateArticleRequest.php
use App\Http\Requests\Request;

class CreateArticleRequest extends Request {
    public function rules()
    {
        $rules = [
          // 使用 | 設定驗證規則
          'title'         => 'required|min:3',
          'body'          => 'required|min:30',

          // 使用陣列設定驗證規則
          'published_at'  => [
            'required',
            'date',
          ],
        ];

        // 其他條件判斷
        if ($condition) {
            $rules['something_else'] = 'required';
        }

        return $rules;
    }

}
```


## 指定 Controller 函式處理指定的請求驗證

在我們使用 Controller 去處理請求時，我們可以再傳入變數內設定要怎麼處理請求：

```php
class ArticleController extends Controller {

    // 新增文章
    public function store(App\Http\Requests\CreateArticleRequest $request)
    {
        Article::create(Request:all());
        // OR
        // Article::create($request->all());

        return redirect('articles');
    }

}
```

這樣設定之後，所有的 HTTP 請求的 Input 資料都會經過 `App\Http\Requests\CreateArticleRequest` 驗證，如果有經過驗證才會繼續執行後面的新增文章動作，否則的話則會丟出驗證錯誤的物件到原頁面。


## 驗證錯誤訊息

這裏要注意到，視圖（View）的每一頁 Laravel 都會將驗證錯誤物件（Illuminate\Support\ViewErrorBag）包成 `$errors` 變數，所以你可以在每一頁去印出 `$errors` 值，`$errors` 變數儲存的是任何資料驗證錯誤的結果

**判斷是否有任何的錯誤並顯示錯誤訊息**

```php
// 任一 blade 視圖（View）皆可以接收此錯誤變數
@if($errors->any())
    // 有錯誤訊息
    <ul>
        $foreach ($errors->all() as $error)
          <li>{{ $error }}</li>
        @endforeach
    </ul>
@endif
```


## 使用 Controller 內建的 validate 驗證請求的資料

除了建立驗證 Request 物件，也可以直接使用 Controller 內建的 validate 去驗證請求

如果不想要使用內建處理 HttpResponseException 的例外，你也可以自己 try catch 並自己處理例外狀況

```php
class ArticleController extends Controller {

    // 新增文章
    public function store(Requests $request)
    {
        try {
            $this->validate($request, [
              'title'         => 'required|min:3',
              'body'          => 'required|min:30',
              'published_at'  => 'required|date',
            ]);
        } catch (Exception $e) {
            // 自己處理例外狀況
        }


        Article::create(Request:all());
        // OR
        // Article::create($request->all());

        return redirect('articles');
    }

}
```


## 使用者資料

取得 User Agent、Referrer

```php
<?php
// User Agent
request()->server('HTTP_USER_AGENT');

// Referrer
request()->server('HTTP_REFERER')
```




## 參考資料
* [Form Requests and Controller Validation - Laracasts](https://laracasts.com/series/laravel-5-fundamentals/episodes/12)


!INCLUDE "../../kejyun/book/laravel-5-for-beginner.md"
