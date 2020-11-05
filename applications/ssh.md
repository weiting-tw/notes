# SSH

## Windows Server 2016 install Open-SSH

1. Download [Win32-OpenSSH](https://github.com/PowerShell/Win32-OpenSSH/releases)
2. Set Environment Variables(Win32-openSSH Path)
3. Run [Powershell] and move to OpenSSH folder you located. Next, run a command `.\install-sshd.ps1` to install sshd
4. Open [Services] and start sshd. And also change to [Automatic] for [Startup Type].
5. Run [Powershell] `.\FixHostFilePermissions.ps1` to fix permissions.
6. Add Firewall 22/TCP port to allow SSH connection.

### Using public key to login

Update `%PROGRAMDATA%\ssh\sshd_config` settings.  
refer issues:  
<https://github.com/PowerShell/Win32-OpenSSH/issues/1358>  
<https://github.com/PowerShell/Win32-OpenSSH/issues/1306>

```text
PasswordAuthentication no
#Match Group administrators
#       AuthorizedKeysFile __PROGRAMDATA__/ssh/administrators_authorized_keys
```

and Add public key to `%PROGRAMDATA%\ssh\administrators_authorized_keys`

## Alias

在 `~/.ssh` 下新增 `config`

```config
Host alias-name                       # 用來連線的 alias 名稱
 HostName server.name                 # host domain 或 ip
 Port port-number                     # host 的 SSH port
 IdentitiesOnly yes                   # 使用指定的 key
 IdentityFile ~/.ssh/private_ssh_file # 指定 pem 或 pub 的 key 路徑
 User username-on-remote-machine      # (選填)登入 SSH 的 username，只連 git 的話，可以不必要
 ForwardX11 yes                       # (選填) 啟用回傳 GUI
```

like:

```config
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
