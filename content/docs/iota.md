---
title: IOTA
---

# IOTAとは

**Githubリポジトリ:** https://github.com/iotaledger

## 目的
- オープンIoTでブロックチェーンの良さ(分散性/耐改ざん性/オープン性)を使えるようにしつつ(スケーリング・マイニングコスト・トランザクションの承認の遅さを解決しつつ)インダストリー4.0を推し進めること.
- IOTA財団の目的は, [オープンソースガバナンス](https://projects.eclipse.org/proposals/eclipse-iota-trinity)を使用して, IOTAをゴールドスタンダード（[主要組織](https://blog.iota.org/iota-becomes-a-founding-member-of-new-international-association-of-trusted-blockchain-applications-b0c6417aaded)や[標準化団体](https://www.omg.org/cgi-bin/doc?omg/2019-03-03)と協力して, エンタープライズ対応DLT）にすること.

## 特徴
- 小さなデータの保存と交換に特化した設計.
- 平衡三進数を使用.
- 量子コンピュータ耐性のために電子署名には[Winternitz One Time Signature](https://eprint.iacr.org/2011/191.pdf)を使用.
 - 署名は使い捨てなので送金に使ったアドレスにもう1度入金すると盗まれる可能性特大.
 - というか多額のIOTAを盗まれた実績有.
 - 今はそうならないようにwalletが上手に管理してくれてる.
- スケーリングのためにブロックは生成せずに各々のトランザクションがDAG形式で自分より前のトランザションを承認していく.
 - まだ発展途上なのでチェック機能が存在する.
 - トランザクションはInput/Output/Remainderの3種類が1まとめになったBundleとしてTangle内を流れてる.
- UTXOを採用.

## CfBによる技術的特徴
- **IoTは, インターネットを介して通信するものではありません.**
  - この誤解は, インターネット用の既存のソリューションを別の名前で販売しようとしている企業によって生み出されました.
  - モノは, インターネットよりもはるかに大きい, 独自のネットワークを形成します.
  - そのようなネットワークは, すべてのトラフィックが少数の組織によって制御されている少数のサーバーを通過するアーキテクチャを使用して拡張することはできません.
- **Tangleは1つだけであり, それだけです.**
  - いくつかのIOTAフォークを実行しても, それらはすべて単一のTangleで機能し, 理論的には後でマージすることができます.
  - IOTAを実行している異なるクラスターは, Tangleに絡み合って異なるサブタングルを形成します.
  - これは, モバイルのモノがあるクラスタから別のクラスタに移動するときに便利な機能です.
- **IOTAを実行しているノードのネットワークは, 1/3未満の要素の障害が重要ではないシステムです.**
  - もしそうならノードソフトウェアはこの機能を念頭に置いて開発されるべきです.
  - 特に, グリッチが発生した場合は, ノードを再起動し, 必要に応じてストレージをクリアすることができます.
  - IOTA上に構築されたソフトウェアは, そのノードが常に信頼できる状態で動作すると想定してはいけません.
  - ソフトウェアは少なくとも3つのノードに接続し, それらのクォーラム(67％+)によって提供される情報に依存します.
- **IOTAプロトコルは不変なものです.**
  - バージョニングを必要としません.
  - すべての改良はプロトコルの上でされなければなりません.
  - さもなければIOTAは物理的にアップグレードすることができないモノの多くによって採用されることができません.
- **IOTAはデータ保存用ではなく, データ配信用です.**
  - あるデータがTangleに格納されている場合, そのデータが24時間の間ずっとTangleで見つかることを期待しないでください.
  - データ片のハッシュを含むMerkleツリーのルートが常に更新されるようなトリックを使用してください.
  - データが欲しい場合は後でデータの所有者または専門のサービス（いわゆるpermanode）へ要求することができます.
- **IOTAはナノペイメントを可能にするもので, これを最大限に活用する必要があります.**
  - IOTAのために売られるものは何でも, 小さな塊で売られて, 支払いがネットワークによって受け入れられるといういくらかの保証が得られるのと同じくらい速く配達されるべきです.
  - たとえ彼らが「より遅い」売り手に対する競争上の優位性を成功させたとしても, 数セントの二重支払いで悩まされる人はほとんどいないでしょう.
- P.S正式にはIOTAは頭字語ではありませんが, 私は常に「黙示録」が「啓示」を意味する「モノのインターネット黙示録(Internet-Of-Things Apocalypse)」の略であると言ってきました.
- **Reference:** [Internet-of-Things Apocalypse](https://medium.com/@comefrombeyond/internet-of-things-apocalypse-eaf28002240d)

## trit, tryte
- `bit := trit`
 - `bit = (0, 1)`
 - `trit = (-1, 0, 1)`
- `byte := tryte`
 - `byte = 2^8 = 256`
 - `tryte = 3^3 = 27`

## 単位変換
- [IOTA Converters](https://laurencetennant.com/iota-tools/)
 - **GitHubリポジトリ:** [hyperreality/iota-tools](https://github.com/hyperreality/iota-tools)

## ICC Network Visualisation
- http://88.99.60.78:8080/

## devnet
- https://nodes.devnet.iota.org
- https://devnet.thetangle.org/nodes
- https://faucet.devnet.iota.org (devnetの蛇口)

## DBのある保存場所
- https://db.iota.partners/iri-mainnet-snapshot.tar.gz
- https://x-vps.com/iota.db.tgz
- https://dbfiles.iota.org

## Academic Papers
- [Academic Papers](https://www.iota.org/research/academic-papers)

## C\#
- https://patriq.gitbook.io/iota/

## 様々なプロジェクト
- [Coodicide](https://coordicide.iota.org): Coodinatorを削除する方法
- [Trinity](https://trinity.iota.org): 公式ウォレット
- [Qubic](https://qubic.iota.org): IOTA Tangle上のスマコン・オラクル・アウトソーシングなど
- [Data Marketplace](https://data.iota.org/): 情報の売買場
- [International Trade](https://tradedemo.iota.org/): トレーサビリティ
- [Certification](https://certification.iota.org): 証明書発行
- [Eco System](https://ecosystem.iota.org/): コミュニティの交流場
- [eCl@ss](https://eclass.iota.org/): データの規格
- [Troika](https://www.cyber-crypt.com/troika/): 3進数用のハッシュ関数
- [IOTA Area Codes](https://github.com/iotaledger/iota-area-codes): タグにエリアコードを埋め込む規格
- [IOTA IPFS](https://github.com/iotaledger/poc-ipfs): IOTAでIPFS

## Coordicide
- **公式サイト:** https://coordicide.iota.org
- Coodinatorを削除する方法のこと.
- Coordicideの機能をモジュール化した.
  - そうすることで将来のアップデートを容易にした.
- コアCoordicideモジュールは, 改良されたコンセンサスメカニズム, つまり分散化とセキュリティを最大限にするためのチップ選択アルゴリズムを補完する投票方式です.
- コンセンサスアルゴリズムには, セルラーコンセンサスと[高速確率的コンセンサス](https://arxiv.org/pdf/1905.10895.pdf)がある.
- [Coodicide White Paper](https://files.iota.org/papers/Coordicide_WP.pdf)
- **モジュール1: ノードIDとマナ**
  - ノードにIDを振り分け, マナというノードのレピュテーションシステムを導入する.
  - トランザクションはノードがトランザクションを伝搬してくれたら, マナというトークンをノードに上げる.
  - マナは, 評判を得るのは難しいが, 失うのは簡単だという考えに依存している.
  - レピュテーションシステムの重要な側面は, 以前に与えられたレピュテーションを取り消すことによって悪いノードを罰する側面.
- **モジュール2: セキュアなオートピアリング**
  - 自動でネイバーを接続させて, 特定のノードに攻撃が起こらないようにする.
  - スモールワールドネットワーク(ほとんどのノードが互いに隣接していないタイプの数学的グラフ)を構築する.
- **モジュール3: スパムプロテクション**
  - 最近発行されたトランザクションの数及びマナのような異なる要因に基づいてノード当たりのPoWの難しさを知的に変化させる適応レート制御メカニズムを設置する.
  - マナの量が多いノードは, 評判の悪いノードと同じPoW要件なしに, より多くのトランザクションを発行することができる. ノードのマナに関係なく, PoWの難易度はトランザクションレートとともに増加もする. すなわち, 短い時間間隔でより多くの取引を発行するためには, ノードは暗号パズルの難易度を上げなければならないが, 一方, 低い取引レートでは, はるかに低い難易度で十分となる.
  - スパムをさらに防ぐために, ノードあたりの最大トランザクションレートの制御も行う.
      - 対象はIoT.
- **モジュール4: チップ選択アルゴリズム**
  - 累積荷重を廃止して, Tangleの優先部分を識別するための投票層を追加する.
  - 累積荷重の悪い点
      - 正直なトランザクションでも累積荷重が無いと取り残された.
      - 攻撃者は累積荷重を悪用して, パラサイトチェーンやTangle分裂を行おうとする.
      - トランザクションの累積荷重を計算することは比較的費用がかかり, 特にハイスループットのシナリオではプロトコルのスケーラビリティに問題を引き起こす.
- **モジュール5: 積極的な矛盾解決**
  - 累積荷重を止めるために, ノードが投票によって意見を交換する追加のセキュリティ層を提案する. **投票者モデル**については, 長年にわたりかなりの研究が行われてきた. 確率モデルでは, ノードは複数のラウンドにわたって少数の他のノードの意見を要求し, そしておそらくはそれ自身の意見も変える.
  - 投票メカニズムを導入するメリット
      - どんどん発行されるトランザクションから状況を解決するまで待つのではなく, ノードが互いに対話して状況を予防的に解決できるようになる.
      - ノードの投票は, それが持つマナの量に応じて重み付けされる. したがって, 優秀な関係者はネットワークに大きな影響を与えることができる.
      - 正直なノードは, たとえ現在トランザクションを発行していなくても, 投票によってネットワークを保護できる. 提案されたシビル保護メカニズム(マナ)と組み合わされて, これは, PoWに依存することなく, ブロックチェーンにおける一定の正直なハッシング力を置き換える.
      - コンセンサスプロセスは, チップ選択やTangleの構造など, 他の側面から切り離されている. これにより, 将来の要件に合わせて簡単に調整できるモジュラDLTが実現できる. また, ホワイトペーパーに記載されているパラサイトチェーン攻撃などの最も危険な攻撃を含め, Tangleの構造を操作して合意メカニズムを破るあらゆる種類の攻撃を防ぐことができる.
  - 新たに**Shimmer**という投票スキームを提案する.
  - Shimmer内での投票交換の候補として2つの候補を提示する.
      - セルオートマトンの振る舞いを模した「Cellular Consensus」
      - 確率論を使用して強力なセキュリティ保証を提供する「Fast Probabilistic Consensus」
- **モジュール5.1: Shimmer**
  - いくつかの事前定義された規則に従って行動する個々の自律エージェントは, 蜂, アリ, 魚群などの自然界の多くのシステム, さらには物理学のある分野でさえ見つけることができます. 非常に単純なルールは, やがてシステムの緊急の特性として現れ, 非常に複雑な機能を作成することができます. Shimmer合意メカニズムも同様に機能します. 他のすべてのノードの意見を再構築しようとするのではなく, 非常に小さいノードのサブセットの意見だけに注意を払い, ネットワークの緊急の特性として有機的に合意が形成されるようにします.

### From Hans Moog[IF]
[one important aspect about this whole solution is that we only vote on conflicts that are arriving more or less at the same time - that means that even an attacker that has unlimited resources (aka unlimited nodes or unlimited amounts of hashing power) - they can only decide which one of the two conflicts wins but they can not rewrite history so even if you invest 1 billion dollars you can not break the network.](https://twitter.com/CryptoVortek/status/1133410604658466816)
### From David Sonstebo
[While IoT is undoubtedly still IOTA's chief focus point, this approach to Coordicide enables it to also cover the regular Internet. I want to see decentralized Facebook, AWS and all. IOTA's vision is expanding.](https://twitter.com/iotatokennews/status/1133433350125965313)
