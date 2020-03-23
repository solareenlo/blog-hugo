---
title: "Plasma × Substrate 勉強会 #1"
date: 2019-04-18T08:00:00+09:00
author: "solareenlo"
categoies: [
  "Blockchain"
]
tags: [
  "Plasma",
  "Substrate",
  "Rust"
]
menu:
  main:
    parent: tutorials
---

> [Plasma × Substrate 勉強会 #1](https://neutrino.connpass.com/event/127035/)の自分なりのメモ

# Introducing Plasma Chamber

---

## プラズマとは

- セキュアにトランザクションをさばく.
- マークル木を使ってデータを圧縮する.
- 1分ごとに行う.
- エンドユーザーが出金したいときはルートチェーンに問い合わせる.
- 他の人にチャレンジされなければ許可されて出金される仕組み.
- スケーラブル・セキュリティ・ユーザビリの高いDappasが作れるぞ.

## プラズマの悪い点
- 受取手はトランザクションの履歴を確認しないといけない.
- ファイナリティは待たないといけない.
- Exit期間があるのでUIが悪い

## Plasma Chamberは上記の3つの悪い点を改善するぞ

### What I mean by 'usable'

High TPS, Less Gas, Work on Mobile, Instant Finality, ERC20使える

### 特徴

- Exit Game
- Operatorが資金をかっさらう事がある. それに対する対策を行った.
- Gas Const/Proof Size Reduction
- Instantaneous Finality
- Fast Finality Contract に供託しておく.
- ユーザー, マーチャント, オペレーター, Ethereum
- オペレーターまではhttpsで通信する.
- オペレーターまでなら2秒以下でファイナリティが得られる.
- Ethereumのブロックチェーンまで待つと2.5分から7分かかる.
- Plasma MVP → Plasma Cash → Plasma Cashflow → ???
- Plapp 出金したい人と入金したい人を合わせるPlapp.


# Plasma Substrate Runtime Module Library

---

PlasmaをSubstrateの上で行おう!

> 発表資料: [Plasm: Plasma Substrate Runtime Module Library](https://docs.google.com/presentation/d/1DAgE7AJHSAP-t2pwAKygD8DITCJkkKBcqHtuV_OqbY4/mobilepresent?slide=id.p)  
> 発表資料: [Staked Substrate](https://speakerdeck.com/staked/0318substratehuresenzi-liao)  
> Substrateとはブロックチェーン開発のフレームワークのこと.  
> Reference: [Polkadotとブロックチェーン開発フレームワークSubstrateを俯瞰する](https://coinchoice.net/overview-polkadot-and-substrate/)

Plasmaには種類が沢山ある.
Substrate Chainの上に全てのPlasmaを作れるようにしよう.

親で且つ子でもあるPlasma Chainを作ろう.
そうする事で無限のスケーラビリティがある.

## Plasma Dreams
大きく3つのライブラリがある
- plasma-utxo
- plasma-parent
- plasma-child

UTXOを使って, 自分の残高がけを保存する.
トランザクションは自分で定義できる.
マークル木に対応したものと対応していないもの両方に対応したい.

