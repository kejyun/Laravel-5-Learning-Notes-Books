# Unit test 變更請求網址(Root Url)

在單元測試(Unit test)的時候，測試網址預設會抓取 `.env` 檔案的 `APP_URL`，但若同個專案有不同的網址時，則需要再寫測試時變更為特定的網址

可以在 `setUp()` 函式使用 `\URL::forceRootUrl()` 強制轉換網址，這樣就可以使用指定的網址進行測試了

```php
<?php
class TestCase {

    function setUp(): void
    {
        parent::setUp();
        $app_url= "http://kejyun.com";
        \URL::forceRootUrl($app_url);
    }

}
```