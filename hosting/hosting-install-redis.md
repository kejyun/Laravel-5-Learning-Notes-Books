# 安裝 Redis

## 設定 Redis 套件資源庫

```shell
sudo add-apt-repository ppa:chris-lea/redis-server
sudo apt-get update
```

## 安裝

```shell
sudo apt-get install redis-server
```

## 設定 Redis

Redis 設定檔放在`/etc/redis/redis.conf`

```shell
sudo vim /etc/redis/redis.conf
```

設定設定檔

```conf
requirepass 你的 Redis 連線密碼
#bind 127.0.0.1 不要限定只有本機才能連線
maxmemory-policy noeviction
appendonly yes
appendfilename redis-staging-ao.aof
```

## 重新啟動 Redis

```shell
sudo service redis-server restart
```

這樣我們就完成 Redis 的安裝了！！


## 參考資料
* [How To Configure a Redis Cluster on Ubuntu 14.04 | DigitalOcean](https://www.digitalocean.com/community/tutorials/how-to-configure-a-redis-cluster-on-ubuntu-14-04)



!INCLUDE "../kejyun/book/laravel-5-for-beginner.md"
