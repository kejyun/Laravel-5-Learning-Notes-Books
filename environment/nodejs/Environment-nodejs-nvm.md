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

## 參考資料
* [creationix/nvm](https://github.com/creationix/nvm)