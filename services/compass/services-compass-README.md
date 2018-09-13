# Compass


## 安裝 RVM

**1. 安裝 GPG keys**

```shell
$ gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
```

**2. 安裝 RVM**

```shell
\curl -sSL https://get.rvm.io | bash
```

## 使用 RVM 安裝 Ruby

**列出所有可以安裝的版本**

```shell
rvm list known
```

**安裝指定版本**

```shell
rvm install 2.3.1
```

> 必須使用管理者權限安裝過以下套件：

> autoconf, automake, bison, libffi-dev, libgdbm-dev, libncurses5-dev, libsqlite3-dev, libtool, libyaml-dev, pkg-config, sqlite3, zlib1g-dev, libgmp-dev, libreadline6-dev

## 安裝 Compass

```shell
gem update --system
```


```shell
gem install compass
```

## 監控 Compass

```
gulp watch
```

## 參考資料
* [RVM: Ruby Version Manager - Installing RVM](https://rvm.io/rvm/install)
* [Install the Compass Stylesheet Authoring Framework | Compass Documentation](http://compass-style.org/install/)
* [creationix/nvm: Node Version Manager - Simple bash script to manage multiple active node.js versions](https://github.com/creationix/nvm)
