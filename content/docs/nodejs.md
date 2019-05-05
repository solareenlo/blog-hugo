# Node.jsとは
サーバーサイドJavaScript環境のこと.
サーバーサイドもJavaScripで記述できるようにした.

## インストール
**Mac:** https://nodejs.org/en/

**Ubuntu:** [Official Node.js binary distributions](https://github.com/nodesource/distributions/blob/master/README.md)

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
