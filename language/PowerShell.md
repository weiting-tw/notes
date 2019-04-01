# [Windows PowerShell](https://ss64.com/ps/)

## [使用靜態類別和方法](https://docs.microsoft.com/zh-tw/powershell/scripting/getting-started/cookbooks/using-static-classes-and-methods?view=powershell-6)

example:

```PowerShell
PS> [DateTime]::Now.ToString()

8/28/2018 2:16:20 AM
```

## PowerShell 執行 ps1 檔時出現「系統上已停用指令碼執行」錯誤

檔案無法載入，因為這個系統已停用指令碼執行」的訊息，表示在目前作業系統中的執行原則 ( Excution Policy ) 預設狀態為 Restricted，也就是不允許執行。

可透過一些指令進行執行原則變更，首先請用管理員權限打開 PowerShell，執行以下指令：

```PowerShell
Set-ExecutionPolicy RemoteSigned
```
