# [Node.js](https://github.com/nodejs)とは
サーバーサイドJavaScript環境のこと.
サーバーサイドもJavaScripで記述できるようにした.

## インストール
**Mac:** https://nodejs.org/en/

**Ubuntu:** [Official Node.js binary distributions](https://github.com/nodesource/distributions/blob/master/README.md)

## テストの種類
- **単体テスト:** 入力をモック化し, 個々の関数やクラスをテストし, 出力結果が予想通りであることを確認するテスト.
- **統合テスト:** いくつかのモジュールを組み合わせて予想通りに動作することを保証するテスト.
- **機能テスト:** 製品自体を使って(例えばブラウザを使って), あるシナリオをテストする. 確実に想定した動作をするかといった内部構造は考慮しない.
- **リグレッションテスト:** プログラムに機能を追加したり変更を加えたことによって、今まで普通に動いていた部分が動かなくなっていないかを確認するテスト.
- **受け入れテスト:** システム開発を外注して, 発注者の本来の目的や意図通りに稼働するかのテスト.

**Reference:** [2017年JavaScriptのテスト概論](https://postd.cc/a-complete-guide-to-testing-javascript-in-2017/)

## テストツールの種類
1. テストの環境を提供する（[Mocha](https://github.com/mochajs/mocha), [Jasmine](https://github.com/jasmine/jasmine), [Jest](https://github.com/facebook/jest), [Karma](https://github.com/karma-runner/karma)）
- テストの構造を提供する（[Mocha](https://github.com/mochajs/mocha), [Jasmine](https://github.com/jasmine/jasmine), [Jest](https://github.com/facebook/jest), Cucumber）
- アサーション機能を提供する（[Chai](https://github.com/chaijs/chai), [Jasmine](https://github.com/jasmine/jasmine), [Jest](https://github.com/facebook/jest), Unexpected）
- 生成, 表示, テスト結果をウォッチする（[Mocha](https://github.com/mochajs/mocha), [Jasmine](https://github.com/jasmine/jasmine), [Jest](https://github.com/facebook/jest), [Karma](https://github.com/karma-runner/karma)）<br>
  以前の実行時からの変更が意図されたものであることを確認するために,
- コンポーネントやデータ構造を生成し, スナップショットを比較する（[Jest](https://github.com/facebook/jest), [Ava](https://github.com/avajs/ava)）
- モック, スパイ, スタブを提供する（[Sinon](https://github.com/sinonjs/sinon), [Jasmine](https://github.com/jasmine/jasmine), [enzyme](https://github.com/airbnb/enzyme), [Jest](https://github.com/facebook/jest), testdouble)
- コードカバレッジのレポートを生成する（Istanbul, [Jest](https://github.com/facebook/jest))
- シナリオ実行の管理ができるブラウザ, または疑似ブラウザの環境を提供する（[Protractor](https://github.com/angular/protractor), [Nightwatch](https://github.com/nightwatchjs/nightwatch), Phantom, Casper）

**Reference:** [2017年JavaScriptのテスト概論](https://postd.cc/a-complete-guide-to-testing-javascript-in-2017/)

## 引数で判定
引数1つでok. それ以外は強制終了する.
```javascript
if(process.argv.length !== 3) {
  console.log(`csvファイル名を入力してください.`);
  process.exit(1); // 正常に強制終了する
}
```

## Streamとは
- データストリーム(データの転送単位がブロックより細かいバイト単位で行う方式)を扱うオブジェクト.
- Streamオブジェクトはデータをストリームとして(データを流れる様にして)扱いたい時に使う.
- **References:**
 - [Node.js Stream を使いこなす](https://qiita.com/masakura/items/5683e8e3e655bfda6756)
 - [Stream API入門](https://qiita.com/Mizunashi_Mana/items/872354cd7bf25090932f)

```javascript
// ブロッキングでファイルを読み書きする.
// データを読み込んでる間は全ての処理がストップする.
const text = fs.readFileSync('src.txt', 'utf8');
fs.writeFileSync('dest.txt', text);

// 非ブロッキングI/Oでファイルを読み書きする.
// データを読み込んでる間も他の処理はできるが, 書き込みは読み込みが終わってから.
fs.readFile('src.txt', 'utf8', (err, data) => {
  fs.writeFile('dest.txt', data);
});

// 一定量だけデータを読み込んで書き込みを行う.
// 大きなファイルだと大活躍.
const src = fs.createReadStream('src.txt', 'utf8');
const dest = fs.createWriteStream('dest.txt', 'utf8');
src.on('data', chunk => dest.write(chunk));
src.on('end', () => dest.end());

// pipeも使える.
const src = fs.createReadStream('src.txt', 'utf8');
const dest = fs.createWriteStream('dest.txt', 'utf8');
src.pipe(dest);
```

### Streamの種類
|名称|意味|
|---|---|
|stream.Readable|読み取りだけができるストリーム|
|stream.Writable|書き込みだけができるストリーム|
|stream.Duplex|読み取りも書き込みもできるストリーム|
|stream.Transform|読み取ったデータを変換して出力するストリーム|

### 公式Reference
- [Stream](https://nodejs.org/api/stream.html)

# NPM
Node.jsのパッケージ管理ツールのこと.

## npm自体のupdate
```bash
npm update -y npm
```

## パッケージを最新バージョンへ
```bash
npm outdated # 新しいバージョンを確認
npm install -g npm-check-updates # アップデートマネージャーをインストール
ncu # 新しいバージョンを確認
ncu -u # package.jsonを更新
npm update # パッケージを更新
```

# yarn
こちらもパッケージ管理ツール.

## パッケージを最新バージョンへ
```bash
yarn outdated # 新しいバージョンを確認
yarn upgrade # パッケージのアップグレード
yarn upgrade-interactive # 上記2つを一緒に行う
```
