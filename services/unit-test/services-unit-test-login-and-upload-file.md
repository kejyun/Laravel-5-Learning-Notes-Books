# 單元測試登入及上傳檔案

## 登入使用者

```
$User = User::find(12345);
$this->be($User);
```

## 上傳檔案

```php
// 設定上傳檔案
$post_file = new UploadedFile($path, $name, filesize($path), 'image/png', null, true);
// 呼叫上傳網址
$response = $this->call('POST', '/photo/store', [], [], $post_file);
// 取得回傳內容
$content = json_decode($response->getContent());
dump($content);
```

## 參考資料
* [php - How to test file upload in Laravel 5.2 - Stack Overflow](https://stackoverflow.com/questions/36408134/how-to-test-file-upload-in-laravel-5-2)
* [How to mock authentication user on unit test in Laravel?](https://medium.com/yish/how-to-mock-authentication-user-on-unit-test-in-laravel-1441d491d82c)