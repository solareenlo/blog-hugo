# GitHub とは
- ソフトウェア開発のプラットフォーム兼ソースコードなどのホスティングサービス．
- https://github.com

## GitHub と GitLab 同時に push
先ずは普通に GitHub と GitLab にリポジトリを作成し，リモートリポジトリを追加する．
```bash
# GitLab のリポジトリをリモートに追加
git remote set-url --add origin git@gitlab.com/solareenlo/リポジトリ名.git
# 確認
git remote -v
> origin  git@github.com:solareenlo/リポジトリ名.git (fetch)
> origin  git@github.com:solareenlo/リポジトリ名.git (push)
> origin  git@gitlab.com:solareenlo/リポジトリ名.git (push)
# 以下を行うと GitHub と GitLab に同時に push される．
git push -u origin master
```

## リポジトリに他のリポジトリをリンク付けする
```bash
git submodule add -b <リンク付けする方のブランチ名> <リンク付けする方の URL> <リンク付けされるディレクトリ名>
```

## リポジトリを fork して更新する
fork -> clone -> remote -> fetch -> marge -> push

## ssh 接続ができなくなったときは
`~/.ssh/config`の中身を
```bash
Host github github.com
  Hostname github.com
  Port 22
  User git
  IdentityFile ~/.ssh/id_git_rsa
```
のように，Host のところに `github.com` を追加してみる．

## スライドショー
- [hiroppy/fusuma](https://github.com/hiroppy/fusuma)
- [hakimel/reveal.js](https://github.com/hakimel/reveal.js)
- [gnab/remark](https://github.com/gnab/remark)
- [gitpitch/gitpitch](https://github.com/gitpitch/gitpitch)

## チートシート
- [Git Cheat Sheets](https://github.github.com/training-kit/)

# GitHub Pages とは
- GitHub の静的サイトホスティングサービスのこと．

## 静的サイトを公開する
```bash
cd 作業ディレクトリ名
git checkout -b gh-pages
vim index.html // index.htmlを作成
git push -u origin gh-pages
```
gh-pages という名前のブランチに index.html ファイルを作っておけば，それが静的サイトとして「https://ユーザー名.github.io/リポジトリ名」として公開される．

## サブドメインを割り当てる
- GitHub Pages でサブドメインを割り当てる時は CNAME を使用する．

|ホスト名|TYPE|TTL|VALUE|
|---|---|---|---|
|docs.iotajapan.com|CNAME|3600|iotajapan.github.io|
