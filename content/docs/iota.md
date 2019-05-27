---
title: IOTA
---

# IOTAとは
**Githubリポジトリ:** https://github.com/iotaledger

## 目的
オープンIoTでブロックチェーンの良さ(分散性/耐改ざん性/オープン性)を使えるようにしつつ(スケーリング・マイニングコスト・トランザクションの承認の遅さを解決しつつ)インダストリー4.0を推し進めること.

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

## DBのある所
- https://db.iota.partners/iri-mainnet-snapshot.tar.gz
- https://x-vps.com/iota.db.tgz

## Academic Papers
- [Academic Papers](https://www.iota.org/research/academic-papers)

## C\#
- https://patriq.gitbook.io/iota/
