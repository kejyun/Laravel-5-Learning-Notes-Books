# 安裝 Let's Encrypt

為了推廣 SSL 憑證，Let's Encrypt 提供了免費的 SSL 憑證，可以讓你們的主機也有 SSL 加密的保障

但是 Let's Encrypt 提供的憑證有效期限每次只有 `90 天`的效期，若過期之後需要重新更新憑證方可繼續使用

## 複製 letsencrypt 套件

```sh
user@webserver:~$ git clone https://github.com/letsencrypt/letsencrypt
user@webserver:~$ cd letsencrypt
user@webserver:~/letsencrypt$ ./letsencrypt-auto --help
```

## 安裝 SSL 憑證

在第一次安裝 SSL 憑證時需要將您的 Nginx 主機服務關閉，才能進行憑證驗證

```sh
./letsencrypt-auto certonly --standalone --email kejyun@gmail.com -d kejyun.dev -d www.kejyun.dev
```

* `--email` 設定您的 Email
* `-d` 設定您要申請憑證的網域，加入多個 `-d` 可以同時設定同主機多組的憑證


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

### 設定 nginx 設定檔

在原有的伺服器設定檔中加入 `.well-known` 設定

```
server {
    # ...

    location ~ /.well-known {
        allow all;
    }

    # ...
}
```

### 建立 Let's Encrypt 設定檔

複製在原本 letsencrypt 目錄下的範例設定檔 `examples/cli.ini`

```sh
user@webserver:~$ cd letsencrypt
user@webserver:~/letsencrypt$ sudo cp examples/cli.ini /usr/local/etc/le-renew-webroot.ini
```

### 編輯自訂設定檔

```sh
sudo vim /usr/local/etc/le-renew-webroot.ini
```

`le-renew-webroot.ini` 設定

```
rsa-key-size = 4096

email = kejyun@gmail.com

domains = kejyun.dev, www.kejyun.dev

webroot-path = /home/kejyun/laravel52/public
```

### 測試自動更新憑證

```
user@webserver:~$ cd letsencrypt
user@webserver:~/letsencrypt$ ./letsencrypt-auto certonly -a webroot --renew-by-default --config /usr/local/etc/le-renew-webroot.ini
```

### 使用 Script 自動更新憑證

下載憑證更新 shell script，並將 Script 設定為可執行檔案

```
sudo curl -L -o /usr/local/sbin/le-renew-webroot https://gist.githubusercontent.com/thisismitch/e1b603165523df66d5cc/raw/fbffbf358e96110d5566f13677d9bd5f4f65794c/le-renew-webroot
sudo chmod +x /usr/local/sbin/le-renew-webroot
```

`le-renew-webroot` Script 內容如下，他會自動去抓取 `/usr/local/etc/le-renew-webroot.ini` 設定資料並進行憑證更新，若憑證還有 30 天以上才過期，則不更新憑證

```sh
#!/bin/bash

web_service='nginx'
config_file="/usr/local/etc/le-renew-webroot.ini"

le_path='/opt/letsencrypt'
exp_limit=30;

if [ ! -f $config_file ]; then
        echo "[ERROR] config file does not exist: $config_file"
        exit 1;
fi

domain=`grep "^\s*domains" $config_file | sed "s/^\s*domains\s*=\s*//" | sed 's/(\s*)\|,.*$//'`
cert_file="/etc/letsencrypt/live/$domain/fullchain.pem"

if [ ! -f $cert_file ]; then
	echo "[ERROR] certificate file not found for domain $domain."
fi

exp=$(date -d "`openssl x509 -in $cert_file -text -noout|grep "Not After"|cut -c 25-`" +%s)
datenow=$(date -d "now" +%s)
days_exp=$(echo \( $exp - $datenow \) / 86400 |bc)

echo "Checking expiration date for $domain..."

if [ "$days_exp" -gt "$exp_limit" ] ; then
	echo "The certificate is up to date, no need for renewal ($days_exp days left)."
	exit 0;
else
	echo "The certificate for $domain is about to expire soon. Starting webroot renewal script..."
        $le_path/letsencrypt-auto certonly -a webroot --agree-tos --renew-by-default --config $config_file
	echo "Reloading $web_service"
	/usr/sbin/service $web_service reload
	echo "Renewal process finished for domain $domain"
	exit 0;
fi
```

### 測試使用 shell script 更新憑證

```sh
sudo le-renew-webroot
Checking expiration date for kejyun.dev...
The certificate is up to date, no need for renewal (89 days left).
```

### 加入 Cronjob 排程執行憑證更新

編輯 crontab

```sh
sudo crontab -e
```

設定每個禮拜一的凌晨 2:30 進行一次憑證的檢查及更新

```c
30 2 * * 1 /usr/local/sbin/le-renew-webroot >> /var/log/le-renewal.log
```

這樣我們就有了一個半永久的 SSL 憑證了！！


## 參考資料
* [letsencrypt/letsencrypt](https://github.com/letsencrypt/letsencrypt)
* [How To Secure Nginx with Let's Encrypt on Ubuntu 14.04 | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-14-04)
