---
title: Bitcoin
---

# [Bitcoin](https://github.com/bitcoin)
## 目的
匿名性を自分で選択できつつ, 安全に価値の移転と保存に特化したシステムを構築すること.

## 特徴
- 非中央集権
- 自律分散
- トラストレス
- 耐改ざん性
- オープンソース
- 匿名性を選択できる
- 二重支払いができない
- 払い(価値の保存と移転)に特化したとても堅牢なシステム(チューリング不完全/マルチシグ/HDウォレット/ニーモニック/Base58とか)

## 技術
- 電子署名
- 公開鍵暗号
  - [楕円曲線DSA](https://ja.wikipedia.org/wiki/楕円曲線DSA)
- ハッシュ関数
- ネットワーク
  - P2P
  - Proof of Work

## プレイヤー
- [コア開発者](https://github.com/bitcoin)
- マイナー
- ユーザー
- 投資家・投機家
- 法律

## スケーリング方法
- コンセンサスアルゴリズムを速くする（分散性のために1ブロック生成にわざと10分かけてる）
- ブロックに含まれるトランザクションを増やす（Block weightが4MB以下/segwit導入により）
- オフチェーン（Lightning networkなど）
- サイドチェーン
- シャーディング
- **Reference:** [ブロックチェーンとスケーラビリティ](https://medium.com/uniqys/blockchain-scalability-d95ee27c5092)

## フルノード
### フルノードの立て方
- [ビットコインのフルノードを立てる方法](https://bitcoin-node.info/)
- [Bitcoin ClockUpMemo](https://bitcoin.clock-up.jp/)
- [bitcoindのブロック保存先ディレクトリを指定する](https://blog.logicky.com/2017/05/14/bitcoindのブロック保存先ディレクトリを指定する/)
- [Bitcoin.conf Configuration File](https://en.bitcoin.it/wiki/Running_Bitcoin#Bitcoin.conf_Configuration_File)

### フルノードの分布図
- [BITNODES](https://bitnodes.earn.com/)

### マイナーとノードの違い
- マイナーも1つのノードではあるが, マイナーの主な役割はプルーフオブワーク（PoW）を行い取引をブロックに収納しネットワークに送信すること.
- マイナー以外のユーザーがノードを立ち上げる主な目的は, 取引が正しくブロックに取り込まれたかを検証しブロックチェーンに追加する.
- マイナーには新規発行のビットコインの取得という経済的インセンティブがある.
- ノードを立ち上げるユーザーには, 自身で取引の正当性を検証することができることと, ノードが増えることで非中央集化するネットワークのセキュリティを強化するというインセンティブがある.
- **Reference:** [ノードの分布状況から紐解く、世界に広がるビットコインのネットワーク](https://btcnews.jp/1hudmbki16479/)

## bitcoindとbitcoin-cliの違い
- **bitcoind:** Bitcoinの一通りの機能を実装したしたサーバーのこと.
  - Bitcoinネットワーク(P2Pネットワーク)との通信のフル機能
  - ウォレット機能
  - マイニングプール
  - マイニング
  - JSON-RPC APIサーバー
- **bitcoin-cli:** JSON-RPCを使って, bitcoindと通信し, 様々な操作を行えるツールのこと.
- **References:** [Bitcoinウォレットの比較](https://bitcoin.peryaudo.org/comparison.html)

### bitcoindにcurlでアクセス
```bash
# getblockinfo
curl --data-binary '{"jsonrpc":"1.0","id":"curltext","method":"getblockchaininfo","params":[]}' -H 'content-type:text/plain;' http://solareenlo:solareenlo@127.0.0.1:8332/ | jq
```

### bitcoin-cliとxargsの使用例
```bash
# ブロック高が一番高いブロックの内容を出力する例
bitcoin-cli getbestblockhash | xargs bitcoin-cli getblock
# 最新ブロックのcoinbase txのトランザクションIDを取得する例
bitcoin-cli getblockhash 1 | xargs -IXXX bitcoin-cli getblock XXX 2 | jq ".tx[0].txid"
# 最新ブロックの一番初めのトランザクション内容を表示する例
bitcoin-cli getbestblockhash | xargs bitcoin-cli getblock | jq ".tx[0]" | xargs bitcoin-cli getrawtransaction | xargs bitcoin-cli decoderawtransaction
```
**References:** [開発によく使うbitcoin 関連cliツール](https://medium.com/blockchain-engineer-blog/開発によく使うbitcoin-関連cliツール-12006691b4a1)

### bitcoin-cliを使ってUTXOを集める
- [Output Descriptorとscantxoutsetを使ってUTXOセットをスキャンする](https://techmedia-think.hatenablog.com/entry/2018/10/15/161105)

## Blockchainの中身の見方
- [blk.dat](http://learnmeabitcoin.com/glossary/blkdat)

## アドレス
### 一般的なアドレス生成の流れ
1. 秘密鍵からECDSAで公開鍵を生成
- 公開鍵をハッシュ関数SHA-256に通しハッシュ値を得る
- そのハッシュ値をさらにハッシュ関数RIPEMD-160に通しハッシュ値を得る
- ハッシュ値の先頭にプレフィックスとして00を加える
- ハッシュ関数SHA-256に通す
- もう一度ハッシュ関数SHA-256に通す
- 4バイトのチェックサムを一番後ろに加える
- Base58のフォーマットでエンコーディングする

**Reference:** [秘密鍵から公開鍵そしてアドレスが生成されるまでの流れ【仮想通貨】](https://zoom-blc.com/from-private-key-to-address)

### 一般的なアドレス生成の図
<img src="/images/bitcoin/public-key-to-bitcoin-address.png" width="50%" height="50%"><img src="/images/bitcoin/base58check-encoding.png" width="50%" height="50%">

**Reference:** [秘密鍵から公開鍵とビットコインアドレスを生成する方法](https://tomokazu-kozuma.com/a-method-of-generating-a-public-key-and-a-bitcoin-address-from-a-private-key/)

### 秘密鍵/公開鍵/アドレスの関係
<img src="/images/bitcoin/pubkey-prvkey-address.gif" width="100%" height="100%">

**Reference:** [公開鍵、秘密鍵、ビットコインアドレスの関係](https://www.crypto-currencies.jp/bitcoin/remittance/wallets.html)

### 秘密鍵/WIF/公開鍵/アドレスの関係
<img src="/images/bitcoin/wif-prv-pub-address.png" width="100%" height="100%">

**Reference:** [暗号通貨(Bitcoin, Monacoin)のプロトコルを理解する: 公開鍵と秘密鍵](https://qiita.com/monapay/items/f708f61f2ad102b548f8)

### HDウォレットにおけるアドレス生成の手順
<img src="/images/bitcoin/hdwallet.png" width="100%" height="100%">
**Reference:** [bips/bip-0032.mediawiki](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki)

Entropy ([BIP39](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki), 128bit, 160bit, 192bit, 224bit, 256bitの長さ)  
↓  
Mnemonic ([BIP39](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki), 2048(11bit)個の単語群から選んだ、12個, 15個, 18個, 21個, 24個の単語群)  
↓  
Master Node ([BIP39](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki), 512bit(=64byte))  
↓  
左半分: Master Key([BIP32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki)), 右半分: Chain Code([BIP32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki))  
↓ (Master Key + Chain Code)  
拡張鍵 ([BIP32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki))  
↓  
HD Wallet ([BIP32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki))  
↓  
アドレス

### Entropyのターミナルでの作成方法
```bash
# entropyの生成はmacのターミナルで
$ cat /dev/urandom |LC_ALL=C tr -dc 'a-f0-9' | fold -w 64 | head -n 1
# entropyの生成はlinuxの端末で
$ cat /dev/urandom |tr -dc a-f0-9|head -c${1:-64}
```

### アドレスの種類
<img src="/images/bitcoin/bitcoin-address.jpg" width="100%" height="100%">

**Reference:** [Address](https://en.bitcoin.it/wiki/Address)

## プレフィックス
|Decimal prefix|Hex|Example use|Leading symbol(s)|
|---|---|---|---|
|0|00|Pubkey hash (P2PKH address)|1|
|5|05|Script hash (P2SH address)|3|
|128|80|Private key (WIF, uncompressed pubkey)|5|
|128|80|Private key (WIF, compressed pubkey)|K or L|
|4 136 178 30|0488B21E|BIP32 pubkey|xpub|
|4 136 173 228|0488ADE4|BIP32 private key|xprv|
|111|6F|Testnet pubkey hash|m or n|
|196|C4|Testnet script hash|2|
|239|EF|Testnet Private key (WIF, uncompressed pubkey)|9|
|239|EF|Testnet Private key (WIF, compressed pubkey)|c|
|4 53 135 207|043587CF|Testnet BIP32 pubkey|tpub|
|4 53 131 148|04358394|Testnet BIP32 private key|tprv|
|||Bech32 pubkey hash or script hash|bc1|
|||Bech32 testnet pubkey hash or script hash|tb1|
[List of address prefixes](https://en.bitcoin.it/wiki/List_of_address_prefixes)

## 単位
0.00000001 BTC = 1 Satoshi  
1 BTC = 100000000 Satoshi

## Script
- TransactionとScriptとLightning Networkについて分かりやすいスライド
 - [スクリプトから理解するライトニングネットワーク](https://www.slideshare.net/YukiInoue1/bitcoin-87157473)
- サトシは後に, 以下の2つの理由でP2PK ではなくP2PKHを使うことを決めた.
 - 楕円曲線暗号（公開鍵や秘密鍵に使われれている暗号）が, 楕円曲線上の離散対数問題を解くために改良されたショアのアルゴリズムによって解かれてしまうから. 簡単に言うとそれが意味するのは, 理論上, 量子コンピューターがそう遠くない未来に公開鍵から秘密鍵を導出できてしまうということ. ビットコインを使うときだけ公開鍵を公開することによって, そういった攻撃を無力化することができる（一度使われたビットコインアドレスを二度と使わない前提だが）.
 - ハッシュサイズがより小さくなるので（20バイトになる）, 印刷するにも小さくできるしQRコードのような小さい記録媒体に埋め込むことがより簡単になる.
 - **Reference:** [P2PKH (Pay to Public Key Hash)](https://programmingblockchain.gitbook.io/programmingblockchain-japanese/other_types_of_ownership/p2pk-h-_pay_to_public_key_-hash)

### ScriptSig
 ScriptSigはInputにあって, [$ \sf{ScriptSig} \fallingdotseq \sf{Unlock Script}]
  ScriptSigにはSigとPubKeyがある.
  そうすることでP2PKHでは, ScriptPubKeyにあるhash化されたPubKeyとScriptSigにあるPubKeyを見比べてTrueを得た後に, さらにScriptSigにあるSigとPubKeyを見比べてTrueを得ることができる.

### ScriptPubKey
 ScriptPubKeyはOutputにあって, [$ \sf{ScriptPubKey} \fallingdotseq \sf{Lock Script}]
  Lockしないと誰でもBTCを取り出せてしますから.

### P2SHのScriptPubKey
```bash
# P2SHのScriptPubKey
OP_HASH160 [20-byte-hash-value] OP_EQUAL
```

### オンラインでBitcoin Scriptの挙動を確認できるサイト
- [Bitcoin Script Online Debugger](https://bitcoin-script-debugger.visvirial.com)


## トランザクション
トランザクション生成と検証の仕方が分かりやすいスライド

- [Bitcoinを技術的に理解する](https://www.slideshare.net/kenjiurushima/20140602-bitcoin1-201406031222) の32ページ目から
- [Bitcoinのtransactionの署名検証をscriptから理解する](https://tech.coincheck.blog/entry/2019/02/12/113706)

## Segwit
- [SegWitの分かりづらい点とその理由](https://medium.com/blockchain-engineer-blog/segwitの分かりづらい点とその理由-83532a40c204)

### スクリプト構造
```bash
# P2WPKH
witness      : <signature> <pubkey>
scriptSig    : (empty)
scriptPubKey : 0 <20-byte SHA160(pubkey)>
               (0x0014{20-byte SHA160(pubkey)})
# P2WSH
witness      : 0 <signature1> <1 <pubkey1> <pubkey2> 2 CHECKMULTISIG>
scriptSig    : (empty)
scriptPubKey : 0 <32-byte sha256(witness script)>
               (0x0020{32-byte sha256(witness script)})
```

### Base32
<img src="/images/bitcoin/segwit-address-format.png" width="100%" height="100%">
**Reference:** https://bc-2.jp/materials/0105_Bech32.pdf



## Transaction Fee
- [PREDICTING BITCOIN FEES FOR TRANSACTIONS.](https://bitcoinfees.earn.com/)
- [Bitcoin Avg. Transaction Fee historical chart](https://bitinfocharts.com/comparison/bitcoin-transactionfees.html)

## References
- [bitcoinのしくみ](https://bitcoin.peryaudo.org/index.html)
- [Programming The Blockchain C# 日本語](https://programmingblockchain.gitbook.io/programmingblockchain-japanese/)
- [Blockchain Core Camp season1のビデオ資料](https://bc-2.jp/season1)
- [Blockchain Core Camp season1のpdf資料](https://bc-2.jp/season1/materials)
- [Blockchain Core Camp season2のビデオ資料](https://bc-2.jp/season2)
