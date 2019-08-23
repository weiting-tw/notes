# DNS

## Clean DND cache

`Windows` 環境下

```powershell
ipconfig /flushdns
```

`Mac OSX` 環境下

```sh
dscacheutil -flushcache
```

`Linux` 環境下

```sh
/etc/init.d/nscd restart
```
