# Supervisor 啟動 queue

## 安裝 Supervisor

```shell
sudo apt-get install supervisor
```

## 設定檔案路徑

```shell
/etc/supervisor/conf.d
```

## 設定

```shell
vim /etc/supervisor/conf.d/laravel-worker.conf
```

## 設定檔案

```
[program:laravel-worker]
process_name=%(program_name)s_%(process_num)02d
command=php /home/forge/app.com/artisan artisan queue:work --queue=instant,high,medium,default,low --delay=1 --memory=512 --sleep=15 --tries=1 --env=dev --daemon
autostart=true
autorestart=true
user=www-data
numprocs=8
redirect_stderr=true
stdout_logfile=/home/forge/app.com/worker.log
```


## 啟動 Supervisor

```shell
sudo supervisorctl reread
sudo supervisorctl update
sudo supervisorctl start laravel-worker:*
```

## 停止 Supervisor

```shell
sudo supervisorctl stop laravel-worker:*
```

## 重新啟動 Supervisor

若當 Laravel 設定檔有做異動時，需要重啟 Supervisor 以讀取新的設定

```shell
# 重新啟動指定的 worker
supervisorctl restart laravel-worker:
# 重新啟動所有的 worker
supervisorctl restart all
```

## 參考資料
* [Queues - Laravel - The PHP Framework For Web Artisans](https://laravel.com/docs/5.4/queues#supervisor-configuration)
* [supervisord - how to restart only certain processes using supervisorctl? - Stack Overflow](https://stackoverflow.com/questions/9301494/how-to-restart-only-certain-processes-using-supervisorctl)


!INCLUDE "../../kejyun/book/laravel-5-for-beginner.md"
