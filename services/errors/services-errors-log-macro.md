# 日誌巨集

## 前言

我們會用 Laravel 內建的 Response 去回應服務的訊息，我們可能會用的回應會像這樣：

```php
// 建立 JSON 回應
return Response::json(['name' => 'KeJyun', 'Country' => 'Taiwan']);
return response()->json(['name' => 'KeJyun', 'Country' => 'Taiwan']);


// 建立 JSONP 回應
return Response::json(['name' => 'KeJyun', 'Country' => 'Taiwan'])
                 ->setCallback($request->input('callback'));
return response()->json(['name' => 'KeJyun', 'Country' => 'Taiwan'])
                 ->setCallback($request->input('callback'));


// 建立檔案下載的回應
return response()->download($pathToFile);
return response()->download($pathToFile, $name, $headers);
return response()->download($pathToFile)->deleteFileAfterSend(true);
```

在這樣的使用下，我們可以很容易的回應訊息給使用者，但是在伺服器發生程式例外錯誤 (Exception) 時，我們可能也需要回應像是這樣的資料：

```php
return Response::json(['status' => 'failure', 'error_code' => '5566']);
return response()->json(['status' => 'failure', 'error_code' => '5566']);
```

在我們用 Laravel 做 API 給手機用的時候，更需要有這些錯誤狀態的資料，所以我們沒辦法直接像網頁一樣跳出整個的錯誤 debug 畫面

但我們若想在 API 回應給手機這樣的錯誤資訊時，也能夠將例外錯誤記錄下來，以便我們進行除錯，我們可以做一個 Response 的巨集，去處理紀錄我們的回應


## 建立服務提供者

我們在命令列輸入 `php artisan make:provider ResponseServiceProvider` 建立回應的服務提供者

```shell
$ php artisan make:provider ResponseServiceProvider
```

該服務提供者檔案會被建立在 `app/Providers/ResponseServiceProvider.php` 中，命名空間為 `App\Providers\ResponseServiceProvider`

> 我們不一定要將服務提供者的檔案放到 `app/Providers` 目錄中，我們可以依照自己專案的需求，將他移動到像是 `app/KeJyun/Providers` 目錄中，這樣命名空間就會變成 `App\KeJyun\Providers\ResponseServiceProvider`，檔案放置的位置隨自己專案需求而定，只要遵照 PSR-4 的規定去設定命名空間及檔案位置即可


我新增了一個名稱為 jsonLog 的 Response 巨集，該巨集會回應 json 資料，並依照記錄層級紀錄我們傳給他的資訊，ResponseServiceProvider 程式會像這樣

```php
<?php namespace App\KeJyun\Providers;
// app/KeJyun/Providers/ResponseServiceProvider.php

use Illuminate\Support\ServiceProvider;
use Response, Log;

class ResponseServiceProvider extends ServiceProvider {

    /**
     * Bootstrap the application services.
     *
     * @return void
     */
    public function boot()
    {

        /**
         *  註冊 Response 記錄錯誤巨集
         *
         *  @param      Array                   $response_data      回傳的 json 資料
         *  @param      Array|Object|String     $log_data           紀錄的資料
         *  @param      String                  $log_level          紀錄資料的等級（預設為 info）
         *
         *  @return     null
         *
         *  @access     public
         *  @author     KeJyun      kejyun@gmail.com
         *  @date       2015-06-06
         */
        Response::macro('jsonLog', function(
            $response_data,
            $log_data ='No Data Be log!!!',
            $log_level = 'info'
        ) {
            // 增加 Log檔案錯訊息間距以便閱讀
            Log::debug("\n\n\n\n\n");
            Log::debug($response_data);
            Log::debug("\n\n");
            switch ($log_level) {
                case 'debug':
                    Log::debug($log_data);
                    break;
                case 'notice':
                    Log::notice($log_data);
                    break;
                case 'warning':
                    Log::warning($log_data);
                    break;
                case 'error':
                    Log::error($log_data);
                    break;
                case 'critical':
                    Log::critical($log_data);
                    break;
                case 'alert':
                    Log::alert($log_data);
                    break;
                case 'info':
                default:
                    Log::info($log_data);
                    break;
            }

            // 增加 Log檔案錯訊息間距以便閱讀
            Log::debug("\n\n\n\n\n");

            return Response::json($response_data);
        });
    }

    /**
     * Register the application services.
     *
     * @return void
     */
    public function register()
    {
        //
    }

}
```

## 設定服務提供者

設定完自己的 jsonLog 紀錄巨集後，我們需要到 `config/app.php` 設定這個 Response 服務提供者，我的命名空間為 `App\KeJyun\Providers\ResponseServiceProvider`，所以設定會像這樣：

```php
// config/app.php

'providers' => [
    // 其他的服務提供者

    'App\KeJyun\Providers\ResponseServiceProvider',
],
```

設定完之後，Laravel 在啟動時就會自動載入該服務提供者了

## 使用自定的 Response 巨集 jsonLog

在我們撰寫商業邏輯時若發生無法預期的例外狀況，我們會想要紀錄該例外狀況的資料，我們就可以這樣使用 Response jsonLog 巨集：

```php
try{
  // 商業邏輯處理
} catch (Exception $exception) {
    $response_data = [
        'status'=>'failure'
        'error_code'=>5566
    ];
    return response()->jsonLog($response_data, $exception, 'alert');
}
```

這樣在系統發生預期之外的例外時，我們也有參考的資料可以幫我們進行除錯了！！

## 參考資料
* [HTTP 回應：回應巨集 - Laravel.tw](http://laravel.tw/docs/5.0/responses#response-macros)
* [服務提供者 - Laravel.tw](http://laravel.tw/docs/5.0/providers)
