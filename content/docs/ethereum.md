# [Ethereum](https://github.com/ethereum)

## 目的
ブロックチェーン上でアプリを動かして, スマートコントラクトを実行できるようにして, 経済をさらに発展させる.  
アプリの特徴: 分散型, 管理者不在, 一度デプロイすると修正不可

## 特徴
- スマートコントラクトのプラットフォーム
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
