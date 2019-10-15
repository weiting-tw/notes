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

## 修復 Dock 上的 icon 遺失

* 先從 Applications 內，將該app拖移到 Dock

* 若不行的話，將電腦進入[安全模式](https://support.apple.com/HT201262)，會清掉 low-level caches

1. 關機
    ![image](https://cdn1.tekrevue.com/wp-content/uploads/2018/03/Shut-Down.jpg)

2. 開機時按住shift，直到開機完成
    ![image](https://cdn1.tekrevue.com/wp-content/uploads/2018/03/Shift-Key.jpg)

3. 正常重新開機

## 完整反安裝 Xcode

```sh
/Apllications/Xcode.app
/Library/Preferences/com.apple.dt.Xcode.plist
~/Library/Preferences/com.apple.dt.Xcode.plist
~/Library/Caches/com.apple.dt.Xcode
~/Library/Application Support/Xcode
```

## 安裝 Xcode Command Line 開發者套件而不安裝 XCode

```sh
sudo xcode-select --install
```

## 刪除 .DS_Store

```sh
find . -type f -name ".DS_Store" -o -name "._*" -delete
```

## 不要在網路磁碟上自動產生 `.DS_Store`

> .DS_Store 會用來儲存目錄圖示狀態

```sh
defaults write com.apple.desktopservices DSDontWriteNetworkStores true
```

## [自動加載ssh key](../applications/git.md#adding-your-ssh-key-to-the-ssh-agent)

## [開啟 cron job 的記錄檔](https://apple.stackexchange.com/questions/38861/where-is-the-cron-log-file-in-macosx-lion)

1. 編輯 /etc/syslog.conf

    在這個檔案裡加上下面一行：

    ```sh
    sudo cron.* /var/log/cron.log
    ```

2. 重新啟動 syslogd

    執行下面的兩行指令，重新指動 syslod：

    ```sh
    sudo launchctl unload /System/Library/LaunchDaemons/com.apple.syslogd.plist
    sudo launchctl load /System/Library/LaunchDaemons/com.apple.syslogd.plist
    ```

3. 檢查 /var/log/cron.log

    等 cron job 的時間到了之後，應該就能看到 `/var/log/cron.log` 了

## QuickLook

Folder path:
> /Library/QuickLook/
> /Users/$(whoami)/Library/QuickLook/

* [QuicklookStephen](https://github.com/whomwah/qlstephen)
  QLStephen is a QuickLook plugin that lets you view plain text files without a file extension. Files like:`README`, `INSTALL`, `Capfile`, `CHANGELOG`, etc...
* [QLMarkdown](https://github.com/toland/qlmarkdown)
  QuickLook generator for Markdown files.

## Beyond compare

```bash
#!/bin/bash
rm "/Users/$(whoami)/Library/Application Support/Beyond Compare/registry.dat"
"`dirname "$0"`"/BCompare.bak $@
```

## zshrc

```bash
## alias
alias cls=clear
alias ping='prettyping --nolegend'
alias help='tldr'
alias top="sudo htop"
### Beyond Compare
alias bc=bcomp

### update brew
up() {
  brew update
  brew upgrade
  brew cleanup
  brew cask upgrade
  rm -rf "$(brew --cache)"
}
## functions

### kill process with particular port
#### port: $1
killport() {
  PROCESS_ID=$(lsof -ti:$1);
  if [ "$PROCESS_ID" != "" ]; then
    echo "Try to kill process:$PROCESS_ID with using port: $1.";
    kill -9 $PROCESS_ID;
  fi
}

## change git author name
gitNameChange2Weiting() {
  git filter-branch --env-filter '
  OLD_EMAIL="wilber_chen@gss.com.tw"
  CORRECT_NAME="weiting"
  CORRECT_EMAIL="a26007565@gmail.com"
  
  if [ "$GIT_COMMITTER_EMAIL" = "$OLD_EMAIL" ]
  then
  export GIT_COMMITTER_NAME="$CORRECT_NAME"
  export GIT_COMMITTER_EMAIL="$CORRECT_EMAIL"
  fi
  if [ "$GIT_AUTHOR_EMAIL" = "$OLD_EMAIL" ]
  then
  export GIT_AUTHOR_NAME="$CORRECT_NAME"
  export GIT_AUTHOR_EMAIL="$CORRECT_EMAIL"
  fi
  ' --tag-name-filter cat -- --branches --tags
}
```
