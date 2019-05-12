# [Kubernetes](https://github.com/kubernetes)とは
Dockerコンテナのクラスタ管理を始めとしたオーケストレーションを行うサービスのこと.
ホスト間の連携やデプロイについても総括的に管理できる(ここがDocker Composeと違うところ).  
**Reference:** [Docker Compose利用者から見た Kubernetes 開発環境構築入門](https://speakerdeck.com/kkoudev/introduction-to-kubernetes-for-docker-compose-user)

Kubernetesの大きな特徴の1つに宣言的設定がある.
宣言的設定とは, イミュータブルなインフラを作るための基本的な考え方で, 「システムのあるべき姿」を設定ファイルに宣言する！という考え方.
Kubernetesは設定ファイルに書いたとおりのインフラを維持するように設計されている.
ので, 設定ファイル(yamlファイル)をたくさん書く事になる.

## 他のオーケストレーションと違うところ
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
|Container|各エントリーが作成したいコンテナを表す.|→|作成したいオブジェクト(Kubernetesクラスタ上で機能する構成要素のこと. Pod, Service, Volume, Namespace, Controllerのこと.)ごとに1つの設定ファイル|
|Network|各エントリーがネットワーク要件を定義する.|→|手動ですべてのネットワークを設定する必要がある.|

## 用語集
|用語|説明|
|---|---|
|Node|コンテナが動作するサーバのこと.<br><li>Master(Master Node): 全Nodeを管理する.<li>Node(Woker Node): 各リソースを動かす.|
|Pod|関連したコンテナの集まりを1つにしたもの.|
|ReplicaSet|対象Podのクラスタ全体における生成・管理を行う.<br>PodTemplateと呼ばれるPodのテンプレートをもとに、Podを指定された数(レプリカ数)に調整・管理を行う仕組み.<br>そうすることで, Podのセルフヒーリングを行う.|
|Deployment|ReplicaSetの生成・管理を行う.<br>ローリングアップデートやロールバックといったデプロイ管理の仕組みを提供する.|
|Service|Podへのアクセス経路を提供する.<br>OSI参照モデルのL4層まで扱える.<br>- クラスタ内部のみで利用できるService(ClusterIP)や,<br>- クラスタ外部からアクセス可能なService(NodePort)などを作成することができる.|
|Ingress|Serviceの上位リソース.<br>OSI参照モデルのL7層レベルまで扱える.|
|ConfigMap|環境変数のような設定値, また設定ファイル情報そのものを管理する.<br>Key-Value形式.|
|Secret|パスワードのような秘匿情報を扱う際に利用する.|
|PersistentVolume|ボリューム領域を定義する.<br>EBSやNFSのような外部ストレージも定義できる.|
|PersistentVolumeClaim|利用するボリューム領域の要求を定義する.<br>PersistentVolumeとPodを紐付けるために利用する.|
|Namespace|仮想的なKubernetesクラスタの分離機能.<br><li>**default**: デフォルトのNamespace<li>**kube-system**: Kubernetesクラスタのコンポーネントやaddonが展開されるNamespace<li>**kube-public**: 全ユーザが利用できるConfigMapなどを配置するNamespace|

- **References:**
 - [Kubernetes: Deployment の仕組み](https://qiita.com/tkusumi/items/01cd18c59b742eebdc6a)
 - [Docker Compose利用者から見た Kubernetes 開発環境構築入門](https://speakerdeck.com/kkoudev/introduction-to-kubernetes-for-docker-compose-user)

## yaml内の用語集
|用語|意味|
|---|---|
|apiVersion|リソースで利用するAPIのバージョンを指定.|
|kind|リソースの種別を指定.<br>(Deployment, Serviceとか)|
|metadata|リソースへ付与可能なメタデータ.<br>主に名称やラベルを付与する.|
|spec|リソース固定の設定を指定.|
|data|ConfigMapやSecretなどの設定データを記述するリソースで使用される.|
|label|Kubernetes上のオブジェクト(Podなど)に付けることができるKey/Valueペアの文字列で, オブジェクトに任意のメタ情報のようなものを持たせることができる.|
|selector|Labelの集合をもとにKubernetesオブジェクトをフィルタリングすることができる.|

### apiVersionの調べ方
```bash
kubectl api-resources # リソースとAPIGROUPの対応を調べる
kubectl api-versions # APIGROUPで利用可能なversionを調べる
```
|APIGROUPあるなし||書き方|
|---|---|---|
|APIGROUPがあるとき|→| apiVersion: (APIGROUP)/(APIVERSION)|
|APIGROUPがないとき(= Core groupに属する)|→| apiVersion: v1|
**Reference:** [Kubernetesの apiVersion に何を書けばいいか](https://qiita.com/soymsk/items/69aeaa7945fe1f875822)

### ServiceのType4種類
- **ClusterIp**
 - クラスタ内のIPにServiceを公開する.
 - この値ではServiceはクラスタ内からのみアクセス可能.
 - デフォルトはこれ.
- **NodePort**
 - 各ノードのIP上のServiceを静的ポート(NordPort)に公開する.
 - NodePort ServiceがルーティングするClusterIP Serviceが自動的に作成される.
 - `NodeIp: NordPort`を要求することで, クラスタ外からNordPort Serviceにアクセスできる.
- **LoadBalancer**
 - クラウドプロバイダのロードバランサを使用して外部にServiceを公開する.
 - 外部ロードバランサがルーティングするNordPort ServiceとClusterIP Serviceが自動的に作成される.
- **ExternalName**
 - 値を含むCNAMEレコードを返すことにより, ServiceをexternalNameフィールドのコンテンツ(たとえば, foo.bar.example.com)にマッピングする.
 - この場合, どのような種類のプロキシも設定されない.
 - この型を使うには, バージョン1.7以上のkube-dnsが必要.

**Reference:** [Kubernetesの Service についてまとめてみた](https://qiita.com/kouares/items/94a073baed9dffe86ea0)

### PersistentVolumeClaimのaccessModes3種類
- **PeadWriteOnce**
 - 1つのノードが読み書き可能
- **ReadOnlyMany**
 - 複数のノードが読み込みだけ可能
- **ReadWriteMany**
 - 複数のノードから読み書き可能

### StorageClassの一覧
- [The StorageClass Resource](https://kubernetes.io/docs/concepts/storage/storage-classes/#the-storageclass-resource)

### volumeMountsのsubPath
`subPath`を指定してあげると`volume`をルートディレクトリからマウントしないで, サブディレクトリからマウントする.
`volume`をそんなに使わない時にオススメ.

**Reference:** [永続ストレージのサブディレクトリにマウントする方法](https://qiita.com/MahoTakara/items/fbe5ffb9fd9b9d35ddbe)

## Macのローカル環境で動かす
1. https://brew.sh からbrewをインストールする.
- `brew install kubernetes-cli`をターミナルで実行する.
  - `kubectl`はKubernetesのmasterを動かすもの.
- https://www.virtualbox.org からMac用のVirtualBoxをインストールする.
  - VirtualBoxはコンテナ群を動かす環境.
- `brew cask install minikube`をターミナルで実行する.
  - `minikube`はコンテナをVM上で動かすもの.
- `minikube start`をターミナルで実行する.

- **References:**
 -  https://kubernetes.io/docs/tasks/tools/install-kubectl/
 -  https://www.virtualbox.org/wiki/Downloads
 -  https://kubernetes.io/docs/tasks/tools/install-minikube/

## Ubuntuのローカル環境で動かす
1. `kubectl`をインストールする.
- VirtualBoxをインストールする.
- `minikube`をインストールする.

- **References:**
 -  https://kubernetes.io/docs/tasks/tools/install-kubectl/
 -  https://www.virtualbox.org/wiki/Downloads
 -  https://kubernetes.io/docs/tasks/tools/install-minikube/

### `minikube`の使い方
```bash
minikube status # 動いているか確認
minikube start # minikubeをスタート
minikube ip # ipを確認
minikube dashboard # ダッシュボード出現
minikube docker-env # 環境変数を出力
```

### `kubectl`の使い方
```bash
kubectl apply -f client-pod.yaml # オブジェクトの新規作成・差分反映
kubectl get pods # 動いているpodを確認
kubectl get deployments # 動いているdeploymentを確認
kubectl describe <object type> <object name> # オブジェクトの詳細表示
kubectl delete -f client-pod.yaml # オブジェクトが存在すれば削除
kubectl delete deployment client-deployment # deploymentの削除
kubectl delete pod client-node-port # podの削除
kubectl api-resources # リソースとAPIGROUPの対応を調べる
kubectl api-versions # APIGROUPで利用可能なversionを調べる
kubectl get storageclass # 管理者が提供するストレージのクラスを表示
kubectl get pv # PersistentVolumeの一覧を表示
kubectl get pvc # PersistentVolumeClaimの一覧を表示
# 以下3つはパスワードを設定する時に使用
kubectl create secret generic pgpassword --from-literal PGPASSWORD=******** # パスワードを生成
kubectl create secret generic <secret_name> --from-literal <key=value> # パスワードを生成
kubectl get secret # secret一覧を表示
# 以下2つはGKEでユーザーの権限を与える時に使用
kubectl create serviceaccount --namespace kube-system tiller # tillerというサービスアカウントを作成
kubectl create clusterrolebinding tiller-cluster-rule --clusterrole=cluster-ad
min --serviceaccount=kube-system:tiller
```

### 自分のPCからVMにアクセスしてDockerコンテナを見る方法
```bash
eval $(minikube docker-env)
docker ps
```

## 1度デプロイしたイメージのバージョンアップ方法
Kubernetesでは`latest`タグイメージの運用は適切なロールバックを行うことが難しいので推奨されていない.  
ので, できる限り`Vsersion`, もしくは`digest`でイメージ指定を行うようにする.

以下のようにDocker Hubにバージョンを指定してpushし,
```bash
docker build -t solareenlo/multi-client:v5
docker push solareenlo/multi-client:v5
```
以下のようにコマンドラインからイメージのバージョンアップを行う.
```bash
kubectl set image deployment/client-deployment client=solareenlo/multi-client:v5
```

## イメージのバージョンにgitのshaを使う
以下の様に`.travis.yml`に`$SHA`を設定して,
```yaml
sudo: required
services:
  - docker
env:
  global:
    - SHA=$(git rev-parse HEAD)
```

以下の様に`build`, `push`, `set`を行うと良い.
```bash
docker build -t solareenlo/multi-client:latest -t solareenlo/multi-client:$SHA -f ./client/Dockerfile ./client
docker build -t solareenlo/multi-server:latest -t solareenlo/multi-server:$SHA -f ./server/Dockerfile ./server
docker build -t solareenlo/multi-worker:latest -t solareenlo/multi-worker:$SHA -f ./worker/Dockerfile ./worker

docker push solareenlo/multi-client:latest
docker push solareenlo/multi-server:latest
docker push solareenlo/multi-worker:latest

docker push solareenlo/multi-client:$SHA
docker push solareenlo/multi-server:$SHA
docker push solareenlo/multi-worker:$SHA

kubectl apply -f k8s
kubectl set image solareenlo/server-deployment server=solareenlo/multi-server:$SHA
kubectl set image solareenlo/client-deployment client=solareenlo/multi-client:$SHA
kubectl set image solareenlo/worker-deployment worker=solareenlo/multi-worker:$SHA
```

## ローカルで動かした例
- [solareenlo/complex-k8s-local](https://github.com/solareenlo/complex-k8s-local)

## GKEで動かした例
- [solareenlo/multi-k8s-gke](https://github.com/solareenlo/multi-k8s-gke)

## TLS
- [jetstack/cert-manager](https://github.com/jetstack/cert-manager)

# [Helm](https://github.com/helm/helm)
Kubernetesのパッケージ管理ツールのこと.  
デフォルトで使用可能なChartは`kubernetes/charts`の`stable`ディレクトリで確認可能.

|用語|意味|役割|
|---|---|---|
|helm|船のかじ|Kubernetesのパッケージマネージャー.|
|chart|海図|Kubernetesのマニフェストのテンプレートをまとめたもの.|
|tiller|舵柄|デプロイを担うサーバーコンポーネント.|
|package|-|1つのアプリケーションとして成り立つchartの単位.<br>この単位でデプロイを行う.|
|release|-|packageがデプロイされて, k8s上でpodとして実際に動作しているもの.|
|repository|-|chartの置き場所. yumのリポジトリのようなイメージ.|
- **Reference:**
 - [Kubernetes: パッケージマネージャHelm](https://qiita.com/tkusumi/items/12857780d8c8463f9b9c)
 - [helm覚書](https://y-ohgi.hatenablog.com/entry/2018/05/21/010839)
 - [helmの過去、現在、未来](https://qiita.com/Ladicle/items/63cad824e27aa8aac7e1#構成と通信方法)
 - [Helmの概要とChart(チャート)の作り方](https://qiita.com/thinksphere/items/5f3e918015cf4e63a0bc)
