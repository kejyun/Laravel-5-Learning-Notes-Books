# Laravel Mix

## 安裝

確認系統有安裝 node 及 npm

```
node -v
npm -v
```


## 套件安裝除錯

在安裝過程中出現 `node_modules/pngquant-bin/vendor/pngquant binary doesn't seem to work correctly` 的訊息，如下：

```
$ npm install

> pngquant-bin@4.0.0 postinstall /home/web-laravel55/node_modules/pngquant-bin
> node lib/install.js

  ⚠ The `/home/web-laravel55/node_modules/pngquant-bin/vendor/pngquant` binary doesn't seem to work correctly
  ⚠ pngquant pre-build test failed
  ℹ compiling from source
  ✔ pngquant pre-build test passed successfully
  ✖ Error: pngquant failed to build, make sure that libpng-dev is installed
    at Promise.all.then.arr (/home/web-laravel55/node_modules/pngquant-bin/node_modules/bin-build/node_modules/execa/index.js:231:11)
    at process._tickCallback (internal/process/next_tick.js:68:7)
npm WARN ajv-keywords@3.4.1 requires a peer of ajv@^6.9.1 but none is installed. You must install peer dependencies yourself.
npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@1.2.9 (node_modules/fsevents):
npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@1.2.9: wanted {"os":"darwin","arch":"any"} (current: {"os":"linux","arch":"x64"})

npm ERR! code ELIFECYCLE
npm ERR! errno 1
npm ERR! pngquant-bin@4.0.0 postinstall: `node lib/install.js`
npm ERR! Exit status 1
npm ERR!
npm ERR! Failed at the pngquant-bin@4.0.0 postinstall script.
npm ERR! This is probably not a problem with npm. There is likely additional logging output above.

npm ERR! A complete log of this run can be found in:
npm ERR!     /home/kejyun/.npm/_logs/2019-07-30T04_22_17_230Z-debug.log
```

可以在 Ubuntu 安裝 `libpng-dev` 套件，即可正常安裝此套件

```
sudo apt-get install libpng-dev
npm install -g pngquant-bin
```


```
npm run dev
```


## 參考資料
* [Compiling Assets (Laravel Mix) - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/5.5/mix)
* [node_modules/pngquant-bin/vendor/pngquant` binary doesn't seem to work correctly · Issue #78 · imagemin/pngquant-bin · GitHub](https://github.com/imagemin/pngquant-bin/issues/78)
