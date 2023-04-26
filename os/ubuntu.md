# Ubuntu

## Swap File

### [Why is swap needed?](https://itsfoss.com/swap-size/)

There are several reasons why you would need swap.

- If your system has RAM less than 1 GB, you must use swap as most applications would exhaust the RAM soon.
- If your system uses resource heavy applications like video editors, it would be a good idea to use some swap space as your RAM may be exhausted here.
- If you use hibernation, then you must add swap because the content of the RAM will be written to the swap partition. This also means that the swap size should be at least the size of RAM.
- Avoid strange events like a program going nuts and eating RAM.

### How much should be the swap size?

| RAM Size | Swap Size (Without Hibernation) | Swap size (With Hibernation) |
| -------: | ------------------------------: | ---------------------------: |
|    256MB |                           256MB |                        512MB |
|    512MB |                           512MB |                          1GB |
|      1GB |                             1GB |                          2GB |
|      2GB |                             1GB |                          3GB |
|      3GB |                             2GB |                          5GB |
|      4GB |                             2GB |                          6GB |
|      6GB |                             2GB |                          8GB |
|      8GB |                             3GB |                         11GB |
|     12GB |                             3GB |                         15GB |
|     16GB |                             4GB |                         20GB |
|     24GB |                             5GB |                         29GB |
|     32GB |                             6GB |                         38GB |
|     64GB |                             8GB |                         72GB |
|    128GB |                            11GB |                        139GB |

### [Create and Use Swap File on Linux](https://itsfoss.com/create-swap-file-linux/)

#### Check swap space in Linux

```bash
$ free -h

total        used        free      shared  buff/cache   available
Mem:         906Mi       451Mi       118Mi       0.0Ki       335Mi       311Mi
Swap:        2.0Gi       416Mi       1.6Gi
```

Show swap file partition path

```bash
$ swapon --show 

NAME      TYPE SIZE   USED PRIO
/swapfile file   2G 413.5M   -2
```

#### Make a new swap file

```bash
sudo fallocate -l 1G /swapfile
```

It is recommended to allow only root to read and write to the swap file. You’ll even see warning like “insecure permissions 0644, 0600 suggested” when you try to use this file for swap area.

```bash
sudo chmod 600 /swapfile
```

Do note that the name of the swap file could be anything. If you need multiple swap spaces, you can give it any appropriate name like swap_file_1, swap_file_2 etc. It’s just a file with a predefined size.

#### Mark the new file as swap space

Your need to tell the Linux system that this file will be used as swap space. You can do that with mkswap tool.

```bash
sudo mkswap /swapfile

# You should see an output like this:
Setting up swapspace version 1, size = 1024 MiB (1073737728 bytes)
no label, UUID=7e1faacb-ea93-4c49-a53d-fb40f3ce016a
```

#### Enable the swap file

```bash
sudo swapon /swapfile
```

Now if you check the swap space, you should see that your Linux system recognizes and uses it as the swap area:

```bash
swapon --show

# output
NAME       TYPE   SIZE USED PRIO
/swapfile  file 1024M   0B   -2

# or
cat /proc/swaps

Filename              Type            Size            Used            Priority
/dev/zram0            partition       235976          210532          -2
```

#### Make the changes permanent

Whatever you have done so far is temporary. Reboot your system and all the changes will disappear.

You can make the changes permanent by adding the newly created swap file to `/etc/fstab` file.

It’s always a good idea to make a backup before you make any changes to the `/etc/fstab` file.

```bash
sudo cp /etc/fstab /etc/fstab.back
```

Now you can add the following line to the end of `/etc/fstab` file:

```bash
/swapfile none swap sw 0 0
```

You can do it manually using a command line text editor or you just use the following command:

```bash
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```

Now you have everything in place. Your swap file will be used even after you reboot your Linux system.

#### Adjust swappiness

數值越高代表交換的越頻繁，[0~100] Default: 60

```bash
sudo sysctl vm.swappiness=25
```

永久化，將 `vm.swappiness=25` 寫入到 `/etc/sysctl.conf`

#### Resizing swap space on Linux

較好的方式先建立另一個新的 swap file 並啟用，在 swap 舊的時，會將資料移過去

如果記憶體有足夠的空間時

```bash
# 關閉要調整大小的
sudo swapoff /swapfile

# 配置成2G
sudo fallocate -l 2G /swapfile

# 製作成 swap file
sudo mkswap /swapfile

# 啟用
sudo swapon /swapfile
```

#### Removing swap file in Linux

如果記憶體有足夠的空間時

```bash
# 關閉要調整大小的
sudo swapoff /swapfile

# 刪除
sudo rm /swapfile
```

## 查詢目前正在使用的 PORT 號

> sudo lsof -i -n -P|grep LISTEN
> sudo netstat -lpn |grep {port}

## 硬碟效能

`hdparm` is a good place to start.

```bash
sudo hdparm -Tt /dev/sda

/dev/sda:
Timing cached reads:   12540 MB in  2.00 seconds = 6277.67 MB/sec
Timing buffered disk reads: 234 MB in  3.00 seconds =  77.98 MB/sec
```

`sudo hdparm -v /dev/sda` will give information as well.

dd will give you information on write speed.

If the drive doesn't have a file system (and only then), use of=`/dev/sda`.

Otherwise, mount it on /tmp and write then delete the test output file.

```bash
dd if=/dev/zero of=/tmp/output bs=8k count=10k; rm -f /tmp/output

10240+0 records in
10240+0 records out
83886080 bytes (84 MB) copied, 1.08009 s, 77.7 MB/s
```

## 更新 hostname

```bash
hostnamectl [OPTIONS...] COMMAND ...

Query or change system hostname.

  -h --help              Show this help
     --version           Show package version
     --no-ask-password   Do not prompt for password
  -H --host=[USER@]HOST  Operate on remote host
  -M --machine=CONTAINER Operate on local container
     --transient         Only set transient hostname
     --static            Only set static hostname
     --pretty            Only set pretty hostname

Commands:
  status                 Show current hostname settings
  set-hostname NAME      Set system hostname
  set-icon-name NAME     Set icon name for host
  set-chassis NAME       Set chassis type for host
  set-deployment NAME    Set deployment environment for host
  set-location NAME      Set location for host
```

把主機名稱修改成 ubuntu-vm-x64：
> sudo hostnamectl set-hostname ubuntu-vm-x64

說用 `hostnamectl status` 檢查有沒有修改成功：

```bash
   Static hostname: ubuntu-vm-x64
         Icon name: computer-vm
           Chassis: vm
        Machine ID: 0f5a04db6f2347ee961e520a9bf233bb
           Boot ID: 41691ad2b02044b88c3895f4a40ec3a5
    Virtualization: kvm
  Operating System: Ubuntu 20.04.3 LTS
            Kernel: Linux 5.4.0-89-generic
      Architecture: x86-64
```

## Daemon 註冊

### 新增服務檔案

> sudo nano /etc/systemd/system/openfortivpn.service

## 設定

```bash
[Unit]
Description=OpenFortiVPN Daemon
After=network-online.target
Wants=network-online.target

[Service]
Type=simple
ExecStart=/usr/bin/openfortivpn --config=/etc/openfortivpn/config --persistent
Restart=always
RestartSec=30

[Install]
WantedBy=multi-user.target
```

### 註冊及啟用

```bash
sudo systemctl daemon-reload
sudo systemctl enable openfortivpn.service
sudo systemctl start openfortivpn.service
```

### 查看 log

```bash
sudo journalctl -u openfortivpn.service
```
