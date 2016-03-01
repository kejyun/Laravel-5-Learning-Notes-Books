# 安裝 php 7

## 加入 php 7 套件資源庫

目前（2016/03） php7 沒有在 Ubuntu 的預設套件庫中，所以若要使用 php7 的話，則必須要自行加入此套件庫，這樣我們才能在 Ubuntu 安裝 php7

```shell
sudo add-apt-repository ppa:ondrej/php-7.0
```

## 更新套件資源庫

加入新的套件資源庫後，必須進行系統套件清單更新，才能夠讀取到新的套件設定

```shell
sudo apt-get update
```

## 安裝 php7 套件

* php7.0-fpm ： Nginx 解析 php 檔案的工具
* php7.0-mysql ： 連線 mysql
* php7.0-mcrypt ： Laravel 加解密工具

> 其他套件是我在 Laravel 專案中需要的套件，可以依照自己需求去進行安裝

```shell
sudo apt-get install php7.0-fpm php7.0-mysql php7.0-mcrypt php7.0-gd php7.0-cli php7.0-curl php7.0-imap
```

## 設定 php7.0-fpm

若有需要變更任何 php7.0-fpm 的任何設定，可以修改下列設定檔案

```
sudo vim  /etc/php/7.0/fpm/pool.d/www.conf
```

像是您如果想要把異動傾聽 php 的執行擁有者變更為 `kejyun`，您可以做以下的設定

```
user = kejyun
group = kejyun
listen.owner = kejyun
listen.group = kejyun
listen.mode = 0660
```


## 設定 Nginx php 檔案處理方式

#### 設定虛擬主機 Virtualhost 設定檔

```shell
sudo vim /etc/nginx/sites-available/kejyun.dev
```

#### 設定 php 檔案處理方式

```shell
server {
    # ...

    # 設定 php 檔案處理方式
    location ~ \.php$ {
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass unix:/var/run/php/php7.0-fpm.sock;
        fastcgi_index index.php;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;

        fastcgi_intercept_errors off;
        fastcgi_buffer_size 16k;
        fastcgi_buffers 4 16k;
        fastcgi_connect_timeout 300;
        fastcgi_send_timeout 300;
        fastcgi_read_timeout 300;
    }

    # ...
}
```

## 重新啟動 php

如果有異動任何 php7.0-fpm 的任何設定的話，必須要將 php7.0-fpm 服務重新啟動，才能夠讀取到新的設定

```
sudo service php7.0-fpm restart
```

這樣我們就完成 php7 的設定了！


## 參考資料
* [Install PHP 7 on Ubuntu 14.04 | Enrico Zimuel](http://www.zimuel.it/install-php-7/)
* [kasparsd/php-7-debian: Install PHP 7 on Debian/Ubuntu](https://github.com/kasparsd/php-7-debian)
* [Installing php7-fpm with phpredis extension on Ubuntu 14.04](https://gist.github.com/hollodotme/418e9b7c6ebc358e7fda)
