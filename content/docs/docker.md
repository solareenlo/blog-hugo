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
