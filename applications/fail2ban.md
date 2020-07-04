# Fail2ban

Fail2ban 是一套以 Python 語言所撰寫的 GPLv2 授權軟體，藉由分析系統紀錄檔，並透過設定過濾條件 (filter) 及動作 (action)，當符合我們所設定的過濾條件時，將觸發相對動作來達到自動化反應的效果 (如封鎖來源 IP、寄信通知管理者、查詢來源 IP 資訊等)

## 設定檔說明

- jail.(conf|local)
  - 用來設定 jail，即是定義 filter 與 action 的對應關係。

- filter.d/
  - 用來定義過濾條件 (filter)，目錄下已定義多種既有的過濾條件，常見的軟體有 apache、sshd、vsftpd、postfix 等，而常見記錄檔格式也可能為 Syslog、Common Log Format 等。

- action.d/
  - 用來定義動作內容 (action)，目錄下已定義多種既有的動作內容，如「sendmail 寄信通知」、「iptables 阻擋來源位址」、「使用 whois 查詢來源 domain 資訊」或「自動通知該來源 IP 的管理者」。

### jail.local

default

```local
[DEFAULT]
#忽略 IP 的清單，以空白區隔不同 IP
ignoreip = 127.0.0.1/8 192.168.1.0/24
#封鎖的時間，單位:秒，600=10分鐘，改為 -1 表示「永久」封鎖  
bantime  = 600
#在多久的時間內，單位:秒，600=10分鐘
findtime = 600
#登入失敗幾次封鎖
maxretry = 3
```

for bitwarden

```local
[bitwarden]
enabled = true
port = 80,443,8081
filter = bitwarden
action = iptables-allports[name=bitwarden]
         sendmail-whois[name=bitwarden, dest=<dest mail>, sender=<sender mail>]
logpath = /path/to/bitwarden.log
maxretry = 3
bantime = 14400
findtime = 14400
```

### filter.local

for bitwarden

```local
[INCLUDES]
before = common.conf

[Definition]
failregex = ^.*Username or password is incorrect\. Try again\. IP: <ADDR>\. Username:.*$
ignoreregex =
```

## Using Docker-compose

```yml
version: '3'
services:
  fail2ban:
   container_name: fail2ban
   restart: always
   image: crazymax/fail2ban:latest
   environment: 
     - TZ=Asia/Taipei
     - F2B_DB_PURGE_AGE=7d
     - F2B_LOG_TARGET=/data/fail2ban.log
     - F2B_LOG_LEVEL=INFO
     - F2B_IPTABLES_CHAIN=INPUT
     - SSMTP_HOST=mail.example.com
     - SSMTP_PORT=465
     - SSMTP_HOSTNAME=example.com
     - SSMTP_USER=<change me>
     - SSMTP_PASSWORD=<change me>
     - SSMTP_TLS=YES
   volumes:
     - /volume1/docker/fail2ban:/data
     - /volume1/docker/Bitwarden:/bitwarden:ro # for bitwarden
   network_mode: "host"
   privileged: true
   cap_add:
     - NET_ADMIN
     - NET_RAW
```

## Commands

```sh
#啟動 fail2ban
systemctl start fail2ban

#停止 fail2ban
systemctl stop fail2ban

#重啟 fail2ban
systemctl restart fail2ban

#檢查 fail2ban 是否正常運作
fail2ban-client ping
// 正常的話會回應：
// Server replied: pong

#重新載入並套用設定檔
fail2ban-client reload

#顯示所有套用的設定值
fail2ban-client -d

#顯示 fail2ban 的所有規則統計
fail2ban-client status
// Status
// |- Number of jail:      1
// `- Jail list:   sshd

#顯示已封鎖的 IP 統計
fail2ban-client status sshd
fail2ban-client status nginx-cc

#顯示已封鎖的 IP (SSH) 與剩餘解封時間
ipset list fail2ban-sshd

#顯示封鎖規則
firewall-cmd --direct --get-all-rules
// ipv4 filter INPUT 0 -p tcp -m multiport --dports ssh -m set --match-set fail2ban-sshd src -j REJECT --reject-with icmp-port-unreachable

#取消封鎖指定 IP
fail2ban-client set sshd unbanip xxx.xxx.xxx.xxx
fail2ban-client set nginx-cc unbanip xxx.xxx.xxx.xxx
```

for docker
前面加上 `sudo docker exec -t fail2ban`

like:

```sh
sudo docker exec -t fail2ban fail2ban-client set sshd unbanip xxx.xxx.xxx.xxx
```