# [Node.js](https://github.com/nodejs)とは
- サーバーサイドJavaScript環境のこと.
- サーバーサイドもJavaScripで記述できるようにした.
- シングルスレッドでノンブロッキングI/Oを行う.

## インストール
**Mac:** https://nodejs.org/en/

**Ubuntu:** [Official Node.js binary distributions](https://github.com/nodesource/distributions/blob/master/README.md)

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

## 日付を扱う
- [moment/moment](https://github.com/moment/moment)
- [iamkun/dayjs](https://github.com/iamkun/dayjs)

## 引数で判定
引数1つでok. それ以外は強制終了する.
```javascript
if(process.argv.length !== 3) {
  console.log(`csvファイル名を入力してください.`);
  process.exit(1); // 正常に強制終了する
}
```

## mapの中で非同期処理
- [Node 高階関数内での非同期処理（async/await）をどう書くか](https://t-kojima.github.io/2018/07/18/0028-async-await-in-loop/)
- [async/awaitやPromiseで簡単に配列のイテレーションできるようにする](https://qiita.com/toniov/items/127267fb64a960e8166e)
- [toniov/p-iteration](https://github.com/toniov/p-iteration)

## CSVを扱う
- [csv.js](https://csv.js.org)を使えば簡単.
- CSVの生成/読み取り/変換/書き出しができる.
- stream/コールバック/同期的にデータを処理ができる.

### stream
以下を行ってから
```bash
npm init -y
npm i --save csv
touch index.js
```
`index.js`に以下を書き込んで
```javascript
const csv = require('csv');

const generator = csv.generate({seed: 1, columns: 2, length: 3});
const parser = csv.parse();
const transformer = csv.transform(data => {
  return data.map(value => {return value.toUpperCase()});
});
const stringifier = csv.stringify();
// CSVを生成して
generator.on('readable', () => {
  while(data = generator.read()){
    parser.write(data);
  }
});
// CSVを読み込んで
parser.on('readable', () => {
  while(data = parser.read()){
    transformer.write(data);
  }
});
// データを変換して
transformer.on('readable', () => {
  while(data = transformer.read()){
    stringifier.write(data);
  }
});
// 文字列に変換して
stringifier.on('readable', () => {
  while(data = stringifier.read()){
    // 標準出力する.
    process.stdout.write(data);
  }
});
```
以下を実行する.
```bash
node index.js
```

### pipeでstream
以下を行ってから
```bash
npm init -y
npm i --save csv
touch index.js
```
`index.js`に以下を書き込んで
```javascript
const csv = require('csv');

// CSVを生成して
csv.generate  ({seed: 1, length: 3}).pipe(
// CSVを読み込んで
csv.parse     ()).pipe(
// データを変換して
csv.transform (record => {
                return record.map(value => {
                  return value.toUpperCase()
              })})).pipe(
// 文字列にして
csv.stringify ()).pipe(
// 標準出力する.
process.stdout);
```
以下を実行する.
```bash
node index.js
```

### コールバック
以下を行ってから
```bash
npm init -y
npm i --save csv
touch index.js
```
`index.js`に以下を書き込んで
```javascript
const csv = require('csv');

// CSVを生成して
csv.generate({seed: 1, columns: 2, length: 3}, (err, data) => {
  // CSVを読み込んで
  csv.parse(data, (err, data) => {
    // データを変換して
    csv.transform(data, data => {
      return data.map(value => {return value.toUpperCase()});
    }, (err, data) => {
      // 文字列に変換して
      csv.stringify(data, (err, data) => {
        // 標準出力する.
        process.stdout.write(data);
      });
    });
  });
});
```
以下を実行する.
```bash
node index.js
```

### 同期処理
以下を行ってから
```bash
npm init -y
npm i --save csv-generate csv-parse stream-transform csv-stringify
touch index.js
```
`index.js`に以下を書き込んで
```javascript
const generate = require('csv-generate/lib/sync');
const parse = require('csv-parse/lib/sync');
const transform = require('stream-transform/lib/sync');
const stringify = require('csv-stringify');

// CSVを生成して
const csvFile = generate({
  seed: 1,
  objectMode: true,
  columns: 2,
  length: 2
});
// CSVを読み込んで
const input = parse(csvFile, {
  columns: true,
  skip_empty_lines: true
});
// データを変換して
const records = transform(input, record => {
  record.push(record.shift())
  return record
});
// 文字列に変換して
const moji = stringify(records, (err, output) => {
  // 出力する.
  console.log(output);
});
```
以下を実行する.
```bash
node index.js
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

## npmを視覚的に比較
- [johnmpotter/npm-trends](https://github.com/johnmpotter/npm-trends)

# yarn
こちらもパッケージ管理ツール.

## パッケージを最新バージョンへ
```bash
yarn outdated # 新しいバージョンを確認
yarn upgrade # パッケージのアップグレード
yarn upgrade-interactive # 上記2つを一緒に行う
```
