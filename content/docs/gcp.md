# Google Cloud Platform(GCP)とは
Gogleが提供するパブリッククラウドコンピューティングのこと.

## GKE
コンテナ化されたアプリケーションをデプロイするためのマネージド型の本番環境のこと.  
Google Kubernetes Engineの略.

## Cloud Shell
ブラウザでShellが扱える.  
右上のメニューに起動するためのボタンがある.
```bash
# 使うプロジェクトの設定
gcloud config set project <プロジェクトID>
# 使うゾーンの設定
gcloud config set compute/zone asia-east1-c
# 使うコンテナクラスタを設定
gcloud container clusters get-credentials <クラスタ名>
```
