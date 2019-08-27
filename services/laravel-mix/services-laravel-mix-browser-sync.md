# BrowserSync

使用 BrowserSync 可以在我們對於 blade 檔案做異動時，自動重新載入頁面


1. 安裝 BrowserSync

```
npm install browser-sync --save-dev
npm install browser-sync-webpack-plugin@2.0.1 --save-dev
```

2. 設定 BrowserSync 至 `webpack.mix.js`

```
mix.browserSync({
    proxy: 'my-domain.test'
});
```


3. 監控 BrowserSync 運作

```
npm run watch
```


## 參考資料
* [BrowserSync | Laravel Mix Documentation](https://laravel-mix.com/docs/4.1/browsersync)
