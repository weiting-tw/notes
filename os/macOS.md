# Mac OS

## rm 刪除大量檔案時，出現 Argument list too long

xargs 的命令一次會被調用 2000~ 4000次左右，因此，如果列出的log有一萬筆的話，可能就會被分成 3到 5次左右來執行，因而避開了 Argument list too long 的錯誤。

```sh
find . -name "*.log" | xargs rm
```

## [Homebrew](http://brew.sh/) 套件管理工具

```sh
# 安裝 Homebrew
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
# 顯示使用說明
man brew
# 更新套件索引
brew update
# 搜尋可安裝的套件
brew search git
# 安裝套件
brew install git
# 升級已安裝的套件
brew upgrade
# 移除套件
brew uninstall git
# 清理暫存檔
brew cleanup
```

### [Homebrew Cask](https://caskroom.github.io) 應用程式管理工具

```sh
# 安裝 Homebrew Cask
brew tap caskroom/cask
# 顯示使用說明
man brew-cask
# 搜尋可安裝的應用程式
brew cask search java
# 安裝應用程式
brew cask install java
# 升級應用程式
brew cask reinstall java
# 移除應用程式
brew cask uninstall java
# 清理暫存檔
brew cask cleanup
# 檢查所有已安裝的應用程式是否有更新的版本
brew cask outdated
# 升級所有已安裝的應用程式至最新的版本
brew cask upgrade
```
