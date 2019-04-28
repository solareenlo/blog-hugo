# [Docker](https://www.docker.com)とは
コンテナ型の仮想環境を作成, 配布, 実行するためのプラットフォームのこと.
プロセスを簡単にコンテナ化(isolate)し, 簡単かつ素早く開発・移動・実行できるプラットフォームのこと.

- Dockerコンテナは実行に必要な全てをパッケージして簡単に動かせる
- Dockerイメージは複数のイメージ・レイヤとメタ情報の積み重なりからできている.
- コンテナのプロセスはデフォルトでisolate(隔離・分離)された状態
 - Dockerについての良い読み物1→[2018年なぜ私達はコンテナ/Dockerを使うのか](http://iga-ninja.hatenablog.com/entry/2018/06/28/091412).
 - Dockerについての良い読み物2→[標準化が進むコンテナとサーバーレス！ 「提供したい価値」から見極める活用の勘所とは【デブサミ2018 福岡】](https://codezine.jp/article/detail/11098)
 - Dockerについての良いスライド→[Docker Compose 徹底解説](https://www.slideshare.net/zembutsu/docker-compose-guidebook)

## Dockerのメリット/デメリット
### メリット
- ゲストOSはホストのKernelを直接使うためオーバーヘッドが小さくて高速
- ゲストOSがそれぞれにKernelを持たないため, Memory消費量やDisk消費量を節約できる
- 必要とする資源が少ないため, 多くのゲストOSを立ち上げることが可能
- Kernelを新しく起動する必要がないため, ゲストOSの起動が速い
- コンテナのイメージ(雛形)からコンテナ(実体)を作るため, 同一構成のOSを簡単に複数作れる
- テストが通ったイメージは本番環境でもすぐに使える(開発とデプロイのサイクルが速い)
 - **Reference:** [Dockerの利点](http://docker.yuichi.com/about/strength/index.html)

### デメリット
- 提供できるホストの種類が少ない(ホストOSのKernelにLinux使ってたらゲストOSのKernelにWindows Serverは使えない)
- 完全仮想化に比べて, 管理者が学ぶべきことが多い
 - **Reference:** [Dockerの欠点](http://docker.yuichi.com/about/strength/index.html)

## 勉強できるサイト
- [入門 Docker](https://github.com/y-ohgi/introduction-docker)
- [Docker Mastery: The Complete Toolset From a Docker Captain](https://www.udemy.com/docker-mastery/)

## インストール方法
### Linux編
```bash
curl -fsSL get.docker.com -o get-docker.sh
sh get-docker.sh
```

## 基本的な使い方(v18.09.5)
**Reference:** [Dockerコマンドメモ](https://qiita.com/curseoff/items/a9e64ad01d673abb6866)

## container
### DockerでHello World!
```bash
docker container run --rm alpine /bin/echo 'Hello World!'
> Hello World!
```

### コンテナを起動/停止/再起動
```bash
`docker run` = `docker create` + `docker start`
```
```bash
sudo docker container run nginx // 起動
sudo docker container run -d nginx // デーモンとして起動
sudo docker container stop <CONTAINERID か NAMES> // 停止
sudo docker container start <CONTAINERID か NAMES> // 再起動
```

### 一覧/log表示
```bash
sudo docker container run --publish 80:80 --name webhost nginx // nginxを起動
sudo docker container run --publish 80:80 --detach --name webhost nginx // デーモンとして起動
sudo docker container ls // 起動しているコンテナ一覧表示
> CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                NAMES
> b6e083217819        nginx               "nginx -g 'daemon of…"   7 minutes ago       Up 23 seconds       0.0.0.0:80->80/tcp   webhost
> 9ea87fb8e2e1        mongo               "docker-entrypoint.s…"   10 hours ago        Up 10 hours         27017/tcp            mongo
sudo docker container logs <CONTAINERID か NAMES> // 指定したコンテナのlogを表示
sudo docker container ls -a // コンテナの一覧を表示
sudo docker container ls -aq // コンテナIDだけ表示
```

### コンテナの状態を見る
```bash
sudo docker container top <CONTAINERID or NAMES> // コンテナが実行しているプロセスを表示
sudo docker container inspect <CONTAINERID or NAMES> // コンテナの詳細を表示
sudo docker cotnainer stats <CONTAINERID or NAMES> // コンテナのパフォーマンスを表示
```

### コンテナを動かす
`run`コマンドを使う.
```bash
// mysqlを使う
sudo docker container run -d --name mysql -e MYSQL_RANDOM_ROOT_PASSWORD=ture mysql // mysqlを動かす
```

### 対話型モードでコンテナを動かす
`-it`オプションを使う.

- `-t`: ホスト側の入力をコンテナの標準出力をつなげる
- `-i`: キーボードから入力した文字はコンテナ内のプロセスに送られる

```bash
sudo docker container run -it ubuntu /bin/bash // ubuntuを動かす
apt install curl -y
curl https://google.lcom
> <HTML><HEAD><meta http-equiv="content-type" content="text/html;charset=utf-8">
> <TITLE>301 Moved</TITLE></HEAD><BODY>
> <H1>301 Moved</H1>
> The document has moved
> <A HREF="https://www.google.com/">here</A>.
> </BODY></HTML>
```

### Alpineを動かす
Alpineは軽量なLinux distributionの1つ.
```bash
sudo docker container run -it alpine /bin/ash
apk add curl
curl https://google.com
```

### コンテナの停止とともにコンテナ削除
`--rm`オプションを使う.
```bash
sudo docker container run --rm -it alpine /bin/ash
```

### コンテナを一度に停止する
`-q`オプションで起動しているコンテナIDを取得し, バッククォートで引数として渡す.
```bash
sudo docker container stop `sudo docker container ls -q`
```

### コンテナを一度に削除する
`-aq`オプションで存在するコンテナIDを取得し, バッククォートで引数として渡す.
```bash
sudo docker container rm `sudo docker container ls -aq`
```

### 起動しているコンテナに入る
`exec`コマンドを使う.
```bash
sudo docker container exec -it コンテナ名
```

### portを指定してnginxを起動する
```bash
sudo docker container run -p 80:80 --name webhost nginx // 80:80の順番はHOST:CONTAINERの順番になっている.
sudo docker container port webhost // portの繋がりがどうなっているか確認
> 80/tcp -> 0.0.0.0:80
sudo docker container inspect --format '{{ .NetworkSettings.IPAddress }}' webhost // webhostのIPアドレスを表示
> 172.17.0.2
```

## network
### ネットワークを新規に作って接続して削除する
```bash
sudo docker network ls
> NETWORK ID          NAME                DRIVER              SCOPE
> bc9b26e5c4d9        bridge              bridge              local
> ba4557f6a2be        host                host                local
> 5ac181d8a48b        none                null                local
```
- **bridge:** Linux bridgeで仮想インタフェースを作成し, そのインタフェースに対してvethでDockerコンテナと接続する方式で, Dockerホストが属するネットワークとは異なる, 仮想bridge上のネットワークにコンテナを作成し, NAT形式で外部のノードと通信する形式.
- **host:** Dockerホストと同じネットワークにスタックするドライバで, Dockerホストマシンと同じネットワークインタフェース, IPアドレスを持つようになる.
- **null:** ネットワーク接続を必要としないコンテナを作成する場合に使用する.
 - **Reference:** [Docker network 概論](https://qiita.com/TsutomuNakamura/items/ed046ee21caca4a2ffd9)
- bridge・hostいずれもインターネット経由でコンテナへのアクセスが可能.
- bridgeはホストの任意のポートをコンテナのポートにマップすることが出来る.
- hostはコンテナでexposeされたポートをホストでも利用する. その為一つのホストで同じポートを使うコンテナは利用できない.
 - **Reference:** [Docker の bridge と host ネットワークについて勉強する](https://qiita.com/toshihirock/items/f5b9f7799ec8bf8c328e)

```bash
sudo docker network create my_app_net // 新規ネットワークを作成
sudo docker container run -d --name new_nginx --network my_app_net nginx // この新規に作成したネットワークでコンテナを走らせる
sudo docker network inspect my_app_net // 新規ネットワークにどのコンテナが接続されているかを確認する
sudo docker network connect bridge new_nginx // bridgeネットワークにnew_nginxコンテナを接続する
sudo docker network disconnect bridge new_nginx // 先ほど繋いだコンテナの接続を切る
```
- コンテナを起動するデフォルトの設定では外部へのポートは閉じられてる.
- なので, -pオプションを使って手動で外部とのポートと繋ぐ必要がある.

### コンテナからコンテナに問い合わせる
同じネットワーク内にあるコンテナからコンテナへ問い合わせる.
```bash
docker container run -d --name my_nginx --network my_app_net nginx:alpine
sudo docker container exec -it my_nginx ping new_nginx
> 64 bytes from 172.18.0.2: seq=0 ttl=64 time=0.060 ms
> 64 bytes from 172.18.0.2: seq=1 ttl=64 time=0.069 ms
> 64 bytes from 172.18.0.2: seq=2 ttl=64 time=0.093 ms
```

### 2個コンテナを立ててDNSラウンドロビンを使ってみる
elasticsearchを2つコンテナとして立ててる.  
**Reference:** [はじめての Elasticsearch](https://qiita.com/nskydiving/items/1c2dc4e0b9c98d164329)
```bash
sudo docker network create dude // 新規にdudeネットワークを作成
sudo docker container run -d --net dude --net-alias search elasticsearch:2
sudo docker container run -d --net dude --net-alias search elasticsearch:2
sudo docker container ls
> CONTAINER ID        IMAGE               COMMAND                  CREATED              STATUS              PORTS                NAMES
> 46d60f6737ca        elasticsearch:2     "/docker-entrypoint.…"   About a minute ago   Up About a minute   9200/tcp, 9300/tcp   vigilant_stonebraker
> 4bea0ab17400        elasticsearch:2     "/docker-entrypoint.…"   About a minute ago   Up About a minute   9200/tcp, 9300/tcp   determined_archimedes
sudo docker container run --rm --net dude alpine nslookup search // dudeネットワークでalpineコンテナを立ち上げて, nslookupでsearchのipアドレスを問い合わせてる
> nslookup: can't resolve '(null)': Name does not resolve
> Name:      search
> Address 1: 172.19.0.3 search.dude
> Address 2: 172.19.0.2 search.dude
sudo docker container run --rm --net dude centos curl -s search:9200 | jq // dudeネットワークにcentosコンテナ立ち上げて, curlでsearchのエイリアスの効いたIPアドレスの9200番ポートに問い合わせてる
> {
>   "name": "Miek",
>   "cluster_name": "elasticsearch",
>   "cluster_uuid": "tf7h0SLXS8Gx6UMxgC7hRg",
>   "version": {
>     "number": "2.4.6",
>     "build_hash": "5376dca9f70f3abef96a77f4bb22720ace8240fd",
>     "build_timestamp": "2017-07-18T12:17:44Z",
>     "build_snapshot": false,
>     "lucene_version": "5.5.4"
>   },
>   "tagline": "You Know, for Search"
> }
sudo docker container run --rm --net dude centos curl -s search:9200 | jq // dudeネットワークにcentosコンテナ立ち上げて, curlでsearchのエイリアスの効いたIPアドレスの9200番ポートに問い合わせてる
> { // 2回目は1回目とは違うコンテナに問い合わせてる
>   "name": "Mondo",
>   "cluster_name": "elasticsearch",
>   "cluster_uuid": "OoTXK8QASzWdxxEF64iEHA",
>   "version": {
>     "number": "2.4.6",
>     "build_hash": "5376dca9f70f3abef96a77f4bb22720ace8240fd",
>     "build_timestamp": "2017-07-18T12:17:44Z",
>     "build_snapshot": false,
>     "lucene_version": "5.5.4"
>   },
>   "tagline": "You Know, for Search"
> }
```

## image
Dockerのイメージは2種類のレイヤ構造になっている.

- 読み込み専用レイヤのイメージレイヤ
- 読み書き可能なコンテナレイヤ

Dockerはユニオンファイルシステム(複数のファイルシステム上のディレクトリやファイルをレイヤとしてスタックし, それらを仮想的に一つのファイルシステムとして扱う技術)を使い, コンテナを軽量化している.
**Reference:** [知らないと損する Docker イメージのレイヤ構造とは](https://www.techscore.com/blog/2018/12/10/docker-images-and-layers/)

### docker imageをDocker Hubからpullする
```bash
sudo docker pull mysql
```

### 一括してイメージを最新へ更新する
**Reference:** [一括してDockerイメージを最新にアップデートしたい](https://qiita.com/suin/items/5d65320ee9fb9596249f)
```bash
sudo docker images | cut -d ' ' -f1 | tail -n +2 | sort | uniq | egrep -v '^(<none>|ubuntu)$' | xargs -P0 -L1 sudo docker pull
```

### タグ付けしてpull
タグけせずにpullを行うとデフォルトでタグ名がlatestになる.  
タグ付けするときはイメージ名の後ろに「:」を付けて, その後ろにタグ名を指定する.
```bash
sudo docker image pull nginx:mainline
```

### 既にあるイメージに新たにタグ付けする
タグ付けするとDocker Hubにタグ別にどんどんイメージをpushできる.  
タグを変えるだけだと同じイメージが使われる.
```bash
sudo docker image tag nginx solareenlo/nginx
```

### Docker Hubにログインする
そのままローカル環境からDocker Hubにログインするとパスワードが平文のまま保存されるのでpassなどを使って暗号化して保存する.
保存方法は[こらら]({{< relref "/posts/docker-pass.md" >}}).

```bash
sudo docker login
```

### Docker Hubに自分でタグ付けしたイメージをpushする
```bash
sudo docker push solareenlo/nginx
```

## Dockerfile
### Dockerfileを書いてみる
- `FROM`: 元となるイメージを指定する. Docker Hubから引っ張ってくる.
- `ENV`: 環境変数を設定できる.
- `RUN`: 既存イメージ上の新しいレイヤで, あらゆるコマンドを実行し, その結果をコミットする命令.
  一度`RUN`を抜けるとホームディレクトリからの実行となる. ひたすらコマンドで書く.
- `EXPOSE`: コンテナがリッスンするポートを設定できる.
  これを設定した後コンテナ起動時に`-p`を使ってポートの公開範囲を指定する必要がある.
- `CMD`: 構築時には何もしないが、イメージで実行するコマンドを指定する.
  一度だけ実行する. 複数`CMD`がある場合は一番後ろのものが実行される.
  <コマンド>をシェルを使わずに実行したい場合, コマンドをJSON配列で記述し, 実行可能なフルパスで指定する必要がある.
  配列の形式が`CMD`では望ましい形式.
  あらゆる追加パラメータは個々の配列の文字列として指定する必要がある.

```dockerfile
# こんな感じにDockerfileに書いてみる
FROM debian:stretch-slim
ENV NGINX_VERSION 1.13.6-1~stretch
ENV NJS_VERSION   1.13.6.0.1.14-1~stretch
RUN apt-get update \
  && apt-get install --no-install-recommends --no-install-suggests -y gnupg1 \
  && \
  NGINX_GPGKEY=573BFD6B3D8FBC641079A6ABABF5BD827BD9BF62; \
  found=''; \
  for server in \
    ha.pool.sks-keyservers.net \
    hkp://keyserver.ubuntu.com:80 \
    hkp://p80.pool.sks-keyservers.net:80 \
    pgp.mit.edu \
  ; do \
    echo "Fetching GPG key $NGINX_GPGKEY from $server"; \
    apt-key adv --keyserver "$server" --keyserver-options timeout=10 --recv-keys "$NGINX_GPGKEY" && found=yes && break; \
  done; \
  test -z "$found" && echo >&2 "error: failed to fetch GPG key $NGINX_GPGKEY" && exit 1; \
  apt-get remove --purge -y gnupg1 && apt-get -y --purge autoremove && rm -rf /var/lib/apt/lists/* \
  && echo "deb http://nginx.org/packages/mainline/debian/ stretch nginx" >> /etc/apt/sources.list \
  && apt-get update \
  && apt-get install --no-install-recommends --no-install-suggests -y \
            nginx=${NGINX_VERSION} \
            nginx-module-xslt=${NGINX_VERSION} \
            nginx-module-geoip=${NGINX_VERSION} \
            nginx-module-image-filter=${NGINX_VERSION} \
            nginx-module-njs=${NJS_VERSION} \
            gettext-base \
  && rm -rf /var/lib/apt/lists/*
RUN ln -sf /dev/stdout /var/log/nginx/access.log \
  && ln -sf /dev/stderr /var/log/nginx/error.log
EXPOSE 80 443
CMD ["nginx", "-g", "daemon off;"]
```

### DockerFileからイメージを作成する
```bash
// Dockerfileのあるディレクトリで以下を実行して新たにイメージを作成する.
sudo docker image build -t customnginx .

// そして, コンテナを作成し, `localhost:80`にアクセスする.
sudo docker container run --rm -p 80:80 --name nginx2 customnginx
```

### ローカルにあるファイルをコピーしてイメージを作成する
以下のように`index.html`を作って,
```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">

  <title>2番目のDockerfile動いてる!</title>

</head>

<body>
  <h1>ビルド時にカスタムファイルをイメージにコピーして, コンテナを正常に実行できています.</h1>
</body>
</html>
```
以下のようにDokcerfileを作ってみる.
```dockerfile
# Dockerfileはこんな感じ
FROM nginx:latest
WORKDIR /usr/share/nginx/html
COPY index.html index.html
```
そして, イメージを作ってコンテナを走らせて, `localhost:80`にアクセスすると, 先ほど作った`index.html`が表示される.
```bash
sudo docker image build -t nginx-with-html .
sudo docker container run -p 80:80 --rm nginx-with-html
```

### COPYとADDの違い
- `COPY`はローカルのファイルをイメージのレイヤーにコピーできる.
- `ADD`はリモートのファイルもイメージのレイヤーにコピーできる.

### WORKDIRで作業ディレクトリを指定する
- Dockerfileで`RUN`, `CMD`, `ENTRYPOINT`, `COPY`, `ADD`命令実行時の作業ディレクトリを指定できる.
- 複数回の利用が可能で, 複数回利用すると相対パスになる.

```dockerfile
WORKDIR /a
WORKDIR b
WORKDIR c
RUN pwd
```
とすると, `/a/b/c pwd`となる.

- WORKDIRはENVを使った環境変数も利用できる.

```dockerfile
ENV DIRPATH /path
WORKDIR $DIRPATH/to
RUN pwd
```
とすると, `/path/to pwd`となる.

### Node.js用のDockerfileをスクラッチから書いてみる
```dockerfile
# alpine用のnodeイメージインストールする
FROM node:8.16.0-alpine
# コンテナの3000番ポートを開く
EXPOSE 3000
# tiniをインストールして, 作業ディレクトリを作成する
RUN apk add --update tini \
  && mkdir -p /usr/src/app
# 作業ディレクトリに移動する
WORKDIR /usr/src/app
# package.jsonを作業ディレクトリにコピーする
COPY package.json package.json
# 作業ディレクトリにnpmパッケージをインストールする
RUN npm install
RUN npm cache clean --force
# コンテナのイメージレイヤーに必要なファイル全てをコピーする
COPY . .
# tiniでnodeを起動する
CMD ["/sbin/tini", "--", "node", "./bin/www"]
```
こんな感んじでDokcerfileを作成する.  
ソースコード例は[こちら](https://github.com/solareenlo/udemy-docker-mastery/tree/master/dockerfile-assignment-1).  
そして, イメージをビルドして, コンテナを起動して, localhost:3000にアクセスする.
```bash
docker image build -t nodetest .
docker container run --rm -p 80:3000 nodetest
```

### 任意のDockerfileを指定する
`-f`を使う.
```bash
docker build -f Dockerfile.dev .
```

## volume
- volumeはDockerが管理するデータ領域(コンテナが消えても残るデータ領域)をコンテナ上にマウントする機能のこと.
- volumeのデータは`/var/lib/docker/volumes`配下にある.
- 設定する方法は`-v`と`--mount`の2種類ある.
- コンテナ内のvolumeにマウントしたらよいpathはhub.docker.comにあるイメージのページのタグのページから探すと良い.
- 新規ユーザーは`--mount`を使おう.
- `--mount`が`key=value`形式で分かりやすいから.

### -vを使ってvolumeをmysqlコンテナにマウントする
下記ではDocker内のvolumeという永続的にデータが保管されるところに`mysql-db`というディレクトリが作成されて, それがコンテナ内の`/var/lib/mysql`に紐付くということ.
```bash
docker container run -d --name mysql -e MYSQL_ALLOW_EMPTY_PASSWORD=True -v mysql-db:/var/lib/mysql mysql
```

### --mountを使ってvolumeをmysqlコンテナにマウントする
`--mount`は, `key=value`形式でvolumeの指定をするフラグ.
```bash
docker container run -d --name mysql2 -e MYSQL_ALLOW_EMPTY_PASSWORD=True --mount type=volume,src=mysql-db2,dst=/var/lib/mysql mysql
```

### 2つのコンテナに同じvolumeをマウントする
```bash
docker container run -d --name psql --mount type=volume,src=psql,dst=/var/lib/postgresql/data postgres:alpine
docker container logs -f psql
> psqlのlogが標準出力される
docker container stop psql
docker container run -d --name psql2 --mount type=volume,src=psql,dst=/var/lib/postgresql/data postgres:alpine
docker container logs -f psql2
> psql2のlogが出力される
docker volume ls
> DRIVER              VOLUME NAME
> local               psql
```
## bind mount
bind mountはDocker外の任意のファイル・ディレクトリをコンテナ内にマウントする機能のこと.

### -vを使ってDocker外のデータをnginxコンテナにマウントする
```bash
git clone git@github.com:solareenlo/udemy-docker-mastery.git
ch udemy-docker-mastery/dockerfile-sample-2
docker container run -d --name nginx -p 80:80 -v $(pwd):/usr/share/nginx/html nginx
```

### --mountを使ってDocker外のデータをnginxコンテナにマウントする
```bash
git clone git@github.com:solareenlo/udemy-docker-mastery.git
ch udemy-docker-mastery/dockerfile-sample-2
docker container run -d --name nginx2 -p 8080:80 --mount type=bind,src=$(pwd),dst=/usr/share/nginx/html nginx
```
そして, nginxの中に入って, Docker外のデータとコンテナがきちんとマウントされてるかどうかを確かめてみる.
```bash
docker container exec -it nginx /bin/bash
cd /usr/share/nginx/html
ls -la
> drwxr-xr-x 2 1000 1000 4096 Apr 24 16:17 .
> drwxr-xr-x 3 root root 4096 Apr 16 21:20 ..
> -rw-r--r-- 1 1000 1000  415 Apr 19 17:34 Dockerfile
> -rw-r--r-- 1 1000 1000  285 Apr 23 22:54 index.html
# 別のターミナルを開いて, コンテナをマウントしたディレクトリでtest.mdを作成すると,
ls -la
> drwxr-xr-x 2 1000 1000 4096 Apr 25 01:20 .
> drwxr-xr-x 3 root root 4096 Apr 16 21:20 ..
> -rw-r--r-- 1 1000 1000  415 Apr 19 17:34 Dockerfile
> -rw-r--r-- 1 1000 1000  285 Apr 23 22:54 index.html
> -rw-rw-r-- 1 1000 1000    0 Apr 25 01:20 test.md
# test.mdがきちんと増えてる.
```

### ローカルのJekyllをコンテナのJekllサーバで動かす
```bash
git clone git@github.com:solareenlo/udemy-docker-mastery.git
cd udemy-docker-mastery/bindmount-sample-1
docker run -p 80:4000 --mount type=bind,src=$(pwd),dst=/site bretfisher/jekyll-serve
> コンテナでJekyllサーバが起動する
> Configuration file: /site/_config.yml
>        Deprecation: The 'gems' configuration option has been renamed to 'plugins'. Please update your config file accordingly.
>             Source: /site
>        Destination: /site/_site
>  Incremental build: disabled. Enable with --incremental
>       Generating...
>        Jekyll Feed: Generating feed for posts
>                     done in 0.261 seconds.
>  Auto-regeneration: enabled for '/site'
>     Server address: http://0.0.0.0:4000/
>   Server running... press ctrl-c to stop.
```
ブラウザで`localhost:80`にアクセスするとJekyllで作ったサイトが立ち上がってる.

