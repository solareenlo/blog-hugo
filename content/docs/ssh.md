---
title: SSH
---

# SSH(Secure Shell)とは
暗号や認証の技術を利用して, 安全にリモートコンピュータと通信するためのプロトコルのこと.
VPSに接続するときは必須.

## Mac -> Ubuntu を公開鍵暗号方式で接続する
### UbuntuをSSH接続可にする
Ubuntu側で
```bash
sudo apt-get install openssh-server
```
で, Ubuntu側へSSHで接続できるようになる.

### Macで秘密鍵と公開鍵のペアを作成する
まずMacで秘密鍵と公開鍵のペアを作成する.
```bash
ssh-keygen -t rsa
> Generating public/private rsa key pair.
> Enter file in which to save the key (/Users/solareenlo/.ssh/id_rsa):
# ここでエンターを押せば/User/solareenlo/.ssh/id_rsaで保存される.
> Enter passphrase (empty for no passphrase):
# パスフレーズを入力
> Enter same passphrase again:
# 再度パスフレーズを入力
> Your identification has been saved in /Users/solareenlo/.ssh/id_rsa.
> Your public key has been saved in /Users/solareenlo/.ssh/id_rsa.pub.
> The key fingerprint is:
> SHA256:a7CbAlk8ns2XVz8roZ8kkN42EFEgXcE+oftgUcFCQ38 solareenlo@solareenlo-mbp13.local
> The key's randomart image is:
> +---[RSA 2048]----+
> |      .+**=o     |
> |       .o+=      |
> |   .    .=..E    |
> |    +   ooo..    |
> |   + =. S+ o .   |
> |  o o o+*=. . o  |
> |   .  .o++=... o |
> |    .  + .o+...  |
> |     .o    .o.   |
> +----[SHA256]-----+
```
これで秘密鍵は`id_rsa`に, 公開鍵は`id_rsa.pub`に作成された.

### Macの公開鍵をUbuntuへ渡す
`scp`を使って公開鍵をUbuntuへ持っていく.
```bash
cd ~/.ssh
scp id_rsa.pub solareenlo@111.222.333.444:~/.
```
これでユーザー名`solareenlo`, IPアドレスが`111.222.333.444`のUbuntuのホームディレクトリに先ほど作った公開鍵が渡った.

### UbuntuでMacの公開鍵を設置する
```bash
mkdir .ssh
mv id_rsa.pub .ssh
cd ~/.ssh
touch authorized_keys
cat id_rsa.pub >> authorized_keys
rm id_rsa.pub
sudo chmod 700 ~/.ssh
sudo chmod 600 ~/.ssh/authorized_keys
```

### UbuntuでSSH接続での公開鍵認証を有効にする
```bash
sudo cd /etc/ssh
sudo cp -p sshd_config sshd_config.org
sudo vi sshd_config
/* viで以下をコメントアウトかつ変更
PubkeyAuthentication yes # 公開鍵認証の許可
AuthorizedKeysFile .ssh/authorized_keys #公開鍵ファイルのパス
PasswordAuthentication no # パスワード認証禁止
*/

sudo service sshd restart # sshdを再起動
```

### Configファイルを作成する
```bash
cd ~/.ssh
vi config
~~~ viで以下をconfigに書き込む ~~~
Host ubuntu
  HostName 111.222.333.444
  Port 22
  User solareenlo
  IdentityFile ~/.ssh/id_rsa
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
```

### MacからUbuntuへSSH接続する
```bash
ssh ubuntu
# もしくは
ssh -i ~/.ssh/id_rsa solareenlo@111.222.333.444
```

### References
- [RSA公開鍵認証によるssh接続設定について（Macbook -> VPS）](https://qiita.com/m1220/items/9dc3856bbcf985577023)
- [インフラエンジニアじゃなくても押さえておきたいSSHの基礎知識](https://qiita.com/tag1216/items/5d06bad7468f731f590e)
