# [Angular](https://github.com/angular/angular)とは
Typescript/JavaScriptやその他の言語を使用してモバイルおよびデスクトップWebアプリケーションを構築するための開発プラットフォーム.

## 用語
|用語|意味|
|---|---|
|モジュール|アプリを構成するコンポーネントを束ねたもの|
|ルートモジュール|アプリが起動する時に呼びされるモジュール|
|コンポーネント|各要素|
|デコレーター|モジュールやクラスなどの要素に対してメタ情報を付与するもの|

### デコレーターのパラメーター
|パラメーター名|意味|
|---|---|
|imports|現在のモジュールで利用する他のモジュール／コンポーネント|
|exports|現在のモジュールで外部に公開するコンポーネントなど|
|declarations|モジュール配下のコンポーネント|
|bootstrap|最初に起動すべき最上位のコンポーネント(＝ルートコンポーネント)|
|templateUrl|描画するhtml|
|template|描画する内容|
|styleUrls|どのcssを使うか|
|styles|直接cssの内容を記述する|
|selector|コンポーネントの適応先を表す(html, css, classなどとして指定できる)|


## DockerでAngular
`Dockerfile`の中身
```dockerfile
FROM node:10.15.3-alpine

RUN apk update \
  && npm install -g @angular/cli@7.3.8 \
  && rm -rf /tmp/* /var/cache/apk/* *.tar.gz ~/.npm \
  && npm cache clear --force \
  && yarn cache clean \
  && sed -i -e "s/bin\/ash/bin\/sh/" /etc/passwd

WORKDIR /app
```
`docker-compose.serve.yml`の中身
```yaml
version: "3"
services:
  angular:
    image: solareenlo/angular-cli
    build: .
    command: ash -c "ng serve --host=0.0.0.0"
    volumes:
      - ./first-app:/app
    ports:
      - "4200:4200"
```
として, `Dockerfile`だけでコンテナを動かすときは,
```bash
# first-appを作成
docker run -it --rm -w /app -v $(pwd):/app solareenlo/angular-cli ng new first-app
# コンテナに入って作業する
docker run -it --rm -w /app -v $(pwd)/first-app:/app solareenlo/angular-cli sh
# コンポーネントを作成
docker run -it --rm -w /app -v $(pwd)/first-app:/app solareenlo/angular-cli ng g component sample-component
# コンテナを立ち上げる
docker run -d -w /app -v $(pwd)/first-app:/app -p 4200:4200 solareenlo/angular-cli ng serve --host 0.0.0.0
```
で, `localhost:4200`を開く.

`Dockerfile`と`docker-compose.serve.yml`でコンテナを動かすときは,
```bash
# first-appを作成
docker run -it --rm -w /app -v $(pwd):/app solareenlo/angular-cli ng new first-app
# コンテナに入って作業する
docker run -it --rm -w /app -v $(pwd)/first-app:/app solareenlo/angular-cli sh
# docker-compose を使ってコンテナを立ち上げる
docker-compose -f docker-compose.serve.yml up -d
# コンテナの中に入って作業する
docker-compose -f docker-compose.serve.yml exec angular sh
# 関連するコンテンを全て止める
docker-compose -f docker-compose.serve.yml stop
# 関連するコンテナを全削除
docker-compose -f docker-compose.serve.yml rm
```
で, `localhost:4200`を開く.

**コード例:** [solareenlo/angular-cli](https://github.com/solareenlo/angular-cli)
