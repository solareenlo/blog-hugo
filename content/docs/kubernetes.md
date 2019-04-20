# [Kubernetes](https://github.com/kubernetes)とは
コンテナ・オーケストレーターの一種.
### Kubernetesが他と違うところは,
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
