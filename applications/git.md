# Git

## Push when using two-factor authenticationg

啟用二階段驗證時，會無法push到server上

```sh
> git push
Username for 'https://github.com': a26007565
Password for 'https://a26007565@github.com':
remote: Invalid username or password.
fatal: Authentication failed for 'https://github.com/a26007565/notes.git/'
```

解決方法:

1. git config –global credential.helper store
2. Settings => Personal access tokens
3. Generate new token => token的範圍選repo就夠了 => click Generate Token
4. 再次輸入：git push，輸入帳號，再密碼的地方輸入token就行了

**備註** 訊息會存在
> ~/.git-credentials

## Generating a new SSH key

Open Git Bash.

Paste the text below, substituting in your GitHub email address.

```sh
ssh-keygen -t rsa -b 4096 -C <your_email@example.com>
```

This creates a new ssh key, using the provided email as a label.

```sh
Generating public/private rsa key pair.
```

When you're prompted to "Enter a file in which to save the key," press Enter. This accepts the default file location.

```sh
Enter a file in which to save the key (/c/Users/you/.ssh/id_rsa):[Press enter]
```

At the prompt, type a secure passphrase. For more information, see "Working with SSH key passphrases".

```sh
Enter passphrase (empty for no passphrase): [Type a passphrase]
Enter same passphrase again: [Type passphrase again]
```

## Adding your SSH key to the ssh-agent

Before adding a new SSH key to the ssh-agent to manage your keys, you should have checked for existing SSH keys and [generated a new SSH key](#generating-a-new-ssh-key).
When adding your SSH key to the agent, use the default macOS ssh-add command, and not an application installed by `macports`, [`homebrew`](../os/macOS.md#Homebrew-套件管理工具), or some other external source.

Start the ssh-agent in the background.

```sh
eval "$(ssh-agent -s)"
Agent pid 59566
```

If you're using macOS Sierra 10.12.2 or later, you will need to modify your `~/.ssh/config` file to automatically load keys into the ssh-agent and store passphrases in your keychain.

```sh
Host *
 AddKeysToAgent yes
 UseKeychain yes
 IdentityFile ~/.ssh/id_rsa
```

Add your SSH private key to the ssh-agent and store your passphrase in the keychain. If you created your key with a different name, or if you are adding an existing key that has a different name, replace id_rsa in the command with the name of your private key file.

> ssh-add -K ~/.ssh/id_rsa

Note: The -K option is Apple's standard version of ssh-add, which stores the passphrase in your keychain for you when you add an ssh key to the ssh-agent.

If you don't have Apple's standard version installed, you may receive an error. For more information on resolving this error, see "[Error: ssh-add: illegal option -- K](https://help.github.com/articles/error-ssh-add-illegal-option-k)."*

## 救回 Commit

誤 reset 時，使用 `reflog` 觀看 git 的歷程

```sh
> git reflog
657fce7 (HEAD -> master) HEAD@{0}: reset: moving to HEAD~2
e12d8ef (origin/master, origin/HEAD, cat) HEAD@{1}: checkout: moving from cat to master
e12d8ef (origin/master, origin/HEAD, cat) HEAD@{2}: checkout: moving from master to cat
```

指定 Commit 即可救回

```sh
> git reset e12d8ef --hard
```