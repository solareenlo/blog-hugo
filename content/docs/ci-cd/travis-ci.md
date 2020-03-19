# Travis CIとは
- GitHub上のソフトウェアのビルドやテストを行う, オンラインで分散型の継続的インテグレーション(CI)サービスのこと.
- https://travis-ci.org が無料でオープンソースのプロジェクト用, https://travis-ci.com が有料でプライベートリポジトリ用の利用ができる.

## Travis CiからAWS Elastic Beanstalkへ
- [AWS Elastic Beanstalk Deployment](https://docs.travis-ci.com/user/deployment/elasticbeanstalk/)

## Dokcerのテストがpassedにならない時の対処法
Travis CIでDockerのテストを以下のように設定するとpassedにならないことがある.
```yaml
script:
  docker run solareenlo/react-test npm run test -- --coverage
```
そんな時は以下のようにテストを設定する.
```yaml
script:
  docker run -e CI=true solareenlo/react-test npm run test -- --watchAll=falseb
```

## 環境変数が設定できない時
Travis CIの環境変数を設定する項目では特殊文字(; & ( ) | ^ < > ? * [ ] $ ` ' " \ ! { } 改行 タブ スペース)がそのままの入力ではエスケープされないので, シングルクォーテーション('')で囲む必要がある.
```bash
# 例
=rrTDKhZYgT2Zm4&TF+D^pyp84Uf9[Tw7xZ9Parhx[$A83QCGRb.NKxAnqUd%7(t
```
を
```bash
# 例
'=rrTDKhZYgT2Zm4&TF+D^pyp84Uf9[Tw7xZ9Parhx[$A83QCGRb.NKxAnqUd%7(t'
```
と入力する.

## 秘密情報を暗号化
DockerでTravis CLIを使って送る.
```bash
docker run -it -v $(pwd):/app ruby:2.3 sh
gem install travis --no-document
gem install travis
travis login
> Username:
solareenlo
> Password for solareenlo:
******************************
> Two-factor authentication code for solareenlo:
999999
> Successfully logged in as solareenlo!
# そして$(pwd)に秘匿情報が載ったファイル(ここではservice-account.json)を置いて,
# 以下でTravis CIのsolareenlo/multi-k8s-gke用にservice-account.jsonを暗号化するし,
# Travis CIの当該プロジェクトに登録もする.
travis encrypt-file service-account.json -r solareenlo/multi-k8s-gke
```
これで, `service-account.json`ファイルから暗号化された`service-account.json.enc`ファイルが作成される.
