# Tools

## 取得已開啟的 port 清單

```sh
docker run --rm -it instrumentisto/nmap  -T4 -sT -p T:1-65535 ${server}
```
