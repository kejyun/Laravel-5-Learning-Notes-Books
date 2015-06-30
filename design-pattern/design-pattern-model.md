# Model 模型設計模式

我們在使用任何的 Framework 中，都會聽到 MVC 模型，V（View）是負責畫面顯示，C（Controller）是負責控制程式呼叫模型的邏輯，而最重要的 M（Model）是負責整個資料庫的操作，以及撈取資料的邏輯

我們常常把模型用來作為處理資料的商業邏輯，不管是任何的「資料樣式的轉換」、「資料撈取的邏輯」、「資料格式的驗證」、「資料處理的順序及商業邏輯」...等等都是放在模型（Model）去處理

***資料樣式的轉換***


```php
// 2016-01-01 00:00:00.123789
$now = Carbon::now();

// 2016/01/01
$now_date = $now->format('Y/m/d');
```

***資料撈取的邏輯***

*撈取所有的女會員資料，年紀小於 30 歲*

```php
User::where('gender'=>'female')
  ->where('age', '<', 30)
  ->get();
```

*撈取所有的男會員資料，年紀大於 30 歲*

```php
User::where('gender'=>'male')
  ->where('age', '>', 30)
  ->get();
```

*資料格式的驗證*

```php
$validator = Validator::make(Request::all(), [
  'title' => 'required|unique:posts|max:255',
  'content' => 'required',
]);
```

*資料處理的順序及商業邏輯*

```php
/**
 * 發送 Email 及簡訊給所有女會員
 */

// 取得所有女會員資料
$users = User::where('gender'=>'female')
  ->get();

// 發送 Email
foreach ($users as $u) {
  Mail::send('emails.hello', ['user' => $u], function ($mail) use ($u) {
      $mail->to($u->email, $u->name)
           ->subject('安安!');
  });
}

// 發送簡訊
foreach ($users as $u) {
  SMS::send('sms.hello', ['user' => $u], function ($sms) use ($u) {
      $sms->to($u->mobile_phone, $u->name)
          ->content('安安!');
  });
}
```


如果把這些不同類別的資料全部丟到 Model 模型去處理會變得很亂，程式碼難以維護，所以我們會用設計模式來降低程式碼的耦合性，讓程式變得容易維護，我們會將 Model 分成：

* 實體（Entity）
* 資源庫（Repository）
* 服務（Service）
* 表單驗證（Form）
* 資料呈現（Presenter）
* ARCA 架構檔案結構

## 實體（Entity）

實體就是我們用來設定 Eloquent Model 的相關設定，像是資料表名稱（$table）、主鍵名稱（$primaryKey) 等等，裡面除了 Eloquent 相關設定以外，***不要擺任何的商業邏輯或資料撈取方法***

*實體與資料表的關係是「1 對 1」的關係，有幾個資料表就有幾個實體*

> 詳情請見[Eloquent Model (模型) - 設定](../database/model/database-model-eloquent-config.md)

## 資源庫（Repository）

資源庫是我們要用來撈取資料表資料的各個邏輯，我們資料表會有不同的欄位，不同的欄位條件代表不同的意義，像是：

*撈取所有的女會員資料，年紀小於 30 歲*

```php
User::where('gender'=>'female')
  ->where('age', '<', 30)
  ->get();
```

*撈取所有的男會員資料，年紀大於 30 歲*

```php
User::where('gender'=>'male')
  ->where('age', '>', 30)
  ->get();
```

這些不同的撈取資料邏輯，我會將它包在資源庫中，該資源庫長得會像這樣：

```php
class UserRepository {
  /**
   * 撈取所有的女會員資料，年紀小於 30 歲
   */
  public function getYoungFemale()
  {
    return User::where('gender'=>'female')
      ->where('age', '<', 30)
      ->get();
  }

  /**
   * 撈取所有的男會員資料，年紀大於 30 歲
   */
  public function getOldMale()
  {
    return User::where('gender'=>'male')
      ->where('age', '>', 30)
      ->get();
  }
}
```

這樣我們撈取這些不同資料邏輯時就可以這樣去撈取：

```php
$userRepository = new UserRepository();

// 撈取所有的女會員資料，年紀小於 30 歲
$young_female_user = $userRepository->getYoungFemale();

// 撈取所有的男會員資料，年紀大於 30 歲
$old_male_user = $userRepository->getOldMale();
```

這樣除了可以讓程式碼易讀性提高之外，撈取資料的邏輯也可以抽離出來，下次如果有需要撈取同樣的資料時，就可以重複的去使用它，而且不會有重複的程式碼出現在專案的各個地方，讓管理程式碼變得簡單

*資源庫與實體的關係是「1 對 1」的關係，有幾個實體就有幾個資源庫，每個資源庫是代表那個實體的各個不同的資料撈取邏輯*


## 服務（Service）

服務代表我們程式要處理資料的商業邏輯，我會將各個功能邏輯獨立成一個服務，像是使用者「註冊身份驗證」是一個服務，而使用者「個人隱私設定」也是一個服務

*服務與資源庫的關係是「多 對 1」的關係，像是同樣使用者資料，有「註冊身份驗證」及「個人隱私設定」2 種不同類型的服務*


***使用者「註冊身份驗證」服務***

```php
/**
 * 使用者「註冊身份驗證」服務
 */
class UserAuthService
{
  /**
   * 註冊
   */
  public function signup()
  {

  }

  /**
   * 登入驗證
   */
  public function signin()
  {

  }
}
```

***使用者「個人隱私設定」服務***

```php
/**
 * 使用者「個人隱私設定」服務
 */
class UserPrivacyService extends AnotherClass
{
  /**
   * 取得使用者隱私設定
   */
  public function getUserPrivacy()
  {

  }

  /**
   * 設定使用者隱私
   */
  public function setUserPrivacy()
  {

  }
}
```

不同類型的服務，只要彼此耦合性很低，我傾向把他分成不同的服務去處理，這樣可以很清楚的知道哪個個服務是專門處理哪一種商業邏輯，程式也比較好管理，在異動程式時也比較不會影響到彼此，避免牽一髮動全身的狀況發生


## 表單驗證（Form）

我們設計後端程式的原則，是不要相信任何第三方傳來的資料，在資料做進一步處理時都需要對資料格式做檢查，若於我們設定的資料格式相符，我們才會去做進一步的資料商業邏輯處理

但是我們可能會在控制器（Controller）做表單資料的驗證，但是服務（Service）、資源庫（Repository）或實體（Entity）為了保護自己的程式邏輯，也有可能去做表單資料的驗證，若每一個階段都做表單資料的驗證，這樣不僅造成了資料發生重複驗證的狀況，也會降低程式的執行速度，更慘的是會造成驗證程式重複出現，如果有驗證規則要修改，我們就必須要確保所有有驗證表單資料的地方，都有正確的被修改，不然程式的商業邏輯可能會沒辦法順利的去執行。

因為我們對模型做了分層地處理，所以模型的層級架構會像：

> 控制器 (Controller） >  服務（Service） >  資源庫（Repository） >  實體（Entity）

控制器會根據他需要的商業邏輯，呼叫不同的服務來處理他的程式邏輯，而且每個控制器，而且每個控制器可能會有不同類型的服務，可能會有使用者（User）的資料、文章（Posts）的資料...等等需要做資料的驗證，所以驗證資料的規則複雜度會很多。

我自己會傾向將所有的表單資料驗證都放在服務（Service）中去驗證，不同的商業邏輯可能需要驗證的資料規則不同，但是我們可以確定的是，同一個服務會是同一個類型的資料，像是*使用者「註冊身份驗證」服務*及*使用者「個人隱私設定」服務*<
裡面的資料一定是使用者相關的資料，若我們也有*文章的服務（PostService）*，我們也一定可以確保裡面的驗證資料一定是文章相關的資料。

所以除了服務（Service）層去做資料的驗證外，其他的層級都不需要做任何的資料驗證！

## 資料呈現（Presenter）

我們會將需要處理不同資料樣式的邏輯，使用 [laracasts/presenter](https://github.com/laracasts/Presenter) 去做實體（Entity）的分層處理，不要將有程式邏輯的功能出現在實體（Entity）中

## ARCA 架構檔案結構

> to be continued...

## 參考資料
* [在 Laravel 4 使用資源庫 (Repositories) 及服務 (Services) 去降低程式的耦合性](http://laravel4-book.kejyun.com/laravel-design-pattern/model/decoupling-your-code-in-laravel-using-repositiories-and-services.html)
* [胖胖Model減重的五個方法 by howtomakeaturn](http://slides.com/howtomakeaturn/model#/)
* [PHP 也有 Day #16 - 胖胖 Model 減重的五個方法 by 尤川豪](https://www.youtube.com/watch?v=e0qVLniXbHw)
