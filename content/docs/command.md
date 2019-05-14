# コマンドとは
コンピュータに特定の機能の実行を指示する命令のこと.
たくさんのコマンドがあり, 黒い画面に直接入力する.

## psコマンド
psコマンドはプロセスの動作状況を確認するためのコマンド.

**auxオプション**  
auxオプションは、aとuとxというオプションを組み合わせたものです。

- a: 端末操作のプロセスを表示する
- u: CPUやメモリの使用率などを表示する
- x: 端末操作以外のプロセスを表示する

```bash
ps aux
> USER               PID  %CPU %MEM      VSZ    RSS   TT  STAT STARTED      TIME COMMAND
> solareenlo       96263   4.3  2.0  5500012 168620   ??  S    水07PM  21:03.04 /Applications/Utilities/Terminal.app/Contents/MacOS/Terminal
> その他たくさん
```
こんな感じで標準出力される.

**Reference:** [ps auxの見方がよく判らない・・・](https://usado.jp/spdsk/2018/03/26/post-3479/)

## sha256を出力
```bash
# Mac
shasum -a 256 <ファイル名>

# Linux
sha256sum <ファイル名>
```

## ファイルのユーザー所有権とグループ所有権を変更
ファイルのユーザー所有権とグループ所有権をrootからsolareenloへ変更する
```bash
sudo chown solareenlo:solareenlo ファイル名
```

# 基本的なUnixコマンドの使い方

## mkdirを使って一気にファイルを複数作成する
```bash
# app1, app2, ... app39, app40というフォルダを一斉に作成する方法
mkdir app{1..40}
```

## 一般ユーザーと管理ユーザー
```bash
# 一般ユーザーだと
[solareenlo@localhost ~]$

# 管理ユーザーだと
[solareenlo@localhost ~]#
```

## UNIXシステムのディレクトリの大体の構成
- binには, バイナリデータが入ってる.
- binには, catやcpといった, システムを動かすための基本コマンドの実行ファイルがある.
- etcには, アプリケーションの設定ファイルが入っている.
- homeには, 各ユーザーのホームディレクトリが入ってる.
- sbinには, システム管理用のコマンドが入っている.
- usrには, 一般的なアプリケーションが格納されてる.
- varには, システムのログファイルなどの頻繁に書き換わるようなファイルが入ってる.

## ディレクリを移動
```bash
# 現在いるディレクトリを表示
pwd

# 画面をきれいにするには
clear
# Control + l

# ディレクトリへ移動
cd path/to/directory/

# 1つ上の階層へ戻る
cd ..

# 直前のディレクトリへ戻る
cd -

# ホームディレクトリへ戻る
cd
```

## ディレクトリを操作
```bash
# ディレクトリを新規作成
mkdir test

# ディレクトリ一覧表示
ls

# ディレクトリをコピー
cp -r test test2

# test3ディレクトリの中にconfigディレクトリを一気に作成する
mkdir -p test3/config

# test3ディレクトリをtest2ディレクトリに移動する
mv test3 test2

# 空のディレクトリを削除する
rmdir test2/test3/config

# 空でないディレクトリを削除する
rm -r test2
```

## ファイル操作
```bash
# file.txtの中身を全部一斉に見る
cat path/to/file.txt

# file.txtの中身を少しずつ見るには
less path/to/file.txt
# 矢印キーでスクロール
# space で下へ移動
# Control + F で一画面先へ移動
# Control + B で一画面前へ移動
# g でファイル先頭へ移動
# G でファイル末尾へ移動
# q で終了

# 現在のディレクトリにファイルコピー
cp path/to/file.txt .

# 現在のディレクトリにあるファイルを名前変更
mv file.txt file2.txt

# ファイルを削除
rm file2.txt
```

## bashの便利機能
```bash
# 操作をとりあえずキャンセル
# CTRL + C

# コマンドの履歴検索
# CTRL + R
# そこで何かしら前に使ったコマンドを途中まで入力してtabキーを押して補完して, エンターキーを押すと実行される
```

## コマンドの履歴
```bash
# 今までに使用したコマンドの履歴を表示する
history

# 出てきた履歴を使う
!333

# 直前のコマンドを使う
!!

# 2個前のコマンドを使う
!-2

# コマンドに渡した最後の文字列を再利用するには!$を使う
ls test/
cd !$

# 履歴の予測を使う
!pw
# 上記のコマンドで, 今までに使ったことがあるpwdを実行してくれる

# 記憶があやふやな場合は
!pw:p
# で, 直近に使ったコマンド予測が実行されずに帰ってくる

# !を使ってコマンドの履歴にアクセスできる.
```
## コマンドについて詳しく調べる
```bash
# mkdirについて調べる
mkdir --help

# より詳しく調べる
man mkdir

# マニュアル内での検索は
/all
# のように/を使って行う.
# 後の操作はvimと一緒.
```

## シンボリックリンクの使い方
```bash
# シンボリックリンクの付け方
mkdir -p config/production/database
ln -s config/production/database/ dbconfig
ls -l

# シンボリックリンクを解消する
unlink dbconfig
```

## ユーザーとグループを確認
```bash
# ユーザーを確認する
cat /etc/passwd
> solareenlo:x:1000:1000:solareenlo,,,:/home/solareenlo:/bin/bash
# ユーザー名:パスワード有り:ユーザーID:グループID:グループ名,,,:ホームディレクトリ:シェルの場所

# グループを確認する
cat /etc/group
> solareenlo:x:1000:
# グループ名:パスワード有り:グループID:所属するユーザー名

# 自分が所属しているグループ名を表示
groups
```

## パーミッションの変更
```bash
# 一番はじめの状態が
ls -l
> -rwxrw-r--. 1 solareenlo solareenlo 日付 sample
# としよう.
# rwxrwxrwx の並びは, user group other の並びになっている.

# groupに実行権限を付与する
chmod g+x sample
> rwx rwx r--

# groupとotherに実行権限を与える
chmod go+x sample
> rwx rwx r-x

# user, group, other全員から実行権限を剥奪する
chmod a-x sample
> rw- rw- r--
```

## 数値によるパーミッションの変更
```bash
# rwx = 4+2+1 = 7
# と, 2進数の和で表現される.
# ので,
rwx rw- r-- = 4+2+1, 4+2+0, 4+0+0 = 764
# ので,
chmod 774 sample = rwx rwx r--
chmod 664 sample = rw- rw- r--
# となる
```

## 独自のコマンドを作成
```bash
# 先ずはコマンドが先に存在しないことを確認する
type hi
> -bash: type: hi: not found
# で, OK

# hi!と出力されるコマンドをvimで作成する
vim hi
#!/bin/bash
echo 'hi!'
:wq

# hiコマンドの実行権限を見てみる
ls -l
> -rw-rw-r-- solareenlo solareenlo 日付 hi
# 実行権限が無いので付与する
chmod u+x hi
ls -l
> -rwx-rw-r-- solareenlo solareenlo 日付 hi
./hi
> hi!
```

## 一時的にpathを通す
```bash
pwd
> /home/現在の/path/test
export PATH=/home/現在の/path/test:$PATH

# どこにpathが通っているのかを見るには
echo $PATH
> /anaconda3/bin:/usr/local/git/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/home/現在の/path/test
# いろいろなpathが通っているディレクトリが見れる

hi
> hi!
# この方法は一時的なpathの通し方

# 実行しようとしているコマンド(hi)がどこから呼び出されているのかを探るには
which hi

# 全体の環境変数を見るには
printenv
> TERM_PROGRAM=Apple_Terminal
> ANDROID_HOME=/Users/solareenlo/Library/Android/sdk
> TERM=xterm-256color
> SHELL=/bin/bash
> CLICOLOR=1
# こんな感じで出てくる
echo $PATH
> /anaconda3/bin:/usr/local/git/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:/home/現在の/path/test
# 環境変数に$を付けると, その値を意味する
```

## 管理者ユーザーになる
```bash
ls -l /var/log/boot.log
> -rw------- 1 root root 27000 日付 /var/log/boot.log
cat !$
> cat /var/log/boot.log
> cat: /var/log/boot.log: 許可がありません
# となるので,
# rootユーザーに切り替える
# 現在のディレクトリにログイン
sudo su
> Password:
# か,
# ホームディレクトリにログイン
sudo su -l
> Password:
# か,
sudo su -
> Password:

pwd
> /root
cat /var/log/boot.log
> きちんと中身が見れる

# rootユーザーを抜ける
exit

# rootユーザーは何でもできるので, うっかりで大変なことになるので,
# 実際はsudoで1つ1つrootユーザー権限でコマンドを実行する.
sudo cat /var/log/boot.log
> きちんと中身が見れる
```

## 管理者を変更するchown
```bash
cp /var/log/boot.log .
> cp: '/var/log/boot.log' を読み込み用に開くことができません: 許可がありません
sudo !!
> sudo cp /var/log/boot.log .
ls -l
> -rw------- 1 root root 27000 日付 boot.log
cat boot.log
> cat: boot.log: 許可がありません
# boot.logのオーナーを変更する
sudo chown solareenlo:solareenlo boot.log
> -rw------- 1 solareenlo solareenlo 27000 日付 boot.log
cat boot.log
> boot.logの中身が見れる
```

## wc, head, tail, grepを使う
```bash
# ファイルの行数, 単語数, 文字数をカウントしてくれるwc
wc boot.log
> 490 3397 27077 boot.log

# ファイルの行数だけ表示
wc -l boot.log
> 490 boot.log

# ファイルの先頭10行をcatしたい時
head boot.log
> 先頭の10行だけが表示される

# ファイルの先頭3行だけを表示したい時
head -n 3 boot.log
head -3 boot.lgo
> 先頭の3行だけが表示される

# ファイルの末尾から10行を表示する
tail boot.log

# ファイルの末尾から3行を表示する
tail -n 3 boot.log
tail -3 boot.log

# 特定の単語を検索する
# 正規表現ももちろんできる
grep 'etc' boot.log
> 検索結果が返ってくる
```

## リダイレクション, パイプを使う
```bash
# コマンドの実行結果をテキストファイルに出力する
echo 'date' > cmd.txt
cat cmd.txt
> date
# cmd.txtの中にdateが書き込まれた

# コマンドの実行結果をテキストファイルの末尾に付け加える
echo 'free' >> cmd.txt
cat cmd.txt
> date
> free

# 逆にテキストファイルの中身をコマンドに渡して実行する
bash < cmd.txt
> 日付
> メモリの使用状況

# テキストファイルの中身をコマンドに渡した実行結果をテキストファイルに書き込む
bash < cmd.txt > result.txt
cat result.txt
> 日付
> メモリの使用状況

# コマンドの実行結果をパイプを使ってコマンドに渡す
ls -l /etc | grep 'bash'
> bashを含むls -l /etcの結果が返ってくる

# さらに, 上記の結果の行数を調べる
ls -l /etc | grep 'bash' | wc -l
> 3
```

## ワイルカード
```bash
# 任意の文字列を扱う*を使って, 拡張子が.confを表示する
ls /etc/*.conf
> 拡張子が.confのものだけが表示される

# cから始まり, 2文字だけ何でも良くて, 拡張子も何でもいい場合は?と*を使う
ls /etc/c??.*
> 当該条件に合致するものが表示される
```

## find, xargs
```bash
# ファイルを検索してくれるfind
# find 場所 名前で検索 条件(initのあとは何でも良い)
find /etc -name 'init*'
> 当該ファイルが出力される

# ディレクトリを覗いてファイルだけ検索したい場合
find /etc -name 'init*' -type f

# さらに, 検索したファイルに対して別のコマンドを実行したい場合は, -exec wc -l {} + を使う.
# {}の中に検索結果を入れて実行してくれる.
find /etc -name 'init*' -type f -exec wc -l {} +
> 当該ファイルの行数がカウントされて表示される

# 複数の結果をコマンドに流し込めるxargsもある
find /etc -name 'init*' -type f | xargs wc -l
> 当該ファイルの行数がカウントされて表示される
```

## ブレース展開
```bash
# まとめて展開
echo {a, b, c}
> a b c
echo {1..10}
> 1 2 3 4 5 6 7 8 9 10
echo {1..3}{a..c}
> 1a 1b 1c 2a 2b 2c 3a 3b 3c

# 連続してコマンドを実行するには&&を使う
mkdir test && cd test

# ブレース展開を使って一気にディレクトリを作成する
mkdir app{1..3}
ls
> app1 app2 app3

# ブレース展開を使って一気にディレクトリの中にファイルを作成する
touch app{1..3}/test{1..3}{.txt,.jpeg,.git}
ls app2
> test1.txt test1.jpeg test1.git test2.txt test2.jpeg test2.git test3.txt test3.jpeg test3.git

# 上記の中から特定の拡張子のファイルだけ削除する
rm app{1..3}/test{1..3}{.jpeg,.git}
ls app2
> test1.txt test2.txt test3.txtt
```
