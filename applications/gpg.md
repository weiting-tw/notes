# GPG

## Install

### macOS

```text
brew install gpg2
```

### Other

Download Link: [Gnupg](https://www.gnupg.org/download/index.html>)

## 產生金鑰

```text
> gpg --full-generate-key

請選擇你要使用的金鑰種類:
   (1) RSA 和 RSA (預設)
   (2) DSA 和 Elgamal
   (3) DSA (僅能用於簽署)
   (4) RSA (僅能用於簽署)
你要選哪一個? 1

# 建議 4096 位元
RSA 金鑰的長度可能介於 1024 位元和 4096 位元之間.
你想要用多大的金鑰尺寸? (2048) 4096
你所要求的金鑰尺寸是 4096 位元

請指定這把金鑰的有效期限是多久.
         0 = 金鑰不會過期
      <n>  = 金鑰在 n 天後會到期
      <n>w = 金鑰在 n 週後會到期
      <n>m = 金鑰在 n 月後會到期
      <n>y = 金鑰在 n 年後會到期
金鑰的有效期限是多久? (0) 0
金鑰完全不會過期

# 確認
GnuPG 需要建構使用者 ID 以識別你的金鑰.

真實姓名: <Name>
電子郵件地址: <Email>
註釋:
你選擇了這個使用者 ID:
    "<Name> <<Email>>"

變更姓名(N), 註釋(C), 電子郵件地址(E)或確定(O)/退出(Q)? O
```

產生完成

```text
sec   rsa4096 2020-01-09 [SC]
      A7935E31448D6D55E6A90F31DF7D85F066F4FC1D
uid           [ultimate] Wilber_chen (GSS) <wilber_chen@gss.com.tw>
ssb   rsa4096 2020-01-09 [E]
```

GPG Keys 的 Fingerprint \(指紋\) `A7935E31448D6D55E6A90F31DF7D85F066F4FC1D`

## 提交到 Github/GitLab

取得 GPG key ID: `DF7D85F066F4FC1D`

GPG Fingerprint: `A7935E31448D6D55E6A90F31DF7D85F066F4FC1D`

```text
> gpg --list-secret-keys --keyid-format LONG

/Users/weiting/.gnupg/pubring.kbx
---------------------------------
sec   rsa4096/DF7D85F066F4FC1D 2020-01-09 [SC]
      A7935E31448D6D55E6A90F31DF7D85F066F4FC1D
uid                 [ultimate] Wilber_chen (GSS) <wilber_chen@gss.com.tw>
ssb   rsa4096/C13513872FF9040D 2020-01-09 [E]
```

### 產生public key

```text
> gpg --armor --export <Fingerprint>

-----BEGIN PGP PUBLIC KEY BLOCK-----

mQINBF4W0NYBEADMFKfGy7ln1q13bfkF9dcSIV3gCQFuvTkqknt1Vrz/fyYlAbUX
l+iEv7WEq8S9VBTj6AtRsspodZtXyRWuW3oreqWeSMt/9e9JYdioQVaRD2lw867+
98x7Pz3EWPlKmBExaUqK35XWokpsn6ErpakSYohG2WfhaH9QZTT1OKysT4ijI3g
....
-----END PGP PUBLIC KEY BLOCK-----
```

## 提交包含簽署的 Commit

```text
# 僅限此倉庫
> git config user.signingkey DF7D85F066F4FC1D

# 全域設定
> git config --global user.signingkey DF7D85F066F4FC1D
```

在 commit 時加上 `-S`

```text
> git commit -S -m "Add xxx.txt"
```

預設加上 -S

```text
# 僅限此倉庫
> git config commit.gpgsign true

# 全域設定
> git config --global commit.gpgsign true
```

## 故障排除

### 如果在下 git commit 後出現這個錯誤：

```text
error: gpg failed to sign the data
fatal: failed to write commit object
```

請嘗試以下兩種解法

#### 解法一

請執行 echo "test" \| gpg --clearsign 檢查看看是 git 的問題還是 gpg 的問題。

如果可以執行正常，請檢查 git 設定是否正確，否之如果出現這個錯誤：

```text
gpg: 簽署時失敗: Inappropriate ioctl for device
gpg: [stdin]: clear-sign failed: Inappropriate ioctl for device
```

請將 export GPG\_TTY=$\(tty\) 加到你的 .bashrc 或 .zshrc，之後重啟終端機或執行重讀指令 . ~/.zshrc or . ~/.bashrc

再執行一次 echo "test" \| gpg --clearsign 看看是成功執行。

#### 解法二

安裝 pinentry-mac

```bash
> brew install pinentry-mac

# 將以下設定新增至 ~/.gnupg/gpg.conf
no-tty

# 以下設定新增至 ~/.gnupg/gpg-agent.conf
pinentry-program /usr/local/bin/pinentry-mac

# 清除背景執行的 gpg-agent
> killall gpg-agent

# 執行看看是成功執行。
> echo "test" | gpg --clearsign
```

### 如果出現 No such file or directory

```bash
error: cannot run gpg: No such file or directory

error: could not run gpg.

fatal: failed to write commit object (128)
```

執行下列

```bash
# mac
> git config --global gpg.program $(which gpg)

# windows
> git config --global gpg.program "C:/Program Files (x86)/GNU/GnuPG/gpg2.exe" 

# GitHub got back to me and said that some users also need to use:
> echo "no-tty" >> ~/.gnupg/gpg.conf
```

## 找出 `gpg-agent.conf` 設定檔的所在路徑

```bash
gpg --version
```

{% tabs %}
{% tab title="Windows" %}
```bash
C:\>gpg --version
gpg (GnuPG) 2.2.23
libgcrypt 1.8.6
Copyright (C) 2020 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <https://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Home: C:/Users/user/AppData/Roaming/gnupg
Supported algorithms:
Pubkey: RSA, ELG, DSA, ECDH, ECDSA, EDDSA
Cipher: IDEA, 3DES, CAST5, BLOWFISH, AES, AES192, AES256, TWOFISH,
        CAMELLIA128, CAMELLIA192, CAMELLIA256
Hash: SHA1, RIPEMD160, SHA256, SHA384, SHA512, SHA224
Compression: Uncompressed, ZIP, ZLIB, BZIP2
```
{% endtab %}

{% tab title="Linux" %}
```bash
$ gpg --version
gpg (GnuPG) 2.2.20
libgcrypt 1.8.6
Copyright (C) 2020 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <https://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Home: /home/will/.gnupg
Supported algorithms:
Pubkey: RSA, ELG, DSA, ECDH, ECDSA, EDDSA
Cipher: IDEA, 3DES, CAST5, BLOWFISH, AES, AES192, AES256, TWOFISH,
        CAMELLIA128, CAMELLIA192, CAMELLIA256
Hash: SHA1, RIPEMD160, SHA256, SHA384, SHA512, SHA224
Compression: Uncompressed, ZIP, ZLIB, BZIP2
```
{% endtab %}
{% endtabs %}

 從上述版本資訊可以看到一個 `Home:` 欄位，這裡指出 `gpg-agent.conf` 設定檔的所在路徑！

###  常見的 `gpg-agent.conf` 設定檔路徑，若沒有出現可自行增加

{% tabs %}
{% tab title="Windows" %}
```bash
%AppData%\gnupg\gpg-agent.conf
```
{% endtab %}

{% tab title="Linux/macOS" %}
```
~/.gnupg/gpg-agent.conf
```
{% endtab %}
{% endtabs %}

### 調整 `gpg-agent` 快取時間

 延長快取時間的 `gpg-agent.conf` 設定內容範例 `單位：秒 (Seconds)`

```bash
# 設定 24 小時內自動密碼快取失效，如果期間有存取過快取，會重置快取時間，
# 自動延長到期時間到當下加上 24 小時。
# 預設值: 600(10分鐘)
default-cache-ttl 86400
# 設定 7 天，代表快取到第七天之後，無論如何都需要再次輸入密碼。
# 預設值:7200(2小時)
max-cache-ttl 604800
```

 重新載入 `gpg-agent.conf` 設定

```bash
gpgconf --reload gpg-agent
```

 查詢目前 `gpg-agent.conf` 設定

```bash
gpgconf --list-options gpg-agent

Monitor:1:0:Options controlling the diagnostic output:0:0::::
verbose:12:0:verbose:0:0::::
quiet:8:0:be somewhat more quiet:0:0::::
Configuration:1:0:Options controlling the configuration:0:0::::
disable-scdaemon:8:1:do not use the SCdaemon:0:0::::
enable-ssh-support:0:0:enable ssh support:0:0::::
ssh-fingerprint-digest:24:2:use ALGO to show ssh fingerprints:1:1:ALGO:"md5::
enable-putty-support:0:0:enable putty support:0:0::::
Debug:1:1:Options useful for debugging:0:0::::
debug-level:26:1:set the debugging level to LEVEL:1:1:LEVEL:"none::
log-file:8:1:write server mode logs to FILE:32:1:FILE:::
Security:1:0:Options controlling the security:0:0::::
default-cache-ttl:24:0:expire cached PINs after N seconds:3:3:N:600::86400
default-cache-ttl-ssh:24:1:expire SSH keys after N seconds:3:3:N:1800::
max-cache-ttl:24:2:set maximum PIN cache lifetime to N seconds:3:3:N:7200::604800
max-cache-ttl-ssh:24:2:set maximum SSH key lifetime to N seconds:3:3:N:7200::
ignore-cache-for-signing:8:0:do not use the PIN cache when signing:0:0::::
allow-emacs-pinentry:8:1:allow passphrase to be prompted through Emacs:0:0::::
grab:8:2::0:0::::
no-allow-external-cache:8:0:disallow the use of an external password cache:0:0::::
no-allow-mark-trusted:8:1:disallow clients to mark keys as "trusted":0:0::::
no-allow-loopback-pinentry:8:2:disallow caller to override the pinentry:0:0::::
Passphrase policy:1:1:Options enforcing a passphrase policy:0:0::::
enforce-passphrase-constraints:8:2:do not allow bypassing the passphrase policy:0:0::::
min-passphrase-len:24:1:set minimal required length for new passphrases to N:3:3:N:8::
min-passphrase-nonalpha:24:2:require at least N non-alpha characters for a new passphrase:3:3:N:1::
check-passphrase-pattern:24:2:check new passphrases against pattern in FILE:32:1:FILE:::
max-passphrase-days:24:2:expire the passphrase after N days:3:3:N:0::
enable-passphrase-history:8:2:do not allow the reuse of old passphrases:0:0::::
pinentry-timeout:24:1:set the Pinentry timeout to N seconds:3:3:N:0::
```

#### 關閉 `gpg-agent` 背景程式

```bash
gpgconf --kill gpg-agent
```

