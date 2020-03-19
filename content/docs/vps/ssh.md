---
title: SSH
---

# SSH(Secure Shell)とは
- 暗号や認証の技術を利用して, 安全にリモートコンピュータと通信するためのプロトコルのこと.
- VPSに接続するときは必須.

## SSHの流れ
**Reference:** [SSH通信って、結局何してるの？](https://qiita.com/naoki_mochizuki/items/aea14a65a4a513e4816f)
```bash
[local]1.通信用の秘密鍵・公開鍵の作成する.
[local]2.サーバーに公開鍵を渡す.

[server]A.サーバーにユーザーを登録する(sudo権限).
[server]B.サーバーに登録されているユーザーと, 渡された公開鍵を紐づける.
※この状態で初めて作成したユーザーによるサーバーへのログインが可能となる.

[local]3.作成したユーザーでログインする.

[server]C.ログイン時に乱数を生成する.
[server]D.Cで生成した乱数から, ハッシュ値を生成する.
※このハッシュ値は認証で使うのでサーバー側で保持しておく.
[server]E.受け取った公開鍵 + Cで生成した乱数を用いて暗号を生成する.
[server]F.Dで作成した暗号をローカルに送る.

[local]4.1で作成した秘密鍵を用いて, 送られてきた暗号を解読し乱数を復元する.
[local]5.乱数からハッシュ値を計算し, そのハッシュ値をサーバーに送る.

[server]G.送られてきたハッシュ値と、Dで生成したハッシュ値を比較する.
一致していれば認証成功. 以後全ての通信は暗号化される.
```


## scp
`scp`とはSSHを用いてファイルを秘匿化してコピーする技術のこと.
```bash
# sshのconfigファイルを用いて通信する方法
scp file_name.txt -F ~/.ssh/config iota3:~/github
scp <渡したいファイルの名前> -F <コンフィグファイルの名前> <渡し先のマシン名>:<渡し先のディレクトリ>
```

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
