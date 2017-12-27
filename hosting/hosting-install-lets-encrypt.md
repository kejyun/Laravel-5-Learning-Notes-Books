# 安裝 Let's Encrypt

為了推廣 SSL 憑證，Let's Encrypt 提供了免費的 SSL 憑證，可以讓你們的主機也有 SSL 加密的保障

但是 Let's Encrypt 提供的憑證有效期限每次只有 `90 天`的效期，若過期之後需要重新更新憑證方可繼續使用

## 下載

```sh
wget https://dl.eff.org/certbot-auto
chmod a+x certbot-auto
```

## 安裝 SSL 憑證

在安裝 SSL 憑證時需要將您的 Nginx 主機服務關閉，才能進行憑證驗證

```sh
How would you like to authenticate with the ACME CA?
-------------------------------------------------------------------------------
1: Place files in webroot directory (webroot)
2: Spin up a temporary webserver (standalone)
-------------------------------------------------------------------------------
Select the appropriate number [1-2] then [enter] (press 'c' to cancel):
```

用 nginx 選擇 `standalone`，若是 apache 則選擇用 `webroot`


## 輸入驗證 SSL 的網址


```sh
Please enter in your domain name(s) (comma and/or space separated)  (Enter 'c'
to cancel): kejyun.dev
```


## 憑證檔案

安裝完的憑證會依照你申請的 domain 當作資料夾名稱放到 `/etc/letsencrypt/live/` 目錄下

如果我同時申請了 `kejyun.dev` 與 `www.kejyun.dev`，那麼憑證檔案就會分別放在 `/etc/letsencrypt/live/kejyun.dev/` 及 `/etc/letsencrypt/live/www.kejyun.dev/` 目錄下

憑證檔案分別會有 4 個

|  檔案名稱 |  說明 |
|---|---|
|  cert.pem | 申請網域的憑證  |
|  chain.pem |  Let's Encrypt 的憑證 |
|  fullchain.pem | cert.pem 及 chain.pem 合併檔案  |
|  privkey.pem |  申請網域的憑證密鑰 |


## 設定 nginx 使用 SSL 憑證

```conf
server {
    # 設定監聽的 port 為 443
    listen 443 ssl;

    # 設定憑證檔案
    ssl_certificate /etc/letsencrypt/live/kejyun.dev/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/kejyun.dev/privkey.pem;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;
    ssl_ciphers 'EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH';
}
```

## 設定 nginx 80 port 自動轉址導向 SSL 443 port

```conf
server {
    listen 80;
    server_name kejyun.dev;
    return 301 https://$host$request_uri;
}
```

## 重新啟動 Nginx

```sh
sudo service nginx restart
```

設定完成後就可以看到自己的網站有做 SSL 加密了！

## 自動更新 Let's Encrypt 憑證

## 自動更新憑證

```sh
./path/to/certbot renew --pre-hook "service nginx stop" --post-hook "service nginx start"
```

在更新憑證的時候，還是需要把 web server 停掉，所以可以在 `--pre-hook` 更新前關掉 nginx，在 `--post-hook` 更新後啟動 nginx

### 加入 Cronjob 排程執行憑證更新

編輯 crontab

```sh
sudo crontab -e
```

設定每個禮拜一的凌晨 2:30 進行一次憑證的檢查及更新

```c
30 2 * * 1 ./path/to/certbot renew --pre-hook "service nginx stop" --post-hook "service nginx start" >> /var/log/letsencrypt-renewal.log
```

這樣我們就有了一個半永久的 SSL 憑證了！！


## 參考資料
* [Certbot - Nginx on Ubuntu 14.04](https://certbot.eff.org/#ubuntutrusty-nginx)
* [Certbot documentation - Renewing certificates](https://certbot.eff.org/docs/using.html#renewing-certificates)
* [letsencrypt/letsencrypt](https://github.com/letsencrypt/letsencrypt)
* [How To Secure Nginx with Let's Encrypt on Ubuntu 14.04 | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-14-04)


!INCLUDE "../kejyun/book/laravel-5-for-beginner.md"
