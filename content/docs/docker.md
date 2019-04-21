# [Docker](https://www.docker.com)とは
コンテナ型の仮想環境を作成, 配布, 実行するためのプラットフォーム.  
Dockerについての良い読み物→[2018年なぜ私達はコンテナ/Dockerを使うのか](http://iga-ninja.hatenablog.com/entry/2018/06/28/091412).

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

## インストール方法
### Linux編
```bash
curl -fsSL get.docker.com -o get-docker.sh
sh get-docker.sh
```

## 基本的な使い方
**Reference:** [Dockerコマンドメモ](https://qiita.com/curseoff/items/a9e64ad01d673abb6866)

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

### コンテナを起動/停止/再起動
```bash
sudo docker container run nginx // 起動
sudo docker container run -d nginx // デーモンとして起動
sudo docker container stop <CONTAINERID か NAMES> // 停止
sudo docker container start <CONTAINERID か NAMES> // 再起動
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
```bash
sudo docker container run -it ubuntu /bin/bash // ubuntuを動かす
apt install curl
curl https://google.lcom
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
