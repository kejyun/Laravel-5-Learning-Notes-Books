# 安裝 Nginx

## 更新系統套件

```shell
sudo apt-get update
```

## 安裝 Nginx

使用 Ubuntu 內建的 nginx 套件安裝，安裝完後預設的 nginx 設定檔是 `/etc/nginx/sites-available/default`，網站目錄會在 `/usr/share/nginx/html`

```shell
sudo apt-get install nginx
```

## 設定虛擬主機 Virtualhost 設定

編輯新的 virtualhost 設定檔案

在這裡通常我會用主機網域名稱當作他的檔案名稱，如果我有一個網域是 `kejyun.dev`，則我就會用 `kejyun.dev` 當作虛擬主機設定檔名稱

```shell
sudo vim /etc/nginx/sites-available/kejyun.dev
```

#### ***設定 Listen 的 port***

設定主機要使用哪一個 port 傾聽 HTTP 請求

```
listen 80;
```

#### ***設定服務主機名稱***

設定你申請的網域名稱，nginx 會以 HTTP Request 的網域不同導向不同的 Virtualhost，所以一定要設定，以下以 `kejyun.dev` 為例

```
server_name kejyun.dev;
```

#### ***設定網站根目錄路徑***

我們將 Laravel 5 的程式放在使用者 `kejyun` 的家目錄下，而我們必須要將網站路徑指定到 Laravel 專案下的 `public` 目錄下才可以正常執行 Laravel 專案

```
root "/home/kejyun/laravel52/public";
```

#### ***設定 Log 路徑***

設定當 Request 發生錯誤的時候，本 Virtualhost 要將 Log 存放在哪個檔案

```
error_log  /var/log/nginx/kejyun.dev-error.log error;
```

### 完整虛擬主機設定

設定檔中有包括`設定 php 檔案處理方式`，在這邊我們可以先設定，等之後安裝完 php 7 時就可以直接使用

```
server {
    # 設定 Listen 的 port
    listen 80;

    # 設定服務主機名稱
    server_name kejyun.dev;

    # 設定網站根目錄路徑
    root "/home/kejyun/laravel52/public";

    # 設定讀取檔案優先順序
    index index.html index.htm index.php;

    # 設定網站編碼
    charset utf-8;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }

    access_log off;
    # 設定 Log 路徑
    error_log  /var/log/nginx/kejyun.dev-error.log error;

    sendfile off;

    client_max_body_size 100m;

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

    location ~ /\.ht {
        deny all;
    }
}
```


## 連結虛擬主機 Virtualhost 設定

Nginx 虛擬主機設定主要是讀取 `/etc/nginx/sites-enabled/` 目錄下的所有檔案，如果要讓此設定檔案啟用，則必須要將原設定檔目錄 `/etc/nginx/sites-available/` 的設定檔案使用軟連結連結過去

```
sudo ln -s /etc/nginx/sites-available/kejyun.dev /etc/nginx/sites-enabled/kejyun.dev
```


## 重新啟動 nginx

重新啟動 nginx 以讀取新的設定

```
sudo service nginx restart
```

這樣我們就完成了 Nginx Server 的設定了！！


## 參考資料
* [How To Set Up nginx Virtual Hosts (Server Blocks) on Ubuntu 12.04 LTS](https://www.digitalocean.com/community/tutorials/how-to-set-up-nginx-virtual-hosts-server-blocks-on-ubuntu-12-04-lts--3)
* [php - How do I change the NGINX user? - Server Fault](http://serverfault.com/questions/433265/how-do-i-change-the-nginx-user)