# [Docker Compose](https://github.com/docker/compose)とは
複数のコンテナで構成されるアプリケーションを定義と実行するためのツールのこと.
Dockerとは切り離されてる.

- Composeはアプリケーションのサービスをファイルで定義する.
- Dockerコマンドと高い親和性があるため, 学習コストが比較的低い.
- Swarmモードにサービスをデプロイできるオーケストレーション機能もある.
 - **Reference:** [Docker Compose 徹底解説](https://www.slideshare.net/zembutsu/docker-compose-guidebook)

## 活用場面
- 利用者視点
 - `docker-compose.yml`があれば, すぐに何でも実行できる.
- 開発者視点
 - 環境の再構築が簡単
 - バージョン違いの環境を作りやすい
 - 1つのマシン上に, 複数の環境を立ち上げられやすい

適切に書かれたYAMLファイルさえあれば, 誰でも簡単に環境構築もアプリケーション実行もできるのが強み.

## インストール
- https://docs.docker.com/compose/install/

## docker-compose.ymlの書き方
- **services:**(使うイメージ), **networks:**(使うネットワーク), **volumes:**(使うボリューム)を定義する.
- docker-compose.ymlの書き方 -> [Docker Compose - docker-compose.yml リファレンス](https://qiita.com/zembutsu/items/9e9d80e05e36e882caaa)

## 起動と停止
```bash
# 起動
docker-compose up
# ビルドして起動
docker-compose up --build
# バックグラウンドで起動
docker-compose up -d
# 停止
docker-compose down
# ボリュームも削除しつつ停止
docker-compose down -v
```

## nginxとhttpd
```bash
git clone git@github.com:solareenlo/udemy-docker-mastery.git
cd udemy-docker-mastery/compose-sample-2
docker-compose up -d # デーモンでコンテナ群を起動
docker-compose ps # 動いてるコンテナ一覧が見られる
>           Name                   Command          State         Ports
> ----------------------------------------------------------------------------
> compose-sample-2_proxy_1   nginx -g daemon off;   Up      0.0.0.0:80->80/tcp
> compose-sample-2_web_1     httpd-foreground       Up      80/tcp
docker-compose down # コンテナ群を停止する
```

## DrupalとPostgreSQL
`docker-compose.yml`に以下を記述する.
```yaml
# Drupal with PostgreSQL
#
# Access via "http://localhost:8081"
#   (or "http://$(docker-machine ip):8081" if using docker-machine)
#
# During initial Drupal setup,
# Database type: PostgreSQL
# Database name: postgres
# Database username: postgres
# Database password: passwd
# ADVANCED OPTIONS; Database host: postgres
# ADVANCED OPTIONS; Database port: 5432

version: '3.7'

services:
  drupal:
    image: drupal:8-apache
    ports:
      - 8081:80
    volumes:
      - drupal-modules:/var/www/html/modules
      - drupal-profiles:/var/www/html/profiles
      - drupal-themes:/var/www/html/themes
      - drupal-sites:/var/www/html/sites
    restart: always

  postgres:
    image: postgres:10
    environment:
      POSTGRES_PASSWORD: passwd
    restart: always

volumes:
  drupal-modules:
  drupal-profiles:
  drupal-themes:
  drupal-sites:
```
そして, 起動する.
```bash
docker-compose up -d # DrupalとPostgreSQLを起動
docker-compose ps # 起動してるか確認
>              Name                            Command               State          Ports
> -----------------------------------------------------------------------------------------------
> compose-assignment-1_drupal_1     docker-php-entrypoint apac ...   Up      0.0.0.0:8081->80/tcp
> compose-assignment-1_postgres_1   docker-entrypoint.sh postgres    Up      5432/tcp
```
任意のブラウザで`localhost:8081`を開くとDrupalが立ち上がってる.  
設定値は以下.
```bash
Database type: PostgreSQL
Database name: postgres
Database username: postgres
Database password: passwd
ADVANCED OPTIONS; Database host: postgres
ADVANCED OPTIONS; Database port: 5432
```
そして, 停止する.
```bash
docker-compose down -v # volumeも一緒に削除.
```

## Node.jsとRedix
**コード例:** [visits-docker-nodejs](https://github.com/solareenlo/visits-docker-nodejs)

## 複数のイメージを作る
Dockerfileからカスタムnginxイメージを作成しつつ, カスタムnginxコンテナとhttpdコンテナを動かしている.
```bash
git clone git@github.com:solareenlo/udemy-docker-mastery.git
cd udemy-docker-mastery/compose-sample-3
```
`docker-compose.yml`の中身は以下.
```yaml
version: '2'
services:
  proxy:
    build:
      context: .
      dockerfile: nginx.Dockerfile
    image: custom-nginx
    ports:
      - '80:80'
  web:
    image: httpd
    volumes:
      - ./html:/usr/local/apache2/htdocs/
```
カスタムnginxの`nginx.Dockerfile`の中身は以下.
```dockerfile
FROM nginx:1.13
COPY nginx.conf /etc/nginx/conf.d/default.conf
```
`nginx.conf`の中身は以下.
```conf
server {
	listen 80;
	location / {
		proxy_pass         http://web;
		proxy_redirect     off;
		proxy_set_header   Host $host;
		proxy_set_header   X-Real-IP $remote_addr;
		proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;
		proxy_set_header   X-Forwarded-Host $server_name;
	}
}
```
webコンテナで使っているhtmlディレクトリの中身は`https://startbootstrap.com/template-overviews/agency/`です.  
そして, 起動する.
```bash
docker-compose up -d
```
`localhost:80`を開くとサイトが見られる.  
そして, 停止する.
```bash
docker-compose down --rmi local
```

## dockerfileでカスタムイメージを作成しつつdocker-composeで動かす
`Dockerfile`に下記を記入する.
```dockerfile
FROM drupal:8-apache
RUN apt-get update && apt-get install -y git \
    && rm -rf /var/lib/apt/lists/*
WORKDIR /var/www/html/themes
RUN git clone --branch 8.x-3.x --single-branch --depth 1 https://git.drupal.org/project/bootstrap.git \
    && chown -R www-data:www-data bootstrap
WORKDIR /var/www/html
```
`docker-compose.yml`に下記を記入する.
```yaml
# Drupal with PostgreSQL
#
# Access via "http://localhost:8081"
#   (or "http://$(docker-machine ip):8081" if using docker-machine)
#
# During initial Drupal setup,
# Database type: PostgreSQL
# Database name: postgres
# Database username: postgres
# Database password: passwd
# ADVANCED OPTIONS; Database host: postgres
# ADVANCED OPTIONS; Database port: 5432

version: '3.7'

services:
  drupal:
    image: custom-drupal
    build: .
    ports:
      - 8081:80
    volumes:
      - drupal-modules:/var/www/html/modules
      - drupal-profiles:/var/www/html/profiles
      - drupal-themes:/var/www/html/themes
      - drupal-sites:/var/www/html/sites
    restart: always

  postgres:
    image: postgres:10
    environment:
      POSTGRES_PASSWORD: passwd
    volumes:
      - drupal-data:/var/lib/postgresql/data
    restart: always

volumes:
  drupal-modules:
  drupal-profiles:
  drupal-themes:
  drupal-sites:
  drupal-data:
```
そして, 下記を実行する.
```bash
dokcer-compose up -d
```
すると, 任意のブラウザで`localhost:8081`を開くとDrupalが走っていて,  
設定値は以下を入力して,
```bash
Database type: PostgreSQL
Database name: postgres
Database username: postgres
Database password: passwd
ADVANCED OPTIONS; Database host: postgres
ADVANCED OPTIONS; Database port: 5432
```
ページのテーマをDockerfileで設定してダウンロードしてきたBootstrapに変えてみる.  
停止するには,
```bash
docker-compose down
```

## テストを行う
以下のように`docker-compose.yml`に`tests`項目を追加する.
```yaml
version: '3'
services:
  web:
    build:
      context: .
      dockerfile: Dockerfile.dev
    ports:
      - "3001:3000"
    volumes:
      - /app/node_modules
      - .:/app
  tests:
    build:
      context: .
      dockerfile: Dockerfile.dev
    volumes:
      - /app/node_modules
      - .:/app
    command: ["npm", "run", "test"]
```
そして, 以下でビルドし, 実行する.
```bash
docker-compose up --build
```
テスト用のコードを変更すると, 標準出力で結果が出てくる.

**コード例:** [frontend-docker-react](https://github.com/solareenlo/frontend-docker-react)

## 環境変数
以下のように`variabeleName=value`と書けばコンテナの中で有効な環境変数になる.
```bash
environment:
  - REDIS_HOST=redis
  - REDIS_PORT=6378
```

以下のように`variableName`とだけ書けばホストから環境変数を取ってきてコンテナの中で有効な環境変数になる.
```bash
environment:
  - REDIS_HOST
```

- **Referece:**
 - [Docker-Compose の変数定義について](https://qiita.com/kimullaa/items/f556431b8103e686f356)

## volumes
以下のようにパスを`volumes`に指定するとボリュームとしてマウントしてくれる.
```yaml
volumes
  - /path/to/filename
```

以下のように`ホストのパス:コンテナのパス`と書くとホストをマウントしてくれる.
```yaml
volumes
  - host/path:./path/to/test
```
- **References:**
 - [Docker、ボリューム(Volume)について真面目に調べた](https://qiita.com/gounx2/items/23b0dc8b8b95cc629f32)
 - [Docker Compose - docker-compose.yml リファレンス](https://qiita.com/zembutsu/items/9e9d80e05e36e882caaa)

## AWSで動かした例
- [solareenlo/complex-docker](https://github.com/solareenlo/complex-docker)
