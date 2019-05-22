# 環境除錯

## Laravel 5 : Parse error: syntax error, unexpected '?', expecting variable (T_VARIABLE)

當安裝 Laravel 5.5 時，出現 `Parse error: syntax error, unexpected '?', expecting variable (T_VARIABLE)` 的訊息


> You need to install PHP version 7.1 because [nullable types were introduced in 7.1](https://php.net/manual/en/migration71.new-features.php):

```
?string $value
```

要解決這個問題只需要使用 php > 7.1.3 版本即可


## 參考資料
* [php - Laravel 5 : Parse error: syntax error, unexpected '?', expecting variable (T_VARIABLE) - Stack Overflow](https://stackoverflow.com/questions/48787078/laravel-5-parse-error-syntax-error-unexpected-expecting-variable-t-var)
