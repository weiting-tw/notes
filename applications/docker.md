# Docker

## Remove Docker Images, Containers, and Volumes

### Purging All Unused or Dangling Images, Containers, Volumes, and Networks

```bash
# all & force
docker system prune -af
```

### Remove one or more specific images

```bash
# images
docker images -a

# remove image
docker rmi Image Image
```

### Removing Containers

```bash
# containers
docker ps -a

# remove containers
docker rm ID_or_Name ID_or_Name
```

### Remove all exited containers

```bash
# exited containers
docker ps -a -f status=exited

# remove all exited containers
docker rm $(docker ps -a -f status=exited -q)
```

### Removing Volumes

```bash
# Volumes
docker volume ls

# remove
docker volume rm volume_name volume_name
```

### daemon.json 的作用

docker 安裝後預設沒有 daemon.json 這個配置檔案，需要進行手動建立。配置檔案的預設路徑：`/etc/docker/daemon.json`

```javascript
{
  "api-cors-header":"", 
  "authorization-plugins":[],
  "bip": "",
  "bridge":"",
  "cgroup-parent":"",
  "cluster-store":"",
  "cluster-store-opts":{},
  "cluster-advertise":"",
  #啟用debug的模式，啟用後，可以看到很多的啟動資訊。預設false
  "debug": true,
  "default-gateway":"",
  "default-gateway-v6":"",
  "default-runtime":"runc",
  "default-ulimits":{},
  "disable-legacy-registry":false,
  # 設定容器DNS的地址，在容器的 /etc/resolv.conf檔案中可檢視。
  "dns": ["192.168.1.1"],
  #  容器 /etc/resolv.conf 檔案，其他設定
  "dns-opts": [],
  # 設定容器的搜尋域，當設定搜尋域為 .example.com 時，在搜尋一個名為 host 的 主機時，DNS不僅搜尋host，還會搜索host.    example.com 。 注意：如果不設定， Docker 會預設用主機上的 /etc/resolv.conf 來配置容器。
  "dns-search": [],
  "exec-opts": [],
  "exec-root":"",
  "fixed-cidr":"",
  "fixed-cidr-v6":"",
  ＃已廢棄，使用data-root代替,這個主要看docker的版本
  "graph":"/var/lib/docker",
  ＃Docker執行時使用的根路徑,根路徑下的內容稍後介紹，預設/var/lib/docker
  "data-root":"/var/lib/docker",
  #Unix套接字的屬組,僅指/var/run/docker.sock
  "group": "",
  #設定容器hosts
  "hosts": [],
  "icc": false,
  #配置docker的私庫地址
  "insecure-registries": [],
  "ip":"0.0.0.0",
  "iptables": false,
  "ipv6": false,
  #預設true, 啟用 net.ipv4.ip_forward ,進入容器後使用 sysctl -a | grepnet.ipv4.ip_forward 檢視
  "ip-forward": false,
   "ip-masq":false,
  # docker主機的標籤，很實用的功能,例如定義：–label nodeName=host-121
  "labels":["nodeName=node-121"],
  "live-restore": true,
  "log-driver":"",
  "log-level":"",
  "log-opts": {},
  "max-concurrent-downloads":3,
  "max-concurrent-uploads":5,
  "mtu": 0,
  "oom-score-adjust":-500,
  #Docker守護程序的PID檔案
  "pidfile": "",
  "raw-logs": false,
  #映像加速的地址，增加後在 docker info中可檢視。
  "registry-mirrors":["xxxx"],
  "runtimes": {
     "runc": {
         "path": "runc"
     },
     "custom": {
         "path":"/usr/local/bin/my-runc-replacement",
         "runtimeArgs": [
             "--debug"
         ]
     }
  },
  #預設 false，啟用selinux支援
  "selinux-enabled": false,
  "storage-driver":"",
  "storage-opts": [],
  "swarm-default-advertise-addr":"",
  #預設 false, 啟動TLS認證開關
  "tls": true,
  #預設 ~/.docker/ca.pem，通過CA認證過的的certificate檔案路徑
  "tlscacert": "",
  #預設 ~/.docker/cert.pem ，TLS的certificate檔案路徑
  "tlscert": "",
  #預設~/.docker/key.pem，TLS的key檔案路徑
  "tlskey": "",
  #預設false，使用TLS並做後臺程序與客戶端通訊的驗證
  "tlsverify": true,
  "userland-proxy":false,
  "userns-remap":""
}
```

## 修改容器的 DNS 伺服器

檢視容器的 dns 解析設定檔案，也可以檢查 docker 執行環境 DNS

> docker run busybox:latest cat /etc/resolv.conf

為容器 mybusybox 執行手動設定一個 dns 伺服器，並檢查是否生效

> docker run --dns 10.0.0.2 --name mybusybox busybox:latest cat /etc/resolv.conf

定製化容器執行環境的 dns 伺服器，在 Host OS 上編輯下面檔案，增加 dns 伺服器，並重啟 docker 服務。

> cat /etc/docker/daemon.json

```bash
{
  "dns" : [
    "8.8.8.8"
  ]
}
```

重啟服務

> sudo service docker restart

## 複製容器內資料

```bash
docker cp <containerId>:<path> <toPath>
```

## 載入 volume

```bash
docker run -it --rm -v <volumeId>:/data node bash
```

## 使用 root 身分

```bash
docker exec -it --user root <containerId> /bin/bash
docker exec -it -u 0 <containerId> /bin/bash
```

## [如何由容器連線至宿主](https://docs.docker.com/docker-for-mac/networking/#use-cases-and-workarounds)

{% tabs %}
{% tab title="Docker Deskop for Windows/Mac 桌面開發環境" %}

`host.docker.internal` - 宿主

`gateway.docker.internal` - 網路閘道

```bash
# 在宿主執行 HTTP Server
python3 -m http.server 8000

# 在容器內連回宿主
$ docker run --rm -it alpine sh
apk add curl
curl http://host.docker.internal:8000
exit
```

{% endtab %}

{% tab title="Docker for Linux 伺服器原生環境" %}

可以透過名為 `docker0` 的[預設橋接器](https://docs.docker.com/network/bridge/)網路介面自容器內存取宿主

或是使用 `172.17.0.1`

```bash
# Ubuntu 需要安裝 iproute2 套件以使用 ip 指令
apt install iproute2

# 在容器內取得宿主橋接器網路位址,通常會 172.17.0.1
HOST_IP=$(ip route show | awk '/default/ {print $3}')
ping $HOST_IP
```

{% endtab %}
{% endtabs %}

## Docker-compose

### [Environment variables in Compose](https://docs.docker.com/compose/environment-variables/)

預設會抓 `docker-compose.yml` 根目錄下的 `.env`

也可以使用 `.env.dev`, `.env.prod` 等等檔案，然後使用 `--env-file .env.dev` 來選取

```bash
$ cat .env
TAG=v1.5

$ cat docker-compose.yml
version: '3'
services:
  web:
    image: "webapp:${TAG}"
```

可以使用 `docker-compose config` 來輸出結果

```bash
$ docker-compose config

version: '3'
services:
  web:
    image: 'webapp:v1.5'
```

## docker-compose up 啟動容器超時錯誤：ERROR: An HTTP request took too long to complete. Retry with --verbose to obtain debug information.

```bash
export COMPOSE_HTTP_TIMEOUT=500
export DOCKER_CLIENT_TIMEOUT=500
```

## 免下 sudo

```sh
# 增加 docker 群組
sudo groupadd docker

# 當前使用者加入 docker 群組中
sudo usermod -aG docker $USER

# 切換使用者使用的群組
newgrp docker
```

## 建置多架構的映像檔

https://github.com/docker/buildx

```bash
# 創建或切換到 buildx
docker buildx create --use

# 構建多架構映像檔並推送到 Docker Hub
docker buildx build --platform linux/386,linux/amd64,linux/arm64 --push -t your_dockerhub_username/your_image_name:tag .
```

## 如何在 `docker-compose.yml` 中支持相同的 `docker build` 命令？

對於代碼信息來說，您的情況需要略微調整 `docker-compose.yml` 配置以模仿相同行為。以下是基於您提供的指令對 `docker-compose.yml` 文件的配置示例：

### 修改後的 `docker-compose.yml` 示例
```yaml
version: "3.8"

services:
  gitlab_pr_agent:
    build:
      context: .                # 對應 `docker build .`
      dockerfile: docker/Dockerfile # 對應 `-f docker/Dockerfile`
      target: gitlab_webhook    # 對應 `--target gitlab_webhook`
    image: gitlab_pr_agent      # 對應 `-t gitlab_pr_agent`
    ports:
      - "3000:3000"
```

---

### 逐個部分解釋：
1. **`build.context`**
   - 這相當於 `docker build .` 中的 `.`，表示 Docker build 的上下文是當前工作目錄（`.`）。
   - 上下文目錄內的所有文件（包括 `docker/` 資料夾）都會被 Docker daemon 用於建構映像。

2. **`build.dockerfile`**
   - 使用 `docker/Dockerfile`，這相當於您在 `docker build` 命令中使用的 `-f docker/Dockerfile`。

3. **`build.target`**
   - 使用多階段構建 `--target gitlab_webhook`，這裡指定構建目標為您在 Dockerfile 中定義的 `gitlab_webhook` 階段。

4. **`image`**
   - 這指定了構建的映像名稱為 `gitlab_pr_agent`，等同於 `-t gitlab_pr_agent`。

5. **`ports`**
   - 這是一個額外配置，將容器內的端口 `3000` 暴露到主機上的端口 `3000`。

---

### 執行指令
在配置好 `docker-compose.yml` 後，可以使用以下指令來執行服務：

```bash
docker-compose up --build
```

這會同時進行構建和運行，`--build` 確保在啟動之前重新構建映像。

---

#### 注意事項：
1. **目錄結構須正確：**
   如果您傳入的 `context` 和 `dockerfile` 路徑無法對應正確的檔案，`docker-compose` 會出現錯誤。假設目錄結構如下：

   ```
   project/
   ├── docker-compose.yml
   ├── docker/
   │   └── Dockerfile
   └── app/
       └── main.py
   ```

   `docker-compose.yml` 中的 `context: .` 是 `project/`，`dockerfile` 必須相對於 `context`。

2. **多階段構建 (`--target`) 要求的特定目標：**
   在您的 `Dockerfile` 中應該有類似以下的多階段構建：

   ```dockerfile
   # 第一階段
   FROM python:3.8 AS base
   # 設置基礎環境...

   # Webhook 階段
   FROM base AS gitlab_webhook
   # 構建 webhook 的環境...
   ```

   `gitlab_webhook` 是一個特定的構建階段名稱，這需要在 Dockerfile 中作為標籤定義。如果不存在，您會遇到 `Unknown target stage` 的錯誤。

3. **手動構建/測試 `docker-compose up` 的過程：**
   若您在測試執行時仍然遇到錯誤，這裡有一些診斷指令：
   - 測試手動構建的效果是否正常（確認 `--target` 是否正確）：
     ```bash
     docker build . -f docker/Dockerfile --target gitlab_webhook -t gitlab_pr_agent
     ```
   - 查看構建過程的詳細日誌，以排除錯誤：
     ```bash
     docker-compose up --build --verbose
     ```
