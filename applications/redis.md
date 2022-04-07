# Redis

## 安裝

### [Docker](https://github.com/docker-library/redis)

```bash
docker run -p 6379:6379 redis:alpine
```

### [Ubuntu](https://launchpad.net/~chris-lea/+archive/ubuntu/redis-server)

```bash
sudo add-apt-repository ppa:chris-lea/redis-server
sudo apt update
sudo apt install redis

# 啟用服務
sudo systemctl enable redis
# 啟動服務
sudo systemctl start redis
```

### [Windows](https://www.memurai.com/)

已知 [Redis on Windows](https://github.com/microsoftarchive/redis) 專案已不再維護, 建議改用 [Memurai](https://www.memurai.com/get-memurai)

> Memurai 開發者版本有每隔 10 天就需要重啟服務的限制

```text
# 設定系統排程每隔 9 天自動重啟 Memurai 服務
Register-ScheduledTask -TaskName RestartMemurai `
-Trigger (New-JobTrigger -Daily -At 0am -DaysInterval 9) `
-User SYSTEM `
-RunLevel Highest `
-Action (New-ScheduledTaskAction `
 -Execute powershell `
 -Argument '-ExecutionPolicy Bypass -Command "Restart-Service Memurai"' `
 )
```

## [常用指令](http://redis.io/commands)

```bash
# 查詢指令用法
redis-cli help

# 查詢常用指令
redis-cli help @generic

# 查詢連線相關指令
redis-cli help @connection

# 查詢伺服器管理指令
redis-cli help @server

# 查詢指定指令的用法
redis-cli help auth

## https://redis.io/commands/info

# 取得主機所有資訊
redis-cli info

# 取得伺服端資訊
redis-cli info server

# 取得用戶端資訊
redis-cli info clients

# 取得複寫資訊
redis-cli info replication

# 取得資料庫統計資訊
redis-cli info keyspace

## https://redis.io/commands/set

# 設定指定鍵值
redis-cli set key "value"

## https://redis.io/commands/get

# 取得指定鍵值
redis-cli get key

## https://redis.io/commands/keys

# 查詢符合的鍵值
redis-cli keys *
redis-cli keys prefix:*
redis-cli keys k??

## https://redis.io/commands/del

# 刪除指定鍵值
DEL key

## https://redis.io/commands/expire

# 設定指定鍵值的生命周期(秒)
redis-cli expire key 10

## https://redis.io/commands/select

# 在命令列模式切換資料庫(預設為 0)
redis-cli -n 1 info

# 在互動模式切換資料庫(預設為 0)
redis-cli
> select 1

## https://redis.io/commands/slaveof

# 配置 redis2 為 redis1 的複寫節點
redis-cli -h redis2 slaveof redis1 6379

# 取消 redis2 的複寫配置
redis-cli -h redis2 slaveof no one

# 改配置 redis1 為 redis2 的複寫節點
redis-cli -h redis1 -p 6380 slaveof redis2 6379

## https://redis.io/commands/bgsave

# 強制在背景將記憶體資料寫入檔案 (預設為 /var/lib/redis/dump.rdb)
redis-cli bgsave

## https://redis.io/commands/flushdb

# 清空資料庫(預設為 0)
redis-cli flushdb

# 清空指定的資料庫
redis-cli -n 1 flushdb

## https://redis.io/commands/flushall

# 清空所有資料庫
redis-cli flushall

## https://redis.io/commands/monitor

# 監控伺服器執行的所有指令
redis-cli monitor
```

## [複寫機制](https://redis.io/topics/replication)

## [Sentinel](http://redis.io/topics/sentinel) 監控及 Failover 機制

redis-server 2.8 之後內建的 Sentinel 服務用於管理多個 redis 節點,預設監聽於 `TCP:26379`, 必須開放給其它 Sentinel 節點

假設只兩個 redis 節點,設定範例如下:

`/etc/redis/sentinel.conf`

```bash
# port <監聽連接埠>
port 26379

# sentinel monitor <master-name> <ip> <port> <quorum>
# 定義主節點名稱及位置
# <quorum>: 仲裁為失效節點所需的最少票數,應該小於備援節點個數
sentinel monitor mymaster 127.0.0.1 6379 2

# sentinel down-after-milliseconds <master-name> <milliseconds>
# 判斷為主觀失效(SDOWN)的毫秒數
sentinel down-after-milliseconds mymaster 5000

# sentinel failover-timeout <master-name> <milliseconds>
# 判斷需自動故障遷移的毫秒數
sentinel failover-timeout mymaster 900000

# sentinel parallel-syncs <master-name> <numslaves>
# 新主節點產生時，需至少與多少個備援節點完成同步
sentinel parallel-syncs mymaster 1
```

## [叢集架構](https://redis.io/topics/cluster-tutorial)

## 疑難排解

### [線上跨主機同步資料庫](https://github.com/stickermule/rump)

可以透過 [rump](https://github.com/stickermule/rump) 這個開放原始碼的命令列工具來達成

範例如下

```bash
rump -from redis://127.0.0.1:6379/0 -to redis://redis.rxyf8n.ng.0001.apne1.cache.amazonaws.com:6379/0 -ttl
```

### [正確地使用 .NET 用戶端連接 Redis](https://azure.microsoft.com/documentation/articles/cache-dotnet-how-to-use-azure-redis-cache/)

建議利用 NuGet 安裝 [StackExchange.Redis](https://www.nuget.org/packages/StackExchange.Redis/) 套件

使用範例:

```csharp
using StackExchange.Redis;

// 利用 Lazy 延遲連線的建立時機,並利用 static 將連線物件設定為 Singleton 物件
private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
{
    // abortConnect=false 表示當連線發生問題時不會丟出錯誤,而是會自動嘗試重新建立連線
    return ConnectionMultiplexer.Connect("localhost,abortConnect=false");
});

public static ConnectionMultiplexer Connection
{
    get
    {
        return lazyConnection.Value;
    }
}

// 取得資料庫參考,可以指定資料庫編號,例如: Connection.GetDatabase(1);
var db = Connection.GetDatabase();

// 寫入數字並取回
db.StringSet("key1", 25);
var value1 = (int)db.StringGet("key1");

// 寫入字串,指定逾時時間並取回
db.StringSet("key2", "value2", TimeSpan.FromMinutes(90));
var value2 = db.StringGet("key2");

// 非同步寫入並取回
await db.StringSetAsync("key3","value3");
var value3 = await db.StringGetAsync("key3");

// 利用 Newtonsoft.Json 序列化物件存入 Redis 並取回
db.StringSet("key4", JsonConvert.SerializeObject(new Employee(25, "Clayton Gragg")));
var value4 = JsonConvert.DeserializeObject<Employee>(db.StringGet("e25"));
```

## Azure Redis

### Commands

```bash
# List
SCAN 0 COUNT 1000 MATCH *

# Delete
DEL key1 key2 key3...

# 取得 hash 中，所有的值
HGETALL Key

# 批次刪除特定資料
# powershell
redis-cli del @(redis-cli --scan --pattern "prefix:*") 
# linux
redis-cli --scan --pattern prefix:* | xargs -L redis-cli -c del
```
