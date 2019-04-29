# Docker その2
## コンテナの基本的な動かし方
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

## Dockerfile
- `Dockerfile` -> `Docker Client` -> `Docker Server` -> `Image`の流れでイメージが作られる.
- `Dockerfile`に書かれてあること1行ずつに対して履歴を取っている.

以下を`Dockerfile`として保存して,
```dockerfile
FROM alpine
RUN apk add --update redis
CMD ["redis-server"]
```
以下でイメージをし,
```bash
# 最後の「.」はDockerfileがある場所を指定している.
docker build .
> 色々出てくる
> Successfully built 8fcebe7331d6
```
以下でコンテナを走らせる.
```bash
docker run 8fcebe7331d6
> 色々出てくる
```

## タグ付け
以下のようにしてタグ付けを行う.
```bash
docker build -t solareenlo/redis:latest .
```

## 1つずつイメージを作っていく
```bash
docker run -it alpine sh
/ # apk add --update redis
> インストールされる
```
別のターミナルを開いて, 以下のように`commit`で新たにイメージを作成する.
```bash
docker ps
> コンテナの情報
docker commit -c 'CMD ["redis-server"]' <コンテナのhash値か名前>
> sha256: イメージのハッシュ値
docker run イメージのハッシュ値
> 新たに作ったイメージからコンテナが走る
```

## Dockerfileの効率的な作り方
1. ベースにするDockerイメージを決める.
- `docker run -it --name custom-container <元となるdocker image> sh`でコンテナに入る.
- コンテナ内で色々インストールしたり, 設定したりする.
- うまくいったらその内容を1行ずつメモする.
- いい頃合いで, `exit`でコンテナからログアウトし,
- `docker commit custom-container solareenlo/custom-image:latest`で新しいDockerイメージを作成する.
- そして, `docker attach custom-image`で再度コンテナに入る.
- 失敗したら`exit`して, 再度`docker run -it solareenlo/custom-image:latest sh`して, コンテナに入る.
- ファイルの取り込みやポートの外部公開が必要ならオプション付きで`docker run`を行う.
- 最後に作業内容を`Dockerfile`に書き込む.
 - **Reference:** [効率的に安全な Dockerfile を作るには](https://qiita.com/pottava/items/452bf80e334bc1fee69a)

## Dockerfileの書き方の工夫
- よく変更するところだけを`COPY`する.
 - Node.jsにおける`index.js`だけとか.
 - `package.json`はあまり変化させないとして.
- 何故ならば, 無駄なビルドを無くすため.

`Dockerfile`の例は以下.
```dockerfile
# ベースとなるイメージ
FROM node:alpine

# 以降での作業ディレクトリを指定する
WORKDIR /usr/app

# npmだけ先にビルドする
# 無駄な再ビルドを防ぐため
COPY ./package.json ./
RUN npm install

# よく変更があるところを再ビルドするために分けた
COPY ./ ./

# デフォルトのコマンド
CMD ["npm", "start"]
```
全体的なコード例 -> [simple-docker-nodejs](https://github.com/solareenlo/simple-docker-nodejs)

## 再起動
|オプション|意味|
|---|---|
|no|再起動しない (デフォルト)|
|on-failure[:max-retries]|プロセスが0以外のステータスで終了した場合,  最大:max_retriesの分だけ再起動を行う.|
|always|明示的にstopがされない限り, 終了ステータスに関係なく常に再起動を行う.|
unless-stopped|最後にdocker daemonが起動していた際にステータスが終了状態だった場合は再起動しない. それ以外はalwaysと同じ.|

## テストを行う
```bash
docker build -f Dockerfile.dev -t frontend .
docker run -d -p 3000:3000 --name frontend frontend
```
**コード例:** [frontend-docker-react](https://github.com/solareenlo/frontend-docker-react)

## React > Nginx と繋げるのをDockerfileだけで行う
`Dokcerfile`に以下のように書き込む.
```dockerfile
FROM node:alpine as builder
WORKDIR '/app'
COPY package.json .
RUN npm install
COPY . .
RUN npm run build

FROM nginx
COPY --from=builder /app/build /usr/share/nginx/html
```
そして,
```bash
docker build -t nginx-react .
```
1つ目の`FROM`でReactのイメージができ, 2つ目の`FROM`でReactの結果を引き継いだNginxのイメージができる.
そして, 以下でコンテナを走らせる.
```bash
docker run -p 3001:80 nginx-react
```
**コード例:** [frontend-docker-react](https://github.com/solareenlo/frontend-docker-react)

## Travis CIに繋げてテストを実行
Travisにアカウントを作成して, GitHubと連携して, リポジトリを登録して, 登録したリポジトリの`.travis.yml`に以下を書き込む.
```yaml
sudo: required
services:
  - docker

before_install:
  - docker build -t solareenlo/frontend-docker-react -f Dockerfile.dev .

script:
  - docker run -e CI=true solareenlo/frontend-docker-react npm run test -- --watchAll=false
```
そして, GitHubにpushすると自動的にtestが行われる.  
**コード例:** [frontend-docker-react](https://github.com/solareenlo/frontend-docker-react)

## Dockerfile(React, Nginx) > GitHub > Travis CI > AWS Elastic Beanstalkとデプロイする
大まかな流れ

1. `npm install -g create-react-app`で`create-react-app`をインストールして簡単なページを作成する.
- 上記のDokerfileを作成する.
こんな感じ.

    ```dockerfile
    FROM node:alpine as builder
    WORKDIR '/app'
    COPY package.json .
    RUN npm install
    COPY . .

    RUN npm run build
    FROM nginx
    EXPOSE 80
    COPY --from=builder /app/build /usr/share/nginx/html
    ```
- GitHub, Travis CI, AWSでそれぞれアカウントを作成する.
- GitHubにコードを1度`push`する.
- AWS`Elastic Beanstalk`でDocker(React, Nginx)用のwebアプリケーションの基盤を作成する.
- `Elastic Beanstalk`で作ったwebアプリケーションのファイルを保存しておくためのストレージをAWSの`S3`を使って作成する.
- `Elastic Beanstalk`にアクセスするためのアクセスキーとシークレットキーをAWSの`IAM`を使って作成・保存する.
- Travis CIでAWSのアクセスキーとシークレットキーを設定をする.
- `.travis.yml`を作成する.
こんな感じ.

      ```yaml
      sudo: required
      services:
        - docker
      before_install:
        - docker build -t solareenlo/frontend-docker-react -f Dockerfile.dev .
      script:
        - docker run -e CI=true solareenlo/frontend-docker-react npm run test -- --watchAll=false
      deploy:
        provider: elasticbeanstalk
        region: "us-east-1"
        app: "frontend-docker-react"
        env: "FrontendDockerReact-env"
        bucket_name: "elasticbeanstalk-us-east-1-998768475306"
        bucket_path: "frontend-docker-react"
        on:
          branch: master
        access_key_id: $AWS_ACCESS_KEY
        secret_access_key:
          secure: "$AWS_SECRET_KEY"
      ```
- ローカルのソースコードをGitHubに`push`する.
  全ての連携が上手くできていれば, GitHubのpushを感知して, Travis CIでビルドとテストが行われ, テストがOKならAWSの`Elastic Beanstalk`に自動でデプロイされ, 規定のURLにReactのサイトが表示される.
- 出来たサイト: http://frontenddockerreact-env.fbdmefkujd.us-east-1.elasticbeanstalk.com

**コード例:** [frontend-docker-react](https://github.com/solareenlo/frontend-docker-react)
