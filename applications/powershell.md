# [PowerShell](https://docs.microsoft.com/zh-tw/powershell/scripting/overview?view=powershell-7.1)

## ignore output

```powershell
Add-Item > $null

Add-Item | Out-Null
```

## 取得最後執行狀態碼

```powershell
$?
```

## 檔案操作

```powershell

# 偵測檔案路徑存在
Test-Path "$DEPLOY_PATH\review\$CI_COMMIT_REF_NAME"

# 刪除檔案
Remove-Item "$DEPLOY_PATH\review\$CI_COMMIT_REF_NAME" -Recurse -Verbose -Force

# 複製檔案
Copy-Item -Path .\build\ -Destination "$DEPLOY_PATH\review\$CI_COMMIT_REF_NAME" -Recurse -Verbose -Force
```

## [使用指令添加環境變數](https://docs.microsoft.com/zh-tw/powershell/module/microsoft.powershell.core/about/about_environment_variables?view=powershell-7.2)

```powershell
[Environment]::SetEnvironmentVariable('Foo','Bar') # 使用者
[Environment]::SetEnvironmentVariable('Foo', 'Bar', 'Machine') # 系統
[Environment]::GetEnvironmentVariable('Foo')
```
