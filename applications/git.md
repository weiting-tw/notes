# Git

## push when using two-factor authenticationg

啟用二階段驗證時，會無法push到server上

```sh
> git push
Username for 'https://github.com': a26007565
Password for 'https://a26007565@github.com':
remote: Invalid username or password.
fatal: Authentication failed for 'https://github.com/a26007565/notes.git/'
```

解決方法:

1. git config –global credential.helper store
2. Settings => Personal access tokens
3. Generate new token => token的範圍選repo就夠了 => click Generate Token
4. 再次輸入：git push，輸入帳號，再密碼的地方輸入token就行了

**備註** 訊息會存在
> ~/.git-credentials
