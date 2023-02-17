# Windows 10

## port forwarding

將 port 80 導向 59600

```bash
// 新增
> netsh interface portproxy add v4tov4 listenport=80 listenaddress=* connectport=59600 connectaddress=127.0.0.1

// 顯示
> netsh interface portproxy show all

接聽 ipv4:             連線到 ipv4:

位址            連接埠      位址            連接埠
--------------- ----------  --------------- ----------
*               80          127.0.0.1       59600

// 刪除
netsh interface portproxy delete v4tov4 listenport=80 listenaddress=*
```

## [WSL](https://docs.microsoft.com/en-us/windows/wsl/wsl-config#wsl-2-settings)

### 使用 wslconfig 設定全域選項

適用于 Windows 組建 19041 和更新版本

將 `.wslconfig` 放到 [使用者] 資料夾的根目錄中，以設定全域 WSL ex：C:\Users\<yourUserName>\.wslconfig

* 須重開才會套用 `wsl --shutdown`

#### wslconfig 範例

```config
[wsl2]
kernel=C:\\temp\\myCustomKernel
memory=4GB # Limits VM memory in WSL 2 to 4 GB
processors=2 # Makes the WSL 2 VM use two virtual processors
```

| key                 | value   | default                                                                                                               | notes                                                                                                                              |
| ------------------- | ------- | --------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| kernel              | string  | The Microsoft built kernel provided inbox                                                                             | An absolute Windows path to a custom Linux kernel.                                                                                 |
| memory              | size    | 50% of total memory on Windows or 8GB, whichever is less; on builds before 20175: 80% of your total memory on Windows | How much memory to assign to the WSL 2 VM.                                                                                         |
| processors          | number  | The same number of processors on Windows                                                                              | How many processors to assign to the WSL 2 VM.                                                                                     |
| localhostForwarding | boolean | true                                                                                                                  | Boolean specifying if ports bound to wildcard or localhost in the WSL 2 VM should be connectable from the host via localhost:port. |
| kernelCommandLine   | string  | Blank                                                                                                                 | Additional kernel command line arguments.                                                                                          |
| swap                | size    | 25% of memory size on Windows rounded up to the nearest GB                                                            | How much swap space to add to the WSL 2 VM, 0 for no swap file.                                                                    |
| swapFile            | string  | %USERPROFILE%\AppData\Local\Temp\swap.vhdx                                                                            | An absolute Windows path to the swap virtual hard disk.                                                                            |

Entries with the path value must be Windows paths with escaped backslashes, e.g: C:\\Temp\\myCustomKernel

Entries with the size value must be a size followed by a unit, for example 8GB or 512MB.

### 移動 docker image 所佔用空間

預設位置：`%LOCALAPPDATA%/Docker/wsl`

默認情況下，Docker Desktop for Window 會創建如下兩個發行版（distro）：

> data/ext4.vhdx 是被 docker-desktop-data 使用
> distro/ext4.vhdx 是被 docker-desktop 使用

```bash
# 關閉 wsl
wsl --shutdown

# 列出所有資源
D:\work>wsl --list --verbose
NAME                   STATE           VERSION
docker-desktop         Stopped         2
docker-desktop-data    Stopped         2

# 備份出來
wsl --export docker-desktop-data E:\docker-desktop\docker-desktop-data.tar

# 移除當前 docker-desktop-data
wsl --unregister docker-desktop-data

# 重新匯入，記得把備份出的移除
wsl --import docker-desktop-data E:\docker-desktop\data E:\docker-desktop\docker-desktop-data.tar --version 2
```

## IPsec/L2TP

### 無法連線時解決方式

#### Windows 錯誤 809

> 錯誤 809：無法建立計算機與 VPN 服務器之間的網路連接，因為遠程服務器無回應。這可能是因為未將計算機與遠程服務器之間的某種網路設備 (如防火牆、NAT、路由器等) 配置為允許 VPN 連接。請與管理員或服務提供商聯系以確定哪種設備可能產生此問題。

**註：** 僅當你使用 IPsec/L2TP 模式連接到 VPN 時，才需要進行的註冊表更改。對於 `IKEv2` 和 `IPsec/XAuth` 模式，無需進行此更改。

要解決此錯誤，在首次連接之前需要[修改一次註冊表](https://documentation.meraki.com/MX-Z/Client_VPN/Troubleshooting_Client_VPN#Windows_Error_809)，以解決 VPN 服務器 和/或 客戶端與 NAT（比如路由器）的相容問題。請下載並導入下麵的 `.reg` 文件，或者打開 [系統管理員](http://www.cnblogs.com/xxcanghai/p/4610054.html) 並運行以下命令。**完成後必須重啟。**

* 適用於 Windows Vista, 7, 8.x 和 10 ([下載 .reg 文件](https://dl.ls20.com/reg-files/v1/Fix_VPN_Error_809_Windows_Vista_7_8_10_Reboot_Required.reg))

  ```console
  REG ADD HKLM\SYSTEM\CurrentControlSet\Services\PolicyAgent /v AssumeUDPEncapsulationContextOnSendRule /t REG_DWORD /d 0x2 /f
  ```

* 僅適用於 Windows XP ([下載 .reg 文件](https://dl.ls20.com/reg-files/v1/Fix_VPN_Error_809_Windows_XP_ONLY_Reboot_Required.reg))

  ```console
  REG ADD HKLM\SYSTEM\CurrentControlSet\Services\IPSec /v AssumeUDPEncapsulationContextOnSendRule /t REG_DWORD /d 0x2 /f
  ```

另外，某些個別的 Windows 系統禁用了 IPsec 加密，也會導致連接失敗。要重新啟用它，可以運行以下命令並重啟。

* 適用於 Windows XP, Vista, 7, 8.x 和 10 ([下載 .reg 文件](https://dl.ls20.com/reg-files/v1/Fix_VPN_Error_809_Allow_IPsec_Reboot_Required.reg))

  ```console
  REG ADD HKLM\SYSTEM\CurrentControlSet\Services\RasMan\Parameters /v ProhibitIpSec /t REG_DWORD /d 0x0 /f
  ```

## 刪除超過 30 天的檔案

> ForFiles /p "C:\path\to\folder" /s /d -30 /c "cmd /c del /q @file"

## 刪除空的資料夾

> (gci "C:\path\to\folder" -r | ? {$_.PSIsContainer -eq $True}) | ?{$_.GetFileSystemInfos().Count -eq 0} | remove-item

## 查找已被佔用的 port

> netstat -ano | findstr 0.0:80

### 查找禁用範圍

> netsh interface ipv4 show excludedportrange protocol=tcp

## 清除 Windows TCP Port 保留區段

> net stop winnat
> net start winnat