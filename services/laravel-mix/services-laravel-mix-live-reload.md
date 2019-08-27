# LiveReload

使用 Live Reload 可以在我們異動 css 的時候，自動幫我們重新在瀏覽器重新刷新頁面，這樣我們在做畫面設計的時候，就可以更快的設計除錯


1. 安裝 webpack-livereload-plugin

```
npm install webpack-livereload-plugin@1 --save-dev
```


2. 設定 `webpack.mix.js`

```javascript
var LiveReloadPlugin = require('webpack-livereload-plugin');

// LiveReload 設定
mix.webpackConfig({
    plugins: [
        new LiveReloadPlugin()
    ]
});

// 產生 sass 檔案
mix.sass('public/assets/scss/app.scss', 'public/assets/css/');
```

3. 安裝 Chrome LiveReload 套件

在 [LiveReload - Chrome 線上應用程式商店](https://chrome.google.com/webstore/detail/livereload/jnihajbhpnppcggbcgedagnkighmdlei) 安裝此套件


4. 設定 LiveReload 至 blade 樣板

```
@if(config('app.env') == 'local')
    <script src="http://localhost:35729/livereload.js"></script>
@endif
```

5. 監控 LiveReload 運作

```
npm run watch
```

## 參考資料
* [LiveReload | Laravel Mix Documentation](https://laravel-mix.com/docs/4.1/livereload)
* [LiveReload - Chrome 線上應用程式商店](https://chrome.google.com/webstore/detail/livereload/jnihajbhpnppcggbcgedagnkighmdlei)
* [webpack-livereload-plugin/README.md at master · statianzo/webpack-livereload-plugin](https://github.com/statianzo/webpack-livereload-plugin/blob/master/README.md)
