# [Docker](https://www.docker.com)とは
コンテナ型の仮想環境を作成, 配布, 実行するためのプラットフォーム.

## インストール方法
### Linux編
```bash
curl -fsSL get.docker.com -o get-docker.sh
sh get-docker.sh
```

## 簡単な使い方
**Reference:** [Dockerコマンドメモ](https://qiita.com/curseoff/items/a9e64ad01d673abb6866)

```
// nginxをコンテナとして起動
sudo docker container run --publish 80:80 --name webhost nginx

// 上記をデーモンとして動かす
sudo docker container run --publish 80:80 --detach --name webhost nginx

// コンテナ一覧を表示
sudo docker container ls
> CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                NAMES
> b6e083217819        nginx               "nginx -g 'daemon of…"   7 minutes ago       Up 23 seconds       0.0.0.0:80->80/tcp   webhost
> 9ea87fb8e2e1        mongo               "docker-entrypoint.s…"   10 hours ago        Up 10 hours         27017/tcp            mongo

// コンテナが実行しているプロセスを表示
sudo docker container top <CONTAINERID か NAMES>

// 起動しているコンテナを停止
sudo docker container stop <CONTAINERID か NAMES>

// 停止しているコンテナを再起動
sudo docker container start <CONTAINERID か NAMES>

// webhostのlogを見る
sudo docker container logs <CONTAINERID か NAMES>

// インストールしているコンテナ一覧を見る
sudo docker container ls -a
```

### コンテナの状態を見る
```
// コンテナが実行しているプロセスを表示
sudo docker container top <CONTAINERID or NAMES>

// コンテナの詳細を表示
sudo docker container inspect <CONTAINERID or NAMES>

// コンテナのパフォーマンスを表示
sudo docker cotnainer stats <CONTAINERID or NAMES>
```

### mysqlを動かす
```
sudo docker container run -d --name mysql -e MYSQL_RANDOM_ROOT_PASSWORD=ture mysql
```

### Ubuntuを動かす
```
sudo docker container run -it ubuntu /bin/bash
apt install curl
curl https://google.lcom
```

### Alpineを動かす
Alpineは軽量なLinux distributionの1つ.
```
sudo docker container run -it alpine /bin/ash
apk add curl
curl https://google.com
```

### 起動しているコンテナに入る
```
sudo docker container exec -it コンテナ名
```

### portを指定してnginxを起動する
```
sudo docker container run -p 80:80 --name webhost nginx
// 80:80の順番はHOST:CONTAINERの順番になっている.

// portの繋がりがどうなっているか確認
sudo docker container port webhost
> 80/tcp -> 0.0.0.0:80

// webhostのIPアドレスを表示
sudo docker container inspect --format '{{ .NetworkSettings.IPAddress }}' webhost
> 172.17.0.2
```

### ネットワークを新規に作って接続して削除する
```
sudo docker network ls
> NETWORK ID          NAME                DRIVER              SCOPE
> bc9b26e5c4d9        bridge              bridge              local
> ba4557f6a2be        host                host                local
> 5ac181d8a48b        none                null                local
```
- bridgeはLinux bridgeで仮想インタフェースを作成し, そのインタフェースに対してvethでDockerコンテナと接続する方式で, Dockerホストが属するネットワークとは異なる, 仮想bridge上のネットワークにコンテナを作成し, NAT形式で外部のノードと通信する形式.
- hostはDockerホストと同じネットワークにスタックするドライバで, Dockerホストマシンと同じネットワークインタフェース, IPアドレスを持つようになる.
- nullはネットワーク接続を必要としないコンテナを作成する場合に使用する.
 - **Reference:** [Docker network 概論](https://qiita.com/TsutomuNakamura/items/ed046ee21caca4a2ffd9)
- bridge・hostいずれもインターネット経由でコンテナへのアクセスが可能.
- bridgeはホストの任意のポートをコンテナのポートにマップすることが出来る.
- hostはコンテナでexposeされたポートをホストでも利用する. その為一つのホストで同じポートを使うコンテナは利用できない.
 - **Reference:** [Docker の bridge と host ネットワークについて勉強する](https://qiita.com/toshihirock/items/f5b9f7799ec8bf8c328e)

```
// 新規ネットワークを作成
sudo docker network create my_app_net

// この新規に作成したネットワークでコンテナを走らせる
sudo docker container run -d --name new_nginx --network my_app_net nginx

// 新規ネットワークにどのコンテナが接続されているかを確認する
sudo docker network inspect my_app_net

// bridgeネットワークにnew_nginxコンテナを接続する
sudo docker network connect bridge new_nginx

// 先ほど繋いだコンテナの接続を切る
sudo docker network disconnect bridge new_nginx
```
- コンテナを起動するデフォルトの設定では外部へのポートは閉じられてる.
- なので, -pオプションを使って手動で外部とのポートと繋ぐ必要がある.

