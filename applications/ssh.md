# SSH

Secure Shell is a protocol used to securely log onto remote systems. It can be used for logging or executing commands on a remote server.

## Windows Server 2016 install Open-SSH

1. Download [Win32-OpenSSH](https://github.com/PowerShell/Win32-OpenSSH/releases)
2. Set Environment Variables\(Win32-openSSH Path\)
3. Run \[Powershell\] and move to OpenSSH folder you located. Next, run a command `.\install-sshd.ps1` to install sshd
4. Open \[Services\] and start sshd. And also change to \[Automatic\] for \[Startup Type\].
5. Run \[Powershell\] `.\FixHostFilePermissions.ps1` to fix permissions.
6. Add Firewall 22/TCP port to allow SSH connection.

### Using public key to login

Update `%PROGRAMDATA%\ssh\sshd_config` settings.  
refer issues:  
[https://github.com/PowerShell/Win32-OpenSSH/issues/1358](https://github.com/PowerShell/Win32-OpenSSH/issues/1358)  
[https://github.com/PowerShell/Win32-OpenSSH/issues/1306](https://github.com/PowerShell/Win32-OpenSSH/issues/1306)

```text
PasswordAuthentication no
#Match Group administrators
#       AuthorizedKeysFile __PROGRAMDATA__/ssh/administrators_authorized_keys
```

and Add public key to `%PROGRAMDATA%\ssh\administrators_authorized_keys`

## Alias

在 `~/.ssh` 下新增 `config`

```text
Host alias-name                       # 用來連線的 alias 名稱
 HostName server.name                 # host domain 或 ip
 Port port-number                     # host 的 SSH port
 IdentitiesOnly yes                   # 使用指定的 key
 IdentityFile ~/.ssh/private_ssh_file # 指定 pem 或 pub 的 key 路徑
 User username-on-remote-machine      # (選填)登入 SSH 的 username，只連 git 的話，可以不必要
 ForwardX11 yes                       # (選填) 啟用回傳 GUI
```

like:

```text
Host cs-bizform-test
 HostName cs-bizform-test
 IdentitiesOnly yes
 IdentityFile ~/.ssh/id_rsa
 User wilber_chen
```

基本 SSH 連線

> ssh alias-name

SSH 連線再附加其他指令

> ssh alias-name mkdir wahaha

scp 指定 SSH name

> scp -r ./ alias-name:/var/www/html/project/

如果出現以下錯誤訊息：

`Bad owner or permissions on /home/user/.ssh/config`

請將 config 檔案設置比較小的權限：

> chmod 600 config

## ssh-keygen 常用參數詳解

ssh-keygen是SSH服務下的一個生成、管理和轉換認證密鑰的命令工具。包括兩種密鑰類型DSA和RSA 通過公私鑰的驗證可以使服務器與服務器之間實現無密碼通訊。 ssh-keygen常用參數

```bash
-t：指定生成密鑰的類型，默認使用SSH2d的rsa
-f：指定生成密鑰的文件名，默認id_rsa（私鑰id_rsa，公鑰id_rsa.pub）
-P：提供舊密碼，空表示不需要密碼（-P ‘’）
-N：提供新密碼，空表示不需要密碼(-N ‘’)
-b：指定密鑰長度（bits），RSA最小要求768位，默認是2048位；DSA密鑰必須是1024位（FIPS 1862標準規定）
-C：提供一個新註釋
-R hostname：從known_hosta（第一次連接時就會在家目錄.ssh目錄下生產該密鑰文件）文件中刪除所有屬於hostname的密鑰
```

```bash
ssh-keygen -t ed25519 -C "<comment>"
```

## Arguments

* 連到 remote\_host:

```bash
ssh username@remote_host
```

* 使用指定 `私鑰` 連到 remote\_host:

```bash
ssh -i path/to/key_file username@remote_host
```

* 使用指定 port 連到 remote\_host:

```bash
ssh username@remote_host -p 2222
```

* 在 remote\_host 上執行 command:

```bash
ssh remote_host command -with -flags
```

* SSH tunneling: Dynamic port forwarding \(SOCKS proxy on localhost:9999\):

```bash
ssh -D 9999 -C username@remote_host
```

* SSH tunneling: Forward a specific port \(localhost:9999 to example.org:80\) along with disabling pseudo-\[t\]ty allocation and executio\[n\] of remote commands:

```bash
ssh -L 9999:example.org:80 -N -T username@remote_host
```

* SSH jumping: Connect through a jumphost to a remote server \(Multiple jump hops may be specified separated by comma characters\):

```bash
ssh -J username@jump_host username@remote_host
```

* Agent forwarding: Forward the authentication information to the remote machine \(see `man ssh_config` for available options\):

```bash
ssh -A username@remote_host
```

