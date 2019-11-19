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
