# Dockerのコマンド使用例

```bash
# run = create + start
docker run busybox echo hi there
> hi there

# コンテナ作ってスタートさせる
docker create --name my-busybox busybox echo hi there
docker start -a my-busybox
> hi there

# 一定期間後に終了
docker stop CONTAINER_NAME
# 直ぐに終了
docker kill CONTAINER_NAME

# Image,Container,Volumeの数や容量を表示
docker system df
# 止まってるContainer, 使われてないVolume, 使われてないNetwork, 使われてないImageを削除
docker system prune

# 現在動いているredisコンテナにアクセスする
docker run --name myredis -d redis
docker exec -it myredis redis-cli
127.0.0.1:6379> set my-number 5
> OK
127.0.0.1:6379> get my-number
> "5"
# CTL-Cで終了

# Shell Scriptモードで起動
docker run -it busybox sh
/ # ping google.com
> 64 bytes from 172.217.31.174: seq=0 ttl=51 time=3.399 ms
> 64 bytes from 172.217.31.174: seq=1 ttl=51 time=3.672 ms
^C
/ # echo hello
hello
/ #
# こんな感じでbusyboxはシェルスクリプトが使える
 ```
