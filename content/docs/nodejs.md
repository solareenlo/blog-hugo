# Node.jsとは
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
