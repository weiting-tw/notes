# DNS

## Clean DND cache

`Windows` 環境下

```text
ipconfig /flushdns
```

`Mac OSX` 環境下

```bash
dscacheutil -flushcache
```

`Linux` 環境下

```bash
/etc/init.d/nscd restart
```

