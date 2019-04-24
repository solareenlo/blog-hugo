# [GitHub](https://github.com)とは
ソフトウェア開発のプラットフォーム兼ソースコードなどのホスティングサービス.

## GitHub Pagesで静的サイトを公開する
```bash
cd 作業ディレクトリ名
git checkout -b gh-pages
vim index.html // index.htmlを作成
git push -u origin gh-pages
```
gh-pagesという名前のブランチにindex.htmlファイルを作っておけば, それが静的サイトとして「https://ユーザー名.github.io/リポジトリ名」として公開される.

## リポジトリに他のリポジトリをリンク付けする
```bash
git submodule add -b <リンク付けする方のブランチ名> <リンク付けする方のURL> <リンク付けされるディレクトリ名>
```

## リポジトリをforkして更新する
fork -> clone -> remote -> fethc -> marge -> push

## ssh接続ができなくなったときは
`~/.ssh/config`の中身を
```bash
Host github github.com
  Hostname github.com
  Port 22
  User git
  IdentityFile ~/.ssh/id_git_rsa
```
のように, Hostのところに`github.com`を追加してみる.


## チートシート
- [Git Cheat Sheets](https://github.github.com/training-kit/)
