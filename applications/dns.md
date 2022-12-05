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

## [MxToolbox](https://mxtoolbox.com/SuperTool.aspx)

All of your MX record, DNS, blacklist and SMTP diagnostics in one integrated tool.  Input a domain name or IP Address or Host Name. Links in the results will guide you to other relevant tools and information.  And you'll have a chronological history of your results.
