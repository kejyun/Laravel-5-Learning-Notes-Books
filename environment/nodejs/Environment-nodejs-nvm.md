# NVM (Node Version Manager)

## 安裝 NVM

```shell
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.30.1/install.sh | bash
```

## 設定 NVM 環境變數

在 `~/.bash_profile`,`~/.bashrc`, `~/.profile`, 或 `~/.zshrc` 設定環境變數

```shell
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"  # 讀取 NVM
```

## 安裝 Node.js

### 確認可以安裝版本清單

使用 `nvm ls-remote` 可以看到目前 nvm 可以安裝的所有 node.js 版本

```shell
$ nvm ls-remote
```

### 安裝指定版本 5.4.0 的 node.js

```shell
nvm install 5.4.0
```

### 指定 nvm 使用的 Node.js 版本

```shell
nvm use 5.4.0
```

### 預設使用 5.4.0 版本，否則每次重新連線登入，還需要重新 nvm use 一次

```shell
nvm alias default 5.4.0
```

## 參考資料
* [creationix/nvm](https://github.com/creationix/nvm)