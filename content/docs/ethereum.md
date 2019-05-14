# [Ethereum](https://github.com/ethereum)とは

## 目的
ブロックチェーン上でアプリを動かして, スマートコントラクトを実行できるようにし, 経済をさらに発展させる.  
アプリの特徴: 分散型, 管理者不在, 一度デプロイすると修正不可

## 特徴
- 分散型アプリケーション(スマートコントラクト)のプラットフォーム
- 各ノードにバーチャルマシーン(通称: EVM)を積んでいるのでチューリング完全なスクリプトがEthereum上で書ける.
- アカウントベース
- ブロックチェーン2.0の代表格

## スケーリング方法
- サイドチェーン
 - Plasma: Ethereumのメインチェーンとは別のプラズマチェーンと呼ばれるサイドチェーンをマークル木状にどんどん連ねることで, メインチェーンに格納される情報量を減少させる仕組み. エンドユーザーが出金したいときはルートチェーンに問い合わせる.
 - [初心者向けプラズマ勉強法まとめノートー 0から3週間で学んだこと](https://medium.com/cryptoeconomics-lab/初心者向けプラズマ勉強法まとめノートー-0から3週間で学んだこと-f9d68a4d617f)
 - [lightningとplasmaってなにが違うんですか？](https://delegatecall.com/ja/questions/lightningplasma-d3294ff2-b7f8-4118-a556-a6124ef94097)
 - [Plasma スケーラブルな自律型スマートコントラクト](https://github.com/shogochiai/plasma-whitepaper-jp/blob/master/whitepaper.pdf)
- オフチェーン
 - Raiden Network: ブロックチェーン内の処理をブロックチェーンの外側に持っていくことで, メインチェーンの負担を軽減する仕組み. 最終的な取引結果のみをブロックチェーンで処理する.
- シャーディング
 - トランザクション処理を各ノードで分割して行うことで, 処理速度の向上を図る仕組み. Ethereum2.0のCasperで導入されるPoSと相性が良い.
 - [Ethereumのシャーディング概論](https://www.slideshare.net/bitbankink/ethereum-132664122)


## スマートコントラクトとは
- 何人かが合意した内容（契約）を、ヒトがいなくても自動的に実行する仕組み
 - 例は自動販売機



## Ethereum dapps開発七つ道具
1. [Solidity](https://github.com/ethereum/solidity)
2. [MetaMask](https://github.com/MetaMask/metamask-extension)
3. [Truffle](https://github.com/trufflesuite/truffle), [Ganache](https://github.com/trufflesuite/ganache)
4. [web3.js](https://github.com/ethereum/web3.js)
5. [OpenZepplien](https://github.com/OpenZeppelin/openzeppelin-solidity)
6. [Remix](https://github.com/ethereum/remix)
7. [Google翻訳](https://translate.google.com/?hl=ja)

と, 上記の内容を広く浅く説明してくれてるサイト

- [EthereumとContracts開発を取り巻くエコシステムの概要](https://blockchain.gunosy.io/entry/ethereum-contracts-system)


## gethの使い方
gethとはGoで実装された完全なethereumノードを実行するためのコマンドラインインターフェースのこと.
```bash
# gethのインストール
# for Mac
brew tap ethereum/ethereum
brew install ethereum
```
```bash
# ジェネシスブロックの生成の様式を作成
cd
mkdir ethereum
cd ethereum
mkdir privnet
cd privnet
touch genesis.json
# 以下をgenesis.jsonの中に書き込む
{
  "config": { "chainId": 4649 },
  "nonce": "0x0000000000000042",
  "timestamp": "0x0",
  "parentHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
  "extraData": "",
  "gasLimit": "0x8000000",
  "difficulty": "0x4000",
  "mixhash": "0x0000000000000000000000000000000000000000000000000000000000000000",
  "coinbase": "0x3333333333333333333333333333333333333333",
  "alloc": {}
}
```
```bash
# ジェネシスブロックを生成
geth --datadir ~/ethereum/privnet init ~/ethereum/privnet/genesis.json
```
```bash
# ログを書き込むファイルを作成
cd
cd ethereum/privnet
touch geth.log
```
```bash
# gethの起動の仕方
geth --networkid 4649 --nodiscover --datadir ~/ethereum/privnet --rpc --rpccorsdomain "*" console 2>> ~/ethereum/privnet/geth.log
```
で,
gethを起動しつつ,
`--rpc`と`--rpccorsdomain`で, `localhost`に繋ぐ許可を出しつつ,
`console`で対話型にしつつ,
`>> geth.log`でlogをgeth.logファイルに出力している.

そして, 別ターミナルを開いて,
```bash
# logの見方
tail -f geth.log
```
で, そのターミナルでlogを表示する.
```bash
# アカウント作成する
personal.newAccount("passwd")
# アカウントを確認する
eth.accounts
# マイニングするノードを確認
eth.coinbase
# etherの採掘スタート
miner.start()
# etherの採掘ストップ
miner.stop()
# ブロック高確認
eth.blockNumber
# ブロックしてるか確認
eth.mining
# ハッシュレート確認
eth.hashrate
# ブロック内容確認
eth.getBlock(0)
# 残高確認
eth.getBalance(eth.accounts[0])
# ロック解除
personal.unlockAccount(eth.accounts[0])
# 送金
eth.sendTransaction({from: eth.accounts[0], to: eth.accounts[1], value: web3.toWei(3, "ether")})
```
```bash
# solidityコードをコンパイル
solc --bin --abi hello_world.sol
# --bin でバイトコード生成.
# --abi でabi(コントラクトとトランザクションで通信する際に必要な情報を全て格納したもの)を生成.
```

## Remixの使い方
Remixとはブラウザ上で動作するSolidityの統合開発環境（IDE）のこと.

- https://remix.ethereum.org

## truffleの使い方
- truffleで使うsolidityのバージョンを変えるには`truffle-config.js`の中の`compilers`の`solc`の`version`を変更すれば良い.

## Batch Over Flow を体験してみる
- [ERC20のバグと誤報されたBatchOverFlowを体験してみる](https://y-nakajo.hatenablog.com/entry/2018/05/01/005938)

## Ethereumのハッキングの練習
- [The Ethernaut byZeppelin](https://ethernaut.zeppelin.solutions)

## オンラインでsolidityを学ぶ
- [CryptoZombies](https://cryptozombies.io/jp/)
- [Zastrin](https://www.zastrin.com/)
- [ChainShot](https://www.chainshot.com/)
- [openberry](https://www.openberry.ac/)
- [Webのインターフェイスでブロックチェーンのプログラミングを学べる学習サイト5選]( https://coinchoice.net/5-learning-sites-for-learning-blockchain-programming/)
- [動画で学ぶブロックチェーン】Ethereumのスマートコントラクト開発入門 - 谷口耕平氏]( https://goblockchain.network/2019/03/hello_ethereum_handson/)

