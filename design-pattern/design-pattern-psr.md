# PSR (php standard recommendations)

為了讓大家開發的套件，能夠更輕鬆地整合到自己的專案當中，在 PHP 社群中大家一起定義了一些標準的程式碼撰寫規則

但是 Laravel 5.0.x 版本之前，Laravel 都沒有真正的遵照 PSR 的規範去撰寫程式碼，直到 Laravel 5.1 LTS 版本時，Laravel 終於將所有的程式碼遵照 PSR-2 及 PSR-4 的程式碼撰寫規則了，詳細的規則說明大家可以自己參考相關的說明文件。

而為了讓自己專案的開發也能夠遵照 PSR 規則，除了自己一個檔案一個檔案自己修改外，也可以用 [PHP Coding Standards Fixer](http://cs.sensiolabs.org/) 套件去幫我們`自動地`將程式修改成遵照 PSR 規則的程式！

## PHP Coding Standards Fixer 安裝使用教學

### 使用 compser 下載套件

使用 composer 將 php-cs-fixer 安裝到全域（global）目錄下


```shell
$ composer global require fabpot/php-cs-fixer
```

### 設定 composer bin 目錄到環境變數中

我們必須要將我們家目錄下的全域 `~/.composer/vendor/bin` 目錄，設到環境變數中，這樣我們在命令列就可以直接執行 `~/.composer/vendor/bin` 下面的可執行檔案了

```shell
$ export PATH="$PATH:$HOME/.composer/vendor/bin"
```

> 我們可以直接在命令列下這樣的指令就可以了，但每次開啟新的 Terminal 視窗時，都要再重新的設定一次這樣的環境變數，所以我們也可以把這個設定寫在 `~/.bash_profile` 檔案中，這樣每次執行 Terminal 時，就會自動將 `~/.composer/vendor/bin` 設到環境變數中了！


### 使用 php-cs-fixer 修正 PHP 檔案

設定完成後，我們就可以使用 `php-cs-fixer fix /path/to/project --level=psr2` 這樣的指令去修正我們專案目錄下的檔案了

一些 php-cs-fixer 相關的指令會像這樣:

```shell
$ php-cs-fixer fix /path/to/project --level=psr0
$ php-cs-fixer fix /path/to/project --level=psr1
$ php-cs-fixer fix /path/to/project --level=psr2
$ php-cs-fixer fix /path/to/project --level=symfony
```

### 設定 Sublime 使用 php-cs-fixer 修正程式碼

在 Sublime 上方工具列 `Tools\Bulid System\New Build System` 我們可以新增一個新的建立指令，指令中我們輸入像這樣的指令：

```json
{
	"shell_cmd": "php-cs-fixer fix $file --level=psr2"
}

```

將新的指令檔案名稱取為 `php-cs-fixer.sublime-bulid`，這樣我們回到 Sublime 去開啟任一 PHP 檔案，只要按下 `Command（⌘）`+`B`，Sublime 就會自動幫我們執行 php-cs-fixer 的 shell script 指令，去修正我們的 PHP 檔案了！！

### php-cs-fixer 使用小技巧

我們可以將修改 `~/.bash_profile` 檔案，將使用 php-cs-fixer 修正 Terminal 目前目錄的 PHP 指令加入，這樣我們只要用 Terminal 瀏覽到我們想要做 php-cs-fixer 的目錄下，我們每次只需要下 `phpCSFixerThisFolder` 指令就可以了，這樣就不用記住也不用打那麼落落長的 php-cs-fixer 指令了！

```shell
alias phpCSFixerThisFolder="php-cs-fixer fix ./ --level=psr2"
```


## PSR 中文文件
* [PSR-1 - Basic Coding Standard](https://github.com/laravel-taiwan/fig-standards/blob/master/accepted/zh-TW/PSR-1-basic-coding-standard.md)
* [PSR-2 - Coding Style Guide](https://github.com/laravel-taiwan/fig-standards/blob/master/accepted/zh-TW/PSR-2-coding-style-guide.md)
* [PSR-3 - 日誌介面](https://github.com/laravel-taiwan/fig-standards/blob/master/accepted/zh-TW/PSR-3-logger-interface.md)
* [PSR-4 - Autoloader](https://github.com/laravel-taiwan/fig-standards/blob/master/accepted/zh-TW/PSR-4-autoloader.md)

## 參考資料
* [PHP-FIG — PHP Framework Interop Group](http://www.php-fig.org/)
* [PHP: The Right Way - 繁體中文](http://laravel-taiwan.github.io/php-the-right-way/)
* [PHP Coding Standards Fixer](http://cs.sensiolabs.org)
* [PHP-CS-Fixer](https://github.com/FriendsOfPHP/PHP-CS-Fixer)
* [Adopting PSR-2 - laracasts](https://laracasts.com/series/whats-new-in-laravel-5-1/episodes/1)
