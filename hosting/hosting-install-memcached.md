# 安裝 Memcached

## 安裝

直接安裝 Ubuntu 的 Memcached 套件

```shell
sudo apt-get install memcached
sudo apt-get install libmemcached-dev libmemcached11
```

## 設定 php 7 Memcached 套件

php 7 讀取 Memcached 的套件還是在開發版，沒辦法直接使用 apt-get 去完成安裝，所以我們要自行下載開發版套件去進行編譯安裝

### 安裝 php 7 套件編譯軟體

php7.0-dev 是為了執行 php7 的 phpize 而安裝的

```shell
sudo apt-get install pkg-config
sudo apt-get install php7.0-dev
```


### 下載 php Memcached 套件

```shell
git clone https://github.com/php-memcached-dev/php-memcached
```

### 切換開發版套件路徑

```shell
cd php-memcached
git checkout -b php7 origin/php7
```

### 編譯 php7 Memcached 套件

```shell
phpize
./configure
make
make test
sudo make install
```

## 設定載入 php7 memcached 套件

```shell
sudo vim /etc/php/7.0/cli/conf.d/20-memcached.ini
sudo vim /etc/php/7.0/fpm/conf.d/20-memcached.ini
```

在 `20-memcached.ini` 檔案內設定

```ini
extension=memcached.so
```

設定檔分別要在 `cli` 與 `fpm` 兩個地方

* `cli` 指的是 command line 的 php 執行擋
* `fpm` 指的是 nginx 解析 php 的解譯器

記得兩邊都要設定

## 重新啟動 php7.0-fpm

重新啟動之後，在網頁部分可以用 `phpinfo();` 確認看看有沒有看到 Memcached 套件

```shell
sudo service php7.0-fpm restart
```

## 查詢安裝結果

***查詢 phpinfo Memcached 安裝結果***

![查詢 phpinfo Memcached 安裝結果](./images/install-memcached-phpinfo.png)

***查詢 php command line Memcached 安裝結果***

```shell
$ php -i | grep memcached


/etc/php/7.0/cli/conf.d/20-memcached.ini,
memcached
memcached support => enabled
libmemcached version => 1.0.18
memcached.compression_factor => 1.3 => 1.3
memcached.compression_threshold => 2000 => 2000
memcached.compression_type => fastlz => fastlz
memcached.default_binary_protocol => 0 => 0
memcached.default_connect_timeout => 0 => 0
memcached.default_consistent_hash => 0 => 0
memcached.serializer => php => php
memcached.sess_binary_protocol => 1 => 1
memcached.sess_connect_timeout => 0 => 0
memcached.sess_consistent_hash => 1 => 1
memcached.sess_lock_expire => 0 => 0
memcached.sess_lock_max_wait => not set => not set
memcached.sess_lock_retries => 5 => 5
memcached.sess_lock_wait => not set => not set
memcached.sess_lock_wait_max => 2000 => 2000
memcached.sess_lock_wait_min => 1000 => 1000
memcached.sess_locking => 1 => 1
memcached.sess_number_of_replicas => 0 => 0
memcached.sess_persistent => 0 => 0
memcached.sess_prefix => memc.sess. => memc.sess.
memcached.sess_randomize_replica_read => 0 => 0
memcached.sess_remove_failed_servers => 0 => 0
memcached.sess_sasl_password => no value => no value
memcached.sess_sasl_username => no value => no value
memcached.sess_server_failure_limit => 0 => 0
memcached.store_retry_count => 2 => 2
Registered save handlers => files user memcached
```


這樣我們就完成 Memcached 及在 php 7.0 Memcached 套件的安裝了

## 參考資料
* [php-memcached-dev/php-memcached: memcached extension based on libmemcached library](https://github.com/php-memcached-dev/php-memcached)
* [kasparsd/php-7-debian: Install PHP 7 on Debian/Ubuntu](https://github.com/kasparsd/php-7-debian)
* [ServerPilot | How to Install the PHP Memcache Extension](https://serverpilot.io/community/articles/how-to-install-the-php-memcache-extension.html)
* [Installing PHP-7 with Memcached - Servers for Hackers](https://serversforhackers.com/video/installing-php-7-with-memcached)


!INCLUDE "../kejyun/book/laravel-5-for-beginner.md"
