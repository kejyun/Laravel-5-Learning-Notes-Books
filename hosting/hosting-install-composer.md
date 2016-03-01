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

```shell
cd /home/kejyun/laravel52
composer install -vvv
```

## 參考資料
* [Composer Download](https://getcomposer.org/download/)