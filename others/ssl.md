# SSL

## 安全性測試

* [ssl labs](https://www.ssllabs.com/ssltest/)
* [cloudflare ssl-test](https://www.cloudflare.com/lp/ssl-test/)

## LetsEncrypt

### create a credentials in `~/.secrets/certbot/cloudflare.ini`

```text
# Cloudflare API credentials used by Certbot
dns_cloudflare_email = <your mail>
dns_cloudflare_api_key = <your apiKey>
```

### install plugins

> certbot -i certbot-dns-cloudflare

or

> pip3 install certbot-dns-cloudflare

### chmod

> chmod 700 ~/.secrets/certbot/cloudflare.ini

### run command

> certbot certonly --dns-cloudflare --dns-cloudflare-credentials ~/.secrets/certbot/cloudflare.ini -d dns.weiting.me

### renew

> certbot renew --quiet

### set crontab

```bash
# 每兩個月的1號執行
0 0 1 */2 * certbot renew --quiet
```

### docker Run

```bash
docker run -it \
-v ~/letsencrypt/:/etc/letsencrypt \
-v ~/.secrets/certbot/cloudflare.ini:/tmp/certbot/cloudflare.ini \
certbot/dns-cloudflare  certonly \
--dns-cloudflare \
--dns-cloudflare-credentials /tmp/certbot/cloudflare.ini \
-d <your domain> \
-m <your mail>
```

## pem to pfx

> openssl pkcs12 -in cert.pem -inkey privkey.pem -export -out server.pfx

