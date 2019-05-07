---
title: "ローカル環境のDocker Hubのパスワードをpassで管理する方法"
date: 2019-04-23T08:00:00+09:00
author: "solareenlo"
tags: [
    "Docker",
    "Docker Hub",
    "pass"
]
categories: [
  "Docker Hub"
]
menu:
  main:
    parent: tutorials
---

> Ubuntu用
>
> **なぜDocker Hubのパスワードをpassを使って管理するか?**
> : ローカルからDocker Hubにログインすると, その時のパスワードを平文のまま保存されるから.

1. [ここ](https://github.com/docker/docker-credential-helpers/releases)から`docker-credential-pass`の最新バージョンをダウンロードする.
2. `tar -zxvf docker-credential-pass.tar.gz` (解凍する.)
3. `mv docker-credential-pass /usr/local/bin` (PATHが通ってるところにファイルを移動する.)
4. `sudo apt-get install gpg pass` ([gpg](https://www.gnupg.org/index.html)と[pass](https://www.passwordstore.org)をインストール.)
5. `gpg --generate-key`で新しい秘密鍵と公開鍵の組を作る.
6. `gpg --list-keys`で出てきた, pubの16進数の40文字(大文字のA-F, 0-9の文字列)をコピーする.
7. `pass init AAAAAAAAAABBBBBBBBBBCCCCCCCCCCDDDDDDDDD`でpassを初期化する.
8. `pass insert docker-credential-helpers/docker-pass-initialized-check`でとりあえずのディレクトリを作成する.
9. `docker-credential-pass list`で`{}`と返ってくる.
10. `~/.docker/config.json`に`{"credsStore": "pass"}`と書き込む.
11. `docker login`でDockerにログインする.

> うまくいかないときは権限を`chmod`を使って変更してみる.

```bash
curl -O https://github.com/docker/docker-credential-helpers/releases/download/v0.6.0/docker-credential-pass-v0.6.0-amd64.tar.gz
tar -xvf docker-credential-pass.tar.gz
mv docker-credential-pass /usr/local/bin
apt-get install gpg pass
gpg --generate-key
gpg --list-keys
> pub   rsa2048 2019-04-22 [SC] [有効期限: 2021-04-21]
>       D7D35B60A7FA571541959AF3C4821C32793D5F5A // ここの公開鍵をコピーする
> uid           [  究極  ] solareenlo <test@example.com>
> sub   rsa2048 2019-04-22 [E] [有効期限: 2021-04-21]
pass init D7D35B60A7FA571541959AF3C4821C32793D5F5A // 上記でコピーした公開鍵をペーストする
pass insert docker-credential-helpers/docker-pass-initialized-check // ここで入力するパスワードは初期化されるので何でも良い.
pass show docker-credential-helpers/docker-pass-initialized-check // 先ほど入力したパスワードが表示される
docker-credential-pass list
vim ~/.docker/config.json
// {
//   "credsStore": "pass"
// }
/* と~/.docker/config.jsonに書き込む. */
docker login
```

- **References:**
 - [Cannot login to Docker account](https://stackoverflow.com/questions/50151833/cannot-login-to-docker-account/50569553)
 - [Document how to initialize docker-credentials-pass #102](https://github.com/docker/docker-credential-helpers/issues/102)
 - [Credentials store](https://docs.docker.com/engine/reference/commandline/login/#credentials-store)
