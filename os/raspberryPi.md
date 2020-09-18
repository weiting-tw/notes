# Raspberry Pi

## 當機自動重啟

### 加上核心模組

在 `/etc/modules` 內加上這個核心模組 `bcm2708_wdog`

> sudo vi /etc/modules

add this

> bcm2708_wdog

這樣在下次重新開機時，硬體看門狗模組就會自動載入。如果想要立即啟用，可以執行：

> sudo modprobe bcm2708_wdog

## 安裝 `watchdog`

> sudo apt-get install watchdog

安裝好之後，編輯 conf

> /etc/watchdog.conf

將 watchdog-device 註解拿掉：

> watchdog-device = /dev/watchdog

### watchdog 設定

#### 系統負載（loading）

這裡的數值是 `/etc/watchdog.conf` 的預設值，正常來說如果系統真的當機的話，系統的負載慧遠超過 `25`，這裡所設定的數值在系統正常的狀況下是不會重新啟動的。當然您可以依照自己的狀況來調整這個上限值。

```conf
max-load-1 = 24
max-load-5 = 18
max-load-15 = 12
```

#### CPU 溫度

監控樹莓派的 CPU 溫度，當溫度太高時，則關機

`/sys/class/thermal/thermal_zone0/temp` 中儲存的數值是控樹莓派的 CPU 溫度，單位是攝氏千分之一度，我們這裡設定讓 CPU 在超過攝氏 80ᵒC 時重新啟動。而 watchdog 再溫度達到這個數值的 90%、95% 與 98% 時，也會先發出警告。

```conf
temperature-device = /sys/class/thermal/thermal_zone0/temp
max-temperature = 80000
```

#### 記憶體

監控記憶體的使用量，當虛擬記憶體（virtual memory）太少時，則重新啟動

```conf
min-memory = 1
```

這裡的記憶體單位是 pages，您可以用 `getconf` 查看自己系統的 page size 是多大：

```sh
getconf PAGESIZE
# or
getconf PAGE_SIZE

# output (一張 page 的大小就是 4096 bytes。)
# 4096
```

#### 網路

監控某個 IP 位址

> ping = 172.31.14.1

亦可用廣播位址監控整個 subnet 上的主機（這個要小心使用）：

> ping = 172.26.1.255

監控網路卡式否有流量：

> interface = eth0

#### 檔案

監控檔案是否可以正常存取，如果無法存取檔案，則重新啟動：

> file = /var/log/messages

watchdog 會使用 stat 檢查檔案，如果傳回錯誤的話並不會重新啟動，如果 stat 在呼叫時卡住，超過一分鐘之後 watchdog 才會將系統重新啟動，這個狀況通常會發生 NFS 掛載的檔案系統上。

#### 通知 Mail

設定管理者的 Email，讓系統重新啟動或是關機前，以 Email 通知管理者：

> admin = xxx@gmail.com

#### 即時監控模式

在負載量比較大的系統中，`watchdog` 很容易被作業系統搬到記憶體的交換空間（swap），如果來不及搬回記憶體時，很容易造成系統不必要的重新啟動，如果想避免這樣的狀況，可以啟用即時監控模式（realtime），讓 watchdog 常駐在記憶體中：

> realtime = yes

### 開機自動啟動看門狗

先啟動 watchdog 常駐程式進行測試：

> sudo service watchdog start

當確認設定沒問題之後，就可以讓 watchdog 在開機時就自動啟動：

> sudo update-rc.d watchdog defaults

### 當機測試

安裝好看門狗之後，可以用這個 `fork-bombs` 測試一下，執行這個指令之後，它會不停重複 fork 出新的行程，造成系統當機：

> :(){ :|:& };:

正常來說，執行這個指令稿之後，系統就會馬上當住，您可以透過這個方式測試看門狗是否會讓系統重新啟動。

**請不要在一般的 Linux 主機上執行 fork-bombs，它會造成系統當機！**

正常在執行 fork-bombs 之後，系統負載會在幾秒之內迅速上升，然後重新開機，這是 `/var/log/daemon.log`：

```log
Feb 16 14:14:56 raspberrypi watchdog[3818]: loadavg 88 18 6 is higher than the given threshold 24 18 12!
Feb 16 14:14:56 raspberrypi watchdog[3818]: /usr/lib/sendmail does not exist or is not executable (errno = 2)
Feb 16 14:14:56 raspberrypi watchdog[3818]: shutting down the system because of error -3
```

## l2tp

open port: `500` & `4500`

> wget https://git.io/vpnsetup -O vpnsetup.sh
> nano vpnsetup.sh

update settings:

- YOUR_IPSEC_PSK=""
- YOUR_USERNAME=""
- YOUR_PASSWORD=""

> sudo sh vpnsetup.sh

### 設置用戶名密碼

> nano /etc/ppp/chap-secrets

```bash
#用戶名    服務名    密碼    指定IP

username    *    "password"    *
```

### 設置PSK預共享密鑰

> nano /etc/ipsec.secrets

```bash
# source    destination

51.15.139.201 51.15.44.48 : PSK "87zRQqylaoeF5I8o4lRhwvmUzf+pYdDpsCOlesIeFA/2xrtxKXJTbCPZgqplnXgPX5uprL+aRgxD8ua7MmdWaQ"
%any  %any  : PSK "xxxx"
```

### split log

這裡可以利用 syslog 來配置

在/etc/rsyslog.d/ 下新建 `20-xl2tpd.conf` 文件，內容如下：

> vi /etc/rsyslog.d/20-xl2tpd.conf

``` conf
if $programname == 'xl2tpd' then /var/log/l2tp.log

&~
```

在/etc/rsyslog.d/ 下新建 `20-pptpd.conf` 文件，內容如下：

> vi /etc/rsyslog.d/20-pptpd.conf

``` conf
if $programname == 'pppd' then /var/log/l2tp.log

&~
```

> service rsyslog restart

### 服務重啟

> service ipsec restart
> service xl2tpd restart
