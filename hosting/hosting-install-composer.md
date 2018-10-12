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


## proc_open(): fork failed - Cannot allocate memory 無法配置記憶體安裝套件

**1. 可以將 php.ini 的 memory_limit 提高**

```
vim /etc/php/7.1/fpm/php.ini
vim /etc/php/7.1/cli/php.ini
```

```
memory_limit = 1024M
```

**2. 使用 SWAP 增加系統虛擬記憶體**

```
sudo -s
cd /var
fallocate -l 4G swapfile.1
chmod 600 swapfile.1

mkswap /var/swapfile.1
swapon /var/swapfile.1
```

* [虛擬記憶體（SWAP） · ubuntu 學習筆記](https://kejyuntw.gitbooks.io/ubuntu-learning-notes/system/system-virtual-memory.html)


## 參考資料
* [Composer Download](https://getcomposer.org/download/)




!INCLUDE "../kejyun/book/laravel-5-for-beginner.md"
