# [Git](https://git-scm.com)とは
プログラムのソースコードなどの変更履歴を記録・追跡するための分散型バージョン管理システムのこと.
とっても便利.
慣れてしまうともう後戻りはできない.

## 新しいブランチの作成
### ローカルブランチからブランチ作成
```bash
cd ディレクトリ // 作業ディレクトリに移動
git branch -a // branchの一覧を表示
git checkout master // master branchに切り替え
git checkout -b 作成するbranch名 // masterを元にして新しいbranchを作成
git branch -a
git push -u origin 作成したbranch名 // branchをremoteに登録
```

### リモートブランチからブランチ作成
```bash
cd ディレクトリ
git checkout -b ローカルに作成するブランチ名 origin/再生元のリモートブランチ名
git branch -a
```


## ブランチの削除
```bash
cd 作業ティレクトり
git checkout -b master // 削除したいブランチとは違うブランチにまずは移動する
git checkout -d 削除したいブランチ名
```

# Gitの基本操作

## gitの構成
- 作業ディレクトリ
- ステージングエリア（インデックス）
- リポジトリ（ローカル, リモート）

## gitの設定
```bash
# gitへユーザー名を登録
git config --global user.name "solareenlo"

# gitへメールアドレスを登録
git config --global user.email "solareenlo@test.com"

# gitを色づける
git config --global color.ui true

# commitしたときのエディタをvimに設定する
git config --global core.editor vim

# gitの設定一覧を見る
git config -l

# gitのhelpを見る
git config --help
```

## commit
```bash
# gitの初期化
git init

# 作業ディレクトリからステージングエリアへ
git add index.html

# ステージングエリアからリポジトリへ
git commit
# タイトルとか変更内容を書いて, :wq

# 変更のlogを見る
git log
```

## log
```bash
# logをコンパクトに見る
git log --oneline

# logと共に変更内容も見る
git log -p

# どのファイルが変更なったかを見る
git log --stat
```

## 現在の状態を把握する
```bash
# 現在の状態を見る
git status

# ファイルの変更を無かったことにする
git checkout -- index.html
```

## 差分を確認する
```bash
# ファイルの差分を見る
git diff

# ステージングされたファイルの差分を見る
git diff --cached
```

## gitでのファイル操作
```bash
# 現在のディレクトリ以下全部のファイルをaddする
git add .

# gitでのファイル削除
git rm index.html

# gitでのファイル移動
git mv index.html
```

## gitの管理に含めない方法
```bash
# .gitignoreファイルを作り, その中にgitに含めないファイルを記述する
vim .gitignore
*.log

# .gitignoreがある場所から遡ってはignoreされない.
# ので, サブディレクトリだけ.gitignoreしたい場合は, サブディレクトリ内に.gitignoreを入れておく.
```

## 直前のcommitを変更
```bash
# 1つcommitする
git commit -m 'コミットを1つ追加'

# 直前のcommitを上書きする
vim index.html
git add .
git commit --amend
```

## 過去のバージョンに戻る（１）
```bash
# 作業ディレクトリもステージングエリアも直前のcommitへ戻す
git reset --hard HEAD

# 2つ前のcommitに戻る
git reset --hard HEAD^

# 指定したidにcommitを戻す
git reset --hard hash値(最低7桁)
```

## 過去のバージョンに戻る（２）
```bash
# commit変更したけど, やっぱり変更前のcommitに戻りたいときは
git reset --hard ORIG_HEAD
# ORIG_HEADには前回取り消されたHEADの情報が1つだけ入っている.
```

## branch
```bash
# 現在のbranchを確認する
git branch

# 新しいbranch hogeを作成する
git branch hoge

# hoge branchに移動する
git checkout hoge
git branch

# master branchに戻る
git checkout master
git branch
```

## branchをmerge
```bash
# master branchにhoge branchをmergeさせたい
git checkout master
git merge hoge

# いらなくなったhoge branchを削除する
git branch -d hoge

# masterにmerge済みのbranchを確認する
git checkout master
git branch --merged

# masterにまだmergeしていないbranchを確認する
git checkout master
git branch --no-merged

# 作業途中でmergeしたときに, まだcommitしたくないときは, stashを使って一時的に変更内容を退避させる.
git stash save 'コメント'

# stashで退避させた情報の一覧表示
git stash list

# 退避させるたびに, この一覧の上に退避データが積み上がっていく.
# 対比した内容を取り出すには
git stash pop
# 引数をつけない場合, listで表示された一番上の内容が取り出される.
```

## mergeの衝突を解消
```bash
# branch切って, すぐにhogeに移動する
git checkout -b hoge
vim index.html
git add .
git commit -m 'hogeで変更したよ'
git branch master
vim index.html
git add .
git commit -m 'masterでも変更したよ'
git merge hoge
> CONFLICT (content): merge conflict in index.html
vim index.html
# index.htmlのいらないところを削除するだけ
# hogeかmasterのどちらかの変更を残すということ.
git add .
git comit -m 'conflictを解消したよ'
```

## tag
```bash
# 直前のcommitにidに代わるわかりやすいv1.0というtagを付ける
git tag v1.0

# tagを確認する
git tag

# tagを使ってcommit内容を確認する
git show v1.0

# 特定のcommitにtagを付ける
git tag v0.9 id(hash値)

# tagの削除
git -d v0.9
```

## エイリアス
```bash
git config --global alias.co checkout
git config --global alias.st status
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.cm 'commit -m'
git config --global alias.a add
# エイリアスの確認
git config --global --list | grep ^alias\.
# 設定一覧
git config -l
```

## 初めての共同作業
```bash
# 共有リポジトリを作成
# 共有リポジトリには.gitを付けるのが通例
mkdir ourweb.git
cd ourweb.git
git init --bare
# これで管理ファイルだけが管理され, commitとかはしない設定になる.

# 別のリポジトリを現在のリポジトリにoriginとして登録する
git remote add origin ~/path/to/ourweb.git
git config -l
> remote.origin.url=/home/path/to/ourweb.git

# 登録した別のリポジトリ(origin)を削除する
git remote rm origin

# 現在のリポジトリ(master)の内容をremoteしたディレクトリ(origin)にpushする
git push origin master
# originにmasterをpushする.という意味.

# ourweb.gitをmyweb2としてcloneして作業する
git clone ~/path/to/ourweb.git myweb2
cd myweb2
git log
vim index.html
git add .
git commit -m 'line2 added'
git push origin master
# 変更内容(master)をoriginにpushした

# 別のユーザーが上記の変更をpullする
git pull origin master
git log
```

## 共同作業でコンフリクトが起こったら
```bash
# Aさんが先に変更を加えた
vim index.html
git commit -am 'line 3 added'
git push origin master

# Bさんも後から同様の場所に変更を加えた
vim index.html
git commit -am 'line third added'
git push origin master
> CONFLICT (content): Merge conflict in index.html
# pushする前にpullしてまずはconflictを解消する
git pull origin master
vim index.html
git commit -am 'conflict fixed'
git push origin master
```

## git rev-parse
機能がたくさんあるが, ここではHEADのダイジェスト返してくれる方法.
```bash
git rev-parse HEAD
```
