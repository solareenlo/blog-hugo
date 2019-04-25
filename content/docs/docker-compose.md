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

## docker-compose.yml
services:, networks:, volumes:を定義する.

## nginxとhttpdを動かす
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
