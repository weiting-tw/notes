# Line Bot

## Line Beacon

### PRE-REQUIRED

- Node.js v6.x.x+
- python 2.7

### Install

- 安裝套件
  > sudo apt-get install bluetooth bluez libbluetooth-dev libudev-dev

- 安裝 node@6 或使用 [nvm](https://github.com/nvm-sh/nvm) 安裝
  > curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
  > sudo apt-get install nodejs
  > nvm install 6
  
- Clone repo
  > git clone <https://github.com/line/line-simple-beacon.git>

- 安裝相依性套件
  > cd line-simple-beacon/tools/line-simplebeacon-nodejs-sample/
  > npm install

- 執行
  > sudo ./simplebeacon.js --hwid=xxx

- 可能錯誤

  - 出現 `/usr/bin/env: ‘node’: No such file or directory`

  ```bash
  n=$(which node); n=${n%/bin/node}; chmod -R 755 $n/bin/*; sudo cp -r $n/{bin,lib,share} /usr/local
  ```

  - 出現 `Error: Cannot find module 'bluetooth-hci-socket'`，因為不是用 ptyhon2 去編譯套件造成

  ```bash
  sudo apt install python2
  sudo update-alternatives --install /usr/bin/python python /usr/bin/python2.7 1
  sudo update-alternatives --config python
  ```

### 註冊 Beacon

<https://manager.line.biz/beacon/register>

### 註冊背景服務

```bash
sudo nano /lib/systemd/system/lineBeacon.service
```

```service
[Unit]
Description=Line Beacon

[Service]
Type=simple
ExecStartPre=/bin/sleep 15
ExecStart=/home/pi/line-simple-beacon/tools/line-simplebeacon-nodejs-sample/simplebeacon.js --hwid=xxx &
Restart=always

[Install]
WantedBy=multi-user.target
```

```bash
# 重新讀取服務資訊
sudo systemctl daemon-reload
# 啟動服務
sudo systemctl enable lineBeacon
# 開始服務
sudo systemctl start lineBeacon
# 服務的狀態
sudo systemctl status lineBeacon
```
