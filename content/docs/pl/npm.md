---
title: npm
---

# npm とは
- Node.js のパッケージ管理ツール．
- **公式サイト:** https://npmjs.com

## インストール
### npm 自体のインストール
- [Downloading and installing Node.js and npm](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm)

### npm 自体の update
```bash
npm update -y npm
# もしくは
npm install npm@latest -g
```

### npm 自体のバージョン管理
**Reference:** [How to Update Node.js to Latest Version (Linux, Ubuntu, OSX, Others)](https://www.hostingadvice.com/how-to/update-node-js-latest-version/)

### パッケージのインストール
```bash
npm i <パッケージ名> # 本番環境へインストール
npm i -D <パッケージ名> # 開発環境へインストール
```

### パッケージの脆弱性確認
```bash
npm audit
```

### パッケージを最新へ
```bash
npm outdated # 新しいバージョンを確認
npm install -g npm-check-updates # アップデートマネージャーをインストール
ncu # 新しいバージョンを確認
ncu -u # package.json を更新
npm update # パッケージを更新
```

# 便利なパッケージ

## npm-run-all
- 複数の npm を実行するためのコマンドラインツール
- **GitHub リポジトリ:** [mysticatea/npm-run-all](https://github.com/mysticatea/npm-run-all)

### インストール
```bash
npm i -D node-run-all
```

### 簡単な使い方
```bash
run-s <1つ目のタスク> <2つ目の処理> # 順次処理
run-p <1つ目のタスク> <2つ目の処理> # 並列処理
```

## ダウンロード数比較
- [johnmpotter/npm-trends](https://github.com/johnmpotter/npm-trends)
