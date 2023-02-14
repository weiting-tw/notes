# Mac OS

## rm 刪除大量檔案時，出現 Argument list too long

xargs 的命令一次會被調用 2000~ 4000 次左右，因此，如果列出的 log 有一萬筆的話，可能就會被分成 3 到 5 次左右來執行，因而避開了 Argument list too long 的錯誤。

```bash
find . -name "*.log" | xargs rm
```

## [Homebrew](http://brew.sh/) 套件管理工具

```bash
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

```bash
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

### [Fix casks with `depends_on`](https://github.com/Homebrew/homebrew-cask/issues/**58046**)

If you get an error of the type Error: Cask 'hex-fiend-beta' definition is invalid: invalid 'depends\_on macos' value: ":lion", where hex-fiend-beta can be any cask name, and :lion any macOS release name, run the following command:

```bash
/usr/bin/find "$(brew --prefix)/Caskroom/"*'/.metadata' -type f -name '*.rb' -print0 | /usr/bin/xargs -0 /usr/bin/perl -i -pe 's/depends_on macos: \[.*?\]//gsm;s/depends_on macos: .*//g'
```

### Backup Homebrew Setup

```bash
brew bundle dump --describe --force --file=${path}
```

wil create `Brewfile`

``` Brewfile
tap "a26007565/iota"
tap "adoptopenjdk/openjdk"
tap "azure/functions"
tap "buo/cask-upgrade"
tap "heroku/brew"
tap "homebrew/bundle"
tap "homebrew/cask"
tap "homebrew/cask-fonts"
tap "homebrew/cask-versions"
tap "homebrew/core"
tap "homebrew/services"
tap "mongodb/brew"
tap "ringohub/redis-cli"
# Plugin manager for zsh, inspired by oh-my-zsh and vundle
brew "antigen"
# Light UNIX download accelerator
brew "axel"
# Microsoft Azure CLI 2.0
brew "azure-cli"
# Clone of cat(1) with syntax highlighting and Git integration
brew "bat"
# Tool to obtain certs from Let's Encrypt and autoenable HTTPS
brew "certbot", link: false
# Cross-platform make
brew "cmake"
# Top-like interface for container metrics
brew "ctop"
# Good-lookin' diffs with diff-highlight and more
brew "diff-so-fancy"
# Secure communications between a client and a DNS resolver
brew "dnscrypt-proxy"
# Play, record, convert, and stream audio and video
brew "ffmpeg"
# Command-line fuzzy finder written in Go
brew "fzf"
# GNU compiler collection
brew "gcc"
# Distributed revision control system
brew "git"
# Git extension for versioning large files
brew "git-lfs"
# Official GitLab CI runner
brew "gitlab-runner"
# GNU Pretty Good Privacy (PGP) package
brew "gnupg", link: false
# Open source programming language to build simple/reliable/efficient software
brew "go"
# Command-Line Interface for Hasura GraphQL Engine
brew "hasura-cli"
# Convert source code to formatted text with syntax highlighting
brew "highlight"
# Improved top (interactive process viewer)
brew "htop"
# Tools and libraries to manipulate images in many formats
brew "imagemagick"
# Development kit for the Java programming language
brew "openjdk"
# Dex to Java decompiler
brew "jadx"
# Load testing and performance measurement application
brew "jmeter"
# Lightweight and flexible command-line JSON processor
brew "jq"
# Version of the SSL/TLS protocol forked from OpenSSL
brew "libressl"
# Mac App Store command-line interface
brew "mas"
# Utility for testing the memory subsystem
brew "memtester"
# Replacement for ls, cp and other commands for object storage
brew "minio-mc"
# Cross platform, open source .NET development framework
brew "mono"
# Utility for managing network connections
brew "netcat"
# Port scanning utility for large networks
brew "nmap"
# Simplified-traditional Chinese conversion tool
brew "opencc"
# 7-Zip (high compression file archiver) implementation
brew "p7zip"
# Provides an easy way to use U2F-compliant authenticators with PAM
brew "pam-u2f"
# Swiss-army knife of markup format conversion
brew "pandoc"
# Pinentry for GPG on Mac
brew "pinentry-mac"
# Wrapper to colorize and simplify ping's output
brew "prettyping"
# Python version management
brew "pyenv"
# SMART hard drive monitoring
brew "smartmontools"
# Version control system designed to be a better CVS
brew "subversion"
# TCP connect to the given IP/port combo
brew "tcping"
# Microsoft Team Explorer Everywhere command-line Client
brew "tee-clc"
# User interface to the TELNET protocol
brew "telnet"
# Text interface for Git repositories
brew "tig"
# Graphical side by side diff utility
brew "tkdiff"
# Simplified and community-driven man pages
brew "tldr"
# Terminal multiplexer
brew "tmux"
# Display directories as trees (with optional color/HTML output)
brew "tree"
# Watch files and take action when they change
brew "watchman"
# Internet file retriever
brew "wget"
# Fish-like fast/unobtrusive autosuggestions for zsh
brew "zsh-autosuggestions"
# Additional completion definitions for zsh
brew "zsh-completions"
# Zsh port of Fish shell's history search
brew "zsh-history-substring-search"
# Fish shell like syntax highlighting for zsh
brew "zsh-syntax-highlighting"
# Azure Functions Core Tools 3.0
brew "azure/functions/azure-functions-core-tools@3"
# Everything you need to get started with Heroku
brew "heroku/brew/heroku"
# An interactive JavaScript command-line interface to MongoDB
brew "mongodb/brew/mongodb-community-shell"
# Install the redis-cli only.
brew "ringohub/redis-cli/redis-cli"
# Android SDK component
cask "android-platform-tools"
# Allows connection to a computer remotely
cask "anydesk"
# Data management tool that enables working with SQL Server
cask "azure-data-studio"
# Tool to flash OS images to SD cards & USB drives
cask "balenaetcher"
# Menu bar icon organizer
cask "bartender"
# Cross platform SQL editor and database management app
cask "beekeeper-studio"
# Compare files and folders
cask "beyond-compare"
# Desktop password and login vault
cask "bitwarden"
# Web debugging Proxy application
cask "charles"
# Collaboration tool with multi-user screen sharing
cask "coscreen"
# Browser for SQLite databases
cask "db-browser-for-sqlite"
# App to build and share containerized applications and microservices
cask "docker"
# Developer platform
cask "dotnet-sdk"
# QuickLook generator and Spotlight importer
cask "epubquicklook"
cask "font-noto-sans-cjk-tc"
cask "font-source-code-pro"
# GIT client
cask "fork"
# Fabric agent with endpoint protection and cloud sandbox
cask "forticlient"
# Git client focusing on productivity
cask "gitkraken"
cask "iota"
cask "java"
cask "jxplorer"
# Password manager app
cask "keepassxc"
# Visual user interface for Docker Container management
cask "kitematic"
# Explorer for Azure Storage
cask "microsoft-azure-storage-explorer"
# Reverse proxy, secure introspectable tunnels to localhost
cask "ngrok"
# Command-line shell and scripting language
cask "powershell"
# QuickLook generator for Markdown files
cask "qlmarkdown"
cask "qlstephen"
# Centralizes all remote connections on a single platform
cask "remote-desktop-manager-free"
# Remote access and connectivity software focused on security
cask "teamviewer"
# Git client focusing on power and productivity
cask "tower"
# Open-source code editor
cask "visual-studio-code"
cask "wkhtmltopdf"
mas "Disk Speed Test", id: 425264550
mas "Fantastical", id: 975937182
mas "iSuper DVD Ripper", id: 625150797
mas "Keynote", id: 409183694
mas "KKBOX", id: 1105332179
mas "LINE", id: 539883307
mas "Microsoft Remote Desktop", id: 1295203466
mas "Numbers", id: 409203825
mas "Pages", id: 409201541
mas "PNG Compressor", id: 434886325
mas "Slack", id: 803453959
mas "Telegram", id: 747648890
mas "Yoink", id: 457622435
```

### Restore

In Brewfile path

```bash
brew bundle
```

## 修復 Dock 上的 icon 遺失

* 先從 Applications 內，將該 app 拖移到 Dock
* 若不行的話，將電腦進入[安全模式](https://support.apple.com/HT201262)，會清掉 low-level caches
* 關機 ![image](https://cdn1.tekrevue.com/wp-content/uploads/2018/03/Shut-Down.jpg)
* 開機時按住 shift，直到開機完成 ![image](https://cdn1.tekrevue.com/wp-content/uploads/2018/03/Shift-Key.jpg)
* 正常重新開機

## 完整反安裝 Xcode

```bash
/Apllications/Xcode.app
/Library/Preferences/com.apple.dt.Xcode.plist
~/Library/Preferences/com.apple.dt.Xcode.plist
~/Library/Caches/com.apple.dt.Xcode
~/Library/Application Support/Xcode
```

## 安裝 Xcode Command Line 開發者套件而不安裝 XCode

```bash
sudo xcode-select --install
```

## 刪除 .DS\_Store

```bash
find . -type f -name ".DS_Store" -o -name "._*" -delete
```

## 不要在網路磁碟上自動產生 `.DS_Store`

> .DS\_Store 會用來儲存目錄圖示狀態

```bash
defaults write com.apple.desktopservices DSDontWriteNetworkStores true
```

## [自動加載 ssh key](../applications/git.md#adding-your-ssh-key-to-the-ssh-agent)

## [開啟 cron job 的記錄檔](https://apple.stackexchange.com/questions/38861/where-is-the-cron-log-file-in-macosx-lion)

1. 編輯 /etc/syslog.conf

   在這個檔案裡加上下面一行：

   ```bash
    sudo cron.* /var/log/cron.log
   ```

2. 重新啟動 syslogd

   執行下面的兩行指令，重新指動 syslod：

   ```bash
    sudo launchctl unload /System/Library/LaunchDaemons/com.apple.syslogd.plist
    sudo launchctl load /System/Library/LaunchDaemons/com.apple.syslogd.plist
   ```

3. 檢查 /var/log/cron.log

   等 cron job 的時間到了之後，應該就能看到 `/var/log/cron.log` 了

## QuickLook

Folder path:

> /Library/QuickLook/ /Users/$\(whoami\)/Library/QuickLook/

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
if [ -f ~/.bash_profile ]; then
  source ~/.bash_profile
fi

## alias
alias cls=clear
alias ping='prettyping --nolegend'
alias help='tldr'
alias top="sudo htop"
### Beyond Compare
alias bc=bcomp
## HOMEBREW
[[ $PATH != */usr/local/sbin* ]] && export PATH="/usr/local/bin:/usr/local/sbin:$PATH"
[[ -e "/usr/local/opt/curl/bin" ]] && export PATH="/usr/local/opt/curl/bin:$PATH"
function up() {
  brew update && brew doctor
  brew upgrade
  brew upgrade --cask
  brew outdated --cask --greedy --verbose | grep -v latest | awk '{print $1}' | xargs -I{} brew upgrade --cask --greedy {}
  brew cleanup
  hash npm 2>/dev/null && npm up -g
  hash mas 2>/dev/null && mas upgrade
}

## brew
alias bs='brew search'
alias bi='brew install'

## functions

## GSS Wifi Login
655wifiLogin() {
  autowifilogin -u $username -p $password
  pveAutoConnectVM -h pve.gss.com.tw -u $username -p $password
}

### kill process with particular port
#### port: $1
killport() {
  PROCESS_ID=$(lsof -ti:$1);
  if [ "$PROCESS_ID" != "" ]; then
    echo "Try to kill process:$PROCESS_ID with using port: $1.";
    kill -9 $PROCESS_ID;
  fi
}

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

updateSSL() {
  sudo certbot renew --force-renewal --quiet
  sudo cp -r /etc/letsencrypt/live/weiting.me/ /Users/weiting/CloudStation/SSL/
}

## env
export HOMEBREW_GITHUB_API_TOKEN=$HOMEBREW_GITHUB_API_TOKEN
export PATH="/usr/local/opt/node@8/bin:$PATH"

test -e "${HOME}/.iterm2_shell_integration.zsh" && source "${HOME}/.iterm2_shell_integration.zsh"

export GPG_TTY=$(tty)

[ -f ~/.fzf.zsh ] && source ~/.fzf.zsh
fpath=(/usr/local/share/zsh-completions $fpath)

source /usr/local/share/zsh-history-substring-search/zsh-history-substring-search.zsh
source /usr/local/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
source /usr/local/share/zsh-autosuggestions/zsh-autosuggestions.zsh

# source ~/.antigenrc


alias download='axel -n 2'

alias preview="fzf --preview 'bat --style=numbers --color=\"always\" {}'"
export FZF_CTRL_T_OPTS="--preview '(highlight -O ansi -l {} 2> /dev/null || cat {} || tree -C {}) 2> /dev/null'"

# azure-cli
autoload -U +X bashcompinit && bashcompinit
source /usr/local/etc/bash_completion.d/az

export DOTNET_CLI_TELEMETRY_OPTOUT=1
complete -o nospace -C /usr/local/bin/mc mc
```

## 移除檔案

```bash
find . -iname "obj" | xargs rm -rf
find . -iname "bin" | xargs rm -rf
```

## 設定個別的使用者帳號或群組權限

```bash
# 給予管理員權限
sudo dscl . -append /Groups/admin GroupMembership $USER

# 移除管理員權限
sudo dscl . -delete /Groups/admin GroupMembership $USER
```
