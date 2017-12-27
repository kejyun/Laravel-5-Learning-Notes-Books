# 安裝 composer

## 下載 composer

```shell
curl -sS https://getcomposer.org/installer | php
```

## 將 composer 執行檔移動到系統環境變數路徑

移動過去系統環境變數路徑中，這樣所有系統的使用者才都可以執行

```shell
sudo mv composer.phar /usr/bin/composer
```

## 安裝所有 Laravel 專案套件

在我們將程式使用 git 推送到主機時，vendor 下所有的套件都不會被推送到主機，所以程式碼上去主機後，需要自己作安裝套件的動作

```shell
cd /home/kejyun/laravel52
composer install
```

這樣我們就完成了 composer 的安裝跟安裝 Laravel 專案套件了

## 參考資料
* [Composer Download](https://getcomposer.org/download/)


!INCLUDE "../kejyun/book/laravel-5-for-beginner.md"
