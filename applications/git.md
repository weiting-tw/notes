# Git

## Push when using two-factor authenticationg

啟用二階段驗證時，會無法push到server上

```bash
> git push
Username for 'https://github.com': a26007565
Password for 'https://a26007565@github.com':
remote: Invalid username or password.
fatal: Authentication failed for 'https://github.com/a26007565/notes.git/'
```

解決方法:

1. git config –global credential.helper store
2. Settings =&gt; Personal access tokens
3. Generate new token =&gt; token的範圍選repo就夠了 =&gt; click Generate Token
4. 再次輸入：git push，輸入帳號，再密碼的地方輸入token就行了

**備註** 訊息會存在

> ~/.git-credentials

## Generating a new SSH key

Open Git Bash.

Paste the text below, substituting in your GitHub email address.

```bash
ssh-keygen -t rsa -b 4096 -C <your_email@example.com>
```

This creates a new ssh key, using the provided email as a label.

```bash
Generating public/private rsa key pair.
```

When you're prompted to "Enter a file in which to save the key," press Enter. This accepts the default file location.

```bash
Enter a file in which to save the key (/c/Users/you/.ssh/id_rsa):[Press enter]
```

At the prompt, type a secure passphrase. For more information, see "Working with SSH key passphrases".

```bash
Enter passphrase (empty for no passphrase): [Type a passphrase]
Enter same passphrase again: [Type passphrase again]
```

## Adding your SSH key to the ssh-agent

Before adding a new SSH key to the ssh-agent to manage your keys, you should have checked for existing SSH keys and [generated a new SSH key](git.md#generating-a-new-ssh-key). When adding your SSH key to the agent, use the default macOS ssh-add command, and not an application installed by `macports`, [`homebrew`](../os/macos.md#Homebrew-套件管理工具), or some other external source.

Start the ssh-agent in the background.

```bash
eval "$(ssh-agent -s)"
Agent pid 59566
```

If you're using macOS Sierra 10.12.2 or later, you will need to modify your `~/.ssh/config` file to automatically load keys into the ssh-agent and store passphrases in your keychain.

```bash
Host *
 AddKeysToAgent yes
 UseKeychain yes
 IdentityFile ~/.ssh/id_rsa
```

Add your SSH private key to the ssh-agent and store your passphrase in the keychain. If you created your key with a different name, or if you are adding an existing key that has a different name, replace id\_rsa in the command with the name of your private key file.

> ssh-add -K ~/.ssh/id\_rsa

Note: The -K option is Apple's standard version of ssh-add, which stores the passphrase in your keychain for you when you add an ssh key to the ssh-agent.

If you don't have Apple's standard version installed, you may receive an error. For more information on resolving this error, see "[Error: ssh-add: illegal option -- K](https://help.github.com/articles/error-ssh-add-illegal-option-k)."\*

## 救回 Commit

誤 reset 時，使用 `reflog` 觀看 git 的歷程

```bash
> git reflog
657fce7 (HEAD -> master) HEAD@{0}: reset: moving to HEAD~2
e12d8ef (origin/master, origin/HEAD, cat) HEAD@{1}: checkout: moving from cat to master
e12d8ef (origin/master, origin/HEAD, cat) HEAD@{2}: checkout: moving from master to cat
```

指定 Commit 即可救回

```bash
> git reset e12d8ef --hard
```

## 對底下的所有資料夾執行 git 指令

使用 npm 的套件 [gitfox](https:/github.com/aibeb/gitfox)

使用方法

```bash
g <command> [<parent-dir>]
g 'config core.filemode false'
g 'fetch --all'
```

## 複製整個資料夾到另外一個磁碟出現檔案已被修改

可能是因為檔案權限被異動 like `755=rwxr-xr-x` to `644=rw-r--r--`

Suggests setting core.filemode to false.

```bash
git config core.filemode false
git config core.filemode false --global
```

## 移除本地已合併的分支

First, list all branches that were merged in remote.

```bash
git branch --merged
```

You might see few branches you don't want to remove. we can add few arguments to skip important branches that we don't want to delete like master or a develop. The following command will skip master branch and anything that has dev in it.

```bash
git branch --merged| egrep -v "(^\*|master|dev)"
```

If you want to skip, you can add it to the egrep command like the following. The branch skip\_branch\_name will not be deleted.

```bash
git branch --merged| egrep -v "(^\*|master|dev|skip_branch_name)"
```

To delete all local branches that are already merged into the currently checked out branch:

```bash
git branch --merged | egrep -v "(^\*|master|dev)" | xargs git branch -d
```

You can see that master and dev are excluded in case they are an ancestor.

You can delete a merged local branch with:

```bash
git branch -d branchname
```

If it's not merged, use:

```bash
git branch -D branchname
```

To delete it from the remote use:

```bash
git push --delete origin branchname

git push origin :branchname    # for really old git
```

Once you delete the branch from the remote, you can prune to get rid of remote tracking branches with:

```bash
git remote prune origin
```

or prune individual remote tracking branches, as the other answer suggests, with:

```bash
git branch -dr branchname
```

## 設定 git ignore 命令並自動下載所需的 .gitignore 範本

[gitignore.io](https://www.gitignore.io/) 網站收集了各種不同程式語言、開發框架、開發工具所需的 .gitignore 檔案範本

### Web API

該網站其實主要也只有兩個端點可用：

* 列出所有範本清單

  > [https://www.gitignore.io/api/list](https://www.gitignore.io/api/list)

* 取得特定範本內容

  > [https://www.gitignore.io/api/{types}](https://www.gitignore.io/api/{types})

* 例如你要取得 VisualStudio 範本的內容，就可以直接呼叫

  > [https://www.gitignore.io/api/visualstudio](https://www.gitignore.io/api/visualstudio)

* 如果你想取得 dart 與 flutter 的範本，就可以直接呼叫

  > [https://www.gitignore.io/api/dart,flutter](https://www.gitignore.io/api/dart,flutter)

shell

```text
git config --global alias.ignore "!gi() { curl -sL https://www.gitignore.io/api/$@ ;}; gi"
```

> 請注意：官網提供的命令是以「單引號」包含字串，但這個設定並不適用於 Windows 的命令提示字元，建議改用「雙引號」，就可以同時適用於 Windows/Linux/macOS 所有的 Shell 執行環境。不過還是要設定一下驚嘆號的跳脫字元才能執行！

bash

```bash
git config --global alias.ignore '!'"gi() { curl -sL https://www.gitignore.io/api/\$@ ;}; gi"
```

* 取得範本清單

  > git ignore list

* 下載 VisualStudio 範本

  > git ignore visualstudio &gt; .gitignore

* 同時下載 VisualStudio 與 ASPNETCore 範本

  > git ignore visualstudio,aspnetcore &gt; .gitignore

## 快速調整 Git 設定的小工具

[@willh/git-setup](https://www.npmjs.com/package/@willh/git-setup) 會全自動設定 Git 版控環境，並且跨平台支援 Windows, Linux, macOS 等作業系統的命令列環境，尤其針對中文環境經常會出現亂碼的問題都會完整的解決。

> npx @willh/git-setup

## 設定 [core.autocrlf](https://git-scm.com/book/en/v2/Customizing-Git-Git-Configuration) 解決換行問題

* 各系統設定
  | Unix (Mac/Linux) | Windows |
  |---|---|
  | input | true |

* 各設定值的作用
  Git can handle this by auto-converting CRLF line endings into LF when you add a file to the index, and vice versa when it checks out code onto your filesystem. You can turn on this functionality with the core.autocrlf setting. If you’re on a Windows machine, set it to true — this converts LF endings into CRLF when you check out code:

  > git config --global core.autocrlf true

  If you’re on a Linux or macOS system that uses LF line endings, then you don’t want Git to automatically convert them when you check out files; however, if a file with CRLF endings accidentally gets introduced, then you may want Git to fix it. You can tell Git to convert CRLF to LF on commit but not the other way around by setting core.autocrlf to input:

  > git config --global core.autocrlf input
  
  This setup should leave you with CRLF endings in Windows checkouts, but LF endings on macOS and Linux systems and in the repository.

  If you’re a Windows programmer doing a Windows-only project, then you can turn off this functionality, recording the carriage returns in the repository by setting the config value to false:

  > git config --global core.autocrlf false
