---
title: "bitcoindへのアクセス方法"
date: 2019-04-17T09:00:00+09:00
author: "solareenlo"
categories: [
  "Blockchain"
]
tags: [
  "Bitcoin"
]
menu:
  main:
    parent: tutorials
---
***

bitcoindへのアクセス方法3選(bitcoin-cli, curl, POST).
3つとも[JSON-RPC](https://ja.wikipedia.org/wiki/JSON-RPC)で通信してる.

> bitcoindとは, 名前の通りunixのデーモンとして動作する事を目的とするBitcoinのクライアントで, JSON-RPCで開発者向けのAPIを提供する.
> したがって, Webサービスとして動作するBitcoinウォレットのバックエンドとしてや, マイニングプールのサーバーとして使われる.
>
> **Reference:** [Bitcoinウォレットの比較](https://bitcoin.peryaudo.org/comparison.html)


## 1. bitcoin-cliを使ってアクセスする.
> bitcoin-cliとは, bitcoindへJSON-RPCを使ってアクセスするツールのこと.

bitcoindを使ってBitcoinのフルノードを立ち上げて,
```
bitcoin-cli getblockchaininfo
```
とか.


## 2. [cURL](https://ja.wikipedia.org/wiki/CURL)を使ってアクセスする.
下記curlを行う要件.

- ネットワーク: mainnet
- 接続環境: ローカル
- ポート番号: 8332
- ユーザーの名前: user-name
- パスワード: user-password
- 投げつけているbitcoin-cliのメソッド: getblockchaininfo

```
curl --data-binary '{"jsonrpc":"1.0","id":"curltext","method":"getblockchaininfo","params":[]}' -H 'content-type:text/plain;' http://user-name:user-password@127.0.0.1:8332/ | jq
```

## 3. 自作プログラムでPOSTしてアクセスする.
bitcoindはHTTPリクエストメソッドのPOSTに対応しているので, JSON-RPCをPOSTで投げつける.
以下のプログラムはNode.jsを使った例.

- https://github.com/solareenlo/bc-json-rpc
