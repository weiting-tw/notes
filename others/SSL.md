# SSL

## 安全性測試

* [ssl labs](https://www.ssllabs.com/ssltest/)
* [cloudflare ssl-test](https://www.cloudflare.com/lp/ssl-test/)

## LetsEncrypt

### create a credentials in `~/.secrets/certbot/cloudflare.ini`

```ini
# Cloudflare API credentials used by Certbot
dns_cloudflare_email = <your mail>
dns_cloudflare_api_key = <your apiKey>
```

### install plugins

> certbot -i certbot-dns-cloudflare

### chmod

> chmod 700 ~/.secrets/certbot/cloudflare.ini

### run command

> certbot certonly --dns-cloudflare --dns-cloudflare-credentials ~/.secrets/certbot/cloudflare.ini -d dns.weiting.me

### renew

> certbot renew --quiet

### set crontab

```sh
# 每兩個月的1號執行
0 0 1 */2 * certbot renew --quiet
```
