# [Kubernetes](https://github.com/kubernetes)とは
Dockerコンテナのクラスタ管理を始めとしたオーケストレーションを行うサービスのこと.
ホスト間の連携やデプロイについても総括的に管理できる(ここがDocker Composeと違うところ).  
**Reference:** [Docker Compose利用者から見た Kubernetes 開発環境構築入門](https://speakerdeck.com/kkoudev/introduction-to-kubernetes-for-docker-compose-user)

## 他と違うところ
① 様々なOSSと組み合わせることにより, 柔軟に機能拡張なところ.

 - コンテナ運用を更に効率化 / 高速化
  - 詳細なメトリック監視と可視化・・・Prometheus + Grafana
  - コンテナのログの転送収集・・・Fluentd / Fluent Bit
  - ネットワークトラフィックの制御・・・ Istio + Envoy
- Kubernetesの適応領域の拡大
 - 機械学習プラットフォーム・・・Kubeflow
 - 分散ストレージ・・・Rook
 - 分散型データベース・・・Vitess

② 本格的な宣言的オペレーションとInfrastructure as Codeを実現可能

③ クラスター上にデプロイするシステムの構成をコード(マニフェストファイル)によって定義できる

- 運用オペレーションはコードの変更によって実施し, 作業を簡素化する.
- 複数Kubernetesクラスターでの相互運用を実現する

**Reference:** [Kubernetesの基礎](https://crash.academy/ng/video/731/2060)

## Docker Composeとの違い
Docker Composeは動作させるコンテナを意識するだけでほとんど良かったが, Kubernetesではそれに加えて動作させるホスト(Node)やコンテナのグループ化(Pod), その複製(ReplicaSet)と公開(Service, Ingress)といったインフラレベルで意識していたことも全て設定ファイルの1つとして管理できる.  
**Reference:** [Docker Compose利用者から見た Kubernetes 開発環境構築入門](https://speakerdeck.com/kkoudev/introduction-to-kubernetes-for-docker-compose-user)

|項目|Docker Compose|→|Kubernetes|
|---|---|---|---|
|Image|各エントリーは, イメージを構築するためにオプションでdocker-composeを取得できる.|→|すべてのイメージがすでに構築されているとする.|
|Container|各エントリが作成したいコンテナを表す.|→|作成したいオブジェクトごとに1つの設定ファイル|
|Network|各エントリがネットワーク要件を定義する.|→|手動ですべてのネットワークを設定する必要がある.|

## 用語集
|用語|説明|
|---|---|
|Node|コンテナが動作するサーバのこと.<br>- 全Nodeを管理するMaster(Master Node)と,<br>- 各リソースを動かすNode(Worker Node)に分かれる.|
|Pod|関連したコンテナの集まりを1つにしたもの.|
|ReplicaSet|対象Podのクラスタ全体における複製数を定義する.<br>そうすることで, Podのセルフヒーリングができる.|
|Deployment|ReplicaSetの作成・維持の管理を行う.|
|Service|Podへのアクセス経路を提供する.<br>OSI参照モデルのL4層まで扱える.<br>- クラスタ内部のみで利用できるService(ClusterIP)や,<br>- クラスタ外部からアクセス可能なService(NodePort)などを作成することができる.|
|Ingress|Serviceの上位リソース.<br>OSI参照モデルのL7層レベルまで扱える.|
|ConfigMap|環境変数のような設定値, また設定ファイル情報そのものを管理する.<br>Key-Value形式.|
|Secret|パスワードのような秘匿情報を扱う際に利用する.|
|PersistentVolume|ボリューム領域を定義する.<br>EBSやNFSのような外部ストレージも定義できる.|
|PersistentVolumeClaim|利用するボリューム領域の要求を定義する.<br>PersistentVolumeとPodを紐付けるために利用する.|

## Macのローカル環境で動かす
1. https://brew.sh からbrewをインストールする.
- `brew install kubectl`をターミナルで実行する.
  - `kubectl`はKubernetesのmasterを動かすもの.
- https://www.virtualbox.org からMac用のVirtualBoxをインストールする.
  - VirtualBoxはコンテナ群を動かす環境.
- `brew cask install minikube`をターミナルで実行する.
  - `minikube`コンテナをVM上でコンテナを動かすもの.
- `minidube start`をターミナルで実行する.


