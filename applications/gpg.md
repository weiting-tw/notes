# GNU Privacy Guard(GPG)

## Install

### macOS

```shell
brew install gpg2
```

### Other

Download Link: [Gnupg](<https://www.gnupg.org/download/index.html>)

## 產生金鑰

```shell
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

```shell
sec   rsa4096 2020-01-09 [SC]
      A7935E31448D6D55E6A90F31DF7D85F066F4FC1D
uid           [ultimate] Wilber_chen (GSS) <wilber_chen@gss.com.tw>
ssb   rsa4096 2020-01-09 [E]
```

GPG Keys 的 Fingerprint (指紋) `A7935E31448D6D55E6A90F31DF7D85F066F4FC1D`

## 提交到 Github/GitLab

取得 GPG key ID: `DF7D85F066F4FC1D`

GPG Fingerprint: `A7935E31448D6D55E6A90F31DF7D85F066F4FC1D`

```shell
> gpg --list-secret-keys --keyid-format LONG

/Users/weiting/.gnupg/pubring.kbx
---------------------------------
sec   rsa4096/DF7D85F066F4FC1D 2020-01-09 [SC]
      A7935E31448D6D55E6A90F31DF7D85F066F4FC1D
uid                 [ultimate] Wilber_chen (GSS) <wilber_chen@gss.com.tw>
ssb   rsa4096/C13513872FF9040D 2020-01-09 [E]
```

### 產生public key

```shell
> gpg --armor --export <Fingerprint>

-----BEGIN PGP PUBLIC KEY BLOCK-----

mQINBF4W0NYBEADMFKfGy7ln1q13bfkF9dcSIV3gCQFuvTkqknt1Vrz/fyYlAbUX
l+iEv7WEq8S9VBTj6AtRsspodZtXyRWuW3oreqWeSMt/9e9JYdioQVaRD2lw867+
98x7Pz3EWPlKmBExaUqK35XWokpsn6ErpakSYohG2WfhaH9QZTT1OKysT4ijI3g
....
-----END PGP PUBLIC KEY BLOCK-----
```

## 提交包含簽署的 Commit

```shell
# 僅限此倉庫
> git config user.signingkey DF7D85F066F4FC1D

# 全域設定
> git config --global user.signingkey DF7D85F066F4FC1D
```

在 commit 時加上 `-S`

```shell
> git commit -S -m "Add xxx.txt"
```

預設加上 -S

```shell
# 僅限此倉庫
> git config commit.gpgsign true

# 全域設定
> git config --global commit.gpgsign true
```

## 故障排除

### 如果在下 git commit 後出現這個錯誤：

```shell
error: gpg failed to sign the data
fatal: failed to write commit object
```

請嘗試以下兩種解法

#### 解法一

請執行 echo "test" | gpg --clearsign 檢查看看是 git 的問題還是 gpg 的問題。

如果可以執行正常，請檢查 git 設定是否正確，否之如果出現這個錯誤：

```shell
gpg: 簽署時失敗: Inappropriate ioctl for device
gpg: [stdin]: clear-sign failed: Inappropriate ioctl for device
```

請將 export GPG_TTY=$(tty) 加到你的 .bashrc 或 .zshrc，之後重啟終端機或執行重讀指令 . ~/.zshrc or . ~/.bashrc

再執行一次 echo "test" | gpg --clearsign 看看是成功執行。

#### 解法二

安裝 pinentry-mac

```sh
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

```sh
error: cannot run gpg: No such file or directory

error: could not run gpg.

fatal: failed to write commit object (128)
```

執行下列

```sh
> git config --global gpg.program $(which gpg)

# GitHub got back to me and said that some users also need to use:
> echo "no-tty" >> ~/.gnupg/gpg.conf
```
