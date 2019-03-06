# Windows 10

## port forwarding

將 port 80 導向 59600

```sh

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