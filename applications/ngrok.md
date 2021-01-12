# Ngrok

## 讓 ngrok 瀏覽只能接受 localhost 連接的網站

重寫標頭 你可以從 [Rewriting the Host header](https://ngrok.com/docs#host-header) 看到相關的說明與指令。基本上有兩種可能的用法：

指定 `-host-header=rewrite` 參數，並在指定 Ports 時直接給與你要的域名，例如：`localhost`

```bash
ngrok http -host-header=rewrite localhost:5780
```

明確指定 Host 標頭在 -host-header= 參數中

```bash
ngrok http -host-header=localhost 5780
```

