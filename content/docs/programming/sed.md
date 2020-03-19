# sedとは
- 入力ストリーム（ファイルまたはパイプラインからの入力）に対してテキスト変換などのデータ処理をおこなうプログラム.
- 名称「sed」は「ストリームエディタ」を意味する英語「stream editor」から.

## .gitignoreの/distを削除
```bash
sed -i -e "/\/dist/d" .gitignore
# -i: 上書き保存
# -e: 行の削除
# d: 行目を表す(上の例だと\distを含む行)
```

## 初めてのsed
```bash
cat names.txt
> 1 taguchi
> 2 koji
> 3 taro
> 4 hanako
> 5 yasuda

# 3行目を削除して標準出力する
sed -e '3d' names.txt
# 3dのところが1つしかない場合は-eを省略できる
sed '3d' names.txt
> 1 taguchi
> 2 koji
> 4 hanako
> 5 yasuda

# sedして上書きしたい場合
sed -i '3d' names.txt

# 上記をバックアップを取りながらしたい場合
sed -i.bak '3d' names.txt
cat names.txt
> 1 taguchi
> 2 koji
> 4 hanako
> 5 yasuda

ls
> names.txt names.txt.bak
cat names.txt.bak
> 1 taguchi
> 2 koji
> 3 taro
> 4 hanako
> 5 yasuda
mv names.txt.bak names.txt
ls
> names.txt

# ファイルを読み込んでsedする
vim ex1.sed
3d # と書き込む
:wp
sed -f ex1.sed names.txt
> 1 taguchi
> 2 koji
> 4 hanako
> 5 yasuda

# パイプとsedを用いてディレクトリ情報の初めの3行を削除して標準出力
ls -la | sed '1,3d'
> ディレクトリ情報の初めの3行が削除された形で標準出力される

# さらにリダイレクト(>)も使って, 上記をファイルに出力する
ls -la | sed '1,3d' > output.txt
```

## パターンスペースについて
sedはパターンスペースを用いて操作を行っている.  
どういうことかというと,  
`sed '3d' names.txt'`  
は, 3がaddress, dが削除commandの意味で,  
1. fileから1行目を読み込んでパターンスペースに格納  
2. addressにマッチする？ → commandを実行  
3. パターンスペースを表示  
という流れになっている.

## アドレスを使いこなす
```bash
cat names.txt
> 1 taguchi
> 2 koji
> 3 taro
> 4 hanako
> 5 yasuda

# 3行目以外を削除
sed '3!d' names.txt
> 3 taro

# 1行目と3行目を削除
sed '1d;3d' names.txt
> 2 koji
> 4 hanako
> 5 yasuda

# 1行目から3行目までを削除
sed '1,3d' names.txt
> 4 hanako
> 5 yasuda

# 1行目を削除してから2行飛ばしで削除.
# つまり, 奇数行目を削除.
sed '1~2d' names.txt
> 2 koji
> 4 hanako

# 最後の行を削除
sed '$d' names.txt
> 1 taguchi
> 2 koji
> 3 taro
> 4 hanako

# 3行目から最後の行までを削除
# $が最後の意味
sed '3,$d' names.txt
> 1 taguchi
> 2 koji

# iで終わる行は削除
sed '/i$/d' names.txt
> 3 taro
> 4 hanako
> 5 yasuda

# addressは省略もできるのでaddressを書かないと全削除
sed 'd' names.txt
```

## p/a/iコマンドを使う
```bash
cat names.txt
> 1 taguchi
> 2 koji
> 3 taro
> 4 hanako
> 5 yasuda

# 3行目を2回表示する
sed '3p' names.txt
> 1 taguchi
> 2 koji
> 3 taro
> 3 taro
> 4 hanako
> 5 yasuda

# 3行目だけを表示する
sed -n '3p' names.txt
> 3 taro

# 3行目で処理を終わる
sed '3q' names.txt
> 1 taguchi
> 2 koji
> 3 taro

# 1行目の前に文字列をinsertする
sed '1i\--- start ---' names.txt
> --- start ---
> 1 taguchi
> 2 koji
> 3 taro
> 3 taro
> 4 hanako
> 5 yasuda

# 最終行にも文字列を挿入する
sed -e '1i\--- start ---' -e '$a\--- end ---' names.txt
> --- start ---
> 1 taguchi
> 2 koji
> 3 taro
> 3 taro
> 4 hanako
> 5 yasuda
> --- end ---

# 最終行の空の文字列を削除する
# /^$/ は正規表現で空行を表す
sed '/^$/d' names.txt
> 1 taguchi
> 2 koji
> 3 taro
> 3 taro
> 4 hanako
> 5 yasuda
```

## 1文字ずつの文字置換にはyコマンドを使う
```bash
cat names.txt
> 1 taguchi
> 2 koji
> 3 taro
> 4 hanako
> 5 yasuda

# 1文字置換はyを使い,
# tをTに置換する.
sed 'y/t/T/' names.txt
> 1 Taguchi
> 2 koji
> 3 Taro
> 4 hanako
> 5 yasuda

# 複数個の文字を1文字ずつ置換する
# t -> T, o -> O
# という風に1文字ずつ対応して置換してくれる.
sed'y/to/TO/' names.txt
> 1 Taguchi
> 2 kOji
> 3 TarO
> 4 hanakO
> 5 yasuda
```

## 文字列置換にはsコマンドを使う
```bash
cat items.txt
> 1 taguchi Apple, apple, apple, grape
> 2 fkoji Banana, apple, Apple, lemon
> 3 dotinstall Grape, apple, strawberry
> 4 takahashi cherry, pear, kiwi
> 5 yasuda cherrry, Cherry

# 上記のitems.txtのappleをAppleに置換する
# ただし, 初めの1つにだけしか効きません
sed 's/apple/Apple' items.txt
> 1 taguchi Apple, Apple, apple, grape
> 2 fkoji Banana, apple, Apple, lemon
> 3 dotinstall Grape, Apple, strawberry
> 4 takahashi cherry, pear, kiwi
> 5 yasuda cherrry, Cherry

# なので, 全てのappleをAppleに変えたいときは
# gというフラグを用いる
sed 's/apple/Apple/g' items.txt
> 1 taguchi Apple, Apple, Apple, grape
> 2 fkoji Banana, Apple, Apple, lemon
> 3 dotinstall Grape, Apple, strawberry
> 4 takahashi cherry, pear, kiwi
> 5 yasuda cherrry, Cherry

# 2番目にマッチするものを置換するときは
# 2というフラグを用いる
sed 's/apple/Apple/2' items.txt
> 1 taguchi Apple, apple, Apple, grape
> 2 fkoji Banana, Apple, Apple, lemon
> 3 dotinstall Grape, Apple, strawberry
> 4 takahashi cherry, pear, kiwi
> 5 yasuda cherrry, Cherry

# 大文字小文字を区別させないときは
# iというフラグを用いる
# フラグは組み合わせることもできる
sed 's/apple/Ringo/ig' items.txt
> 1 taguchi Ringo, Ringo, Ringo, grape
> 2 fkoji Banana, Ringo, Ringo, lemon
> 3 dotinstall Grape, Ringo, strawberry
> 4 takahashi cherry, pear, kiwi
> 5 yasuda cherrry, Cherry

# 正規表現も使えます
sed 's/[aA]pple/Ringo/ig' items.txt
> 1 taguchi Ringo, Ringo, Ringo, grape
> 2 fkoji Banana, Ringo, Ringo, lemon
> 3 dotinstall Grape, Ringo, strawberry
> 4 takahashi cherry, pear, kiwi
> 5 yasuda cherrry, Cherry
```

## &や\1を使って置換する
```bash
cat items.txt
> 1 taguchi Apple, apple, apple, grape
> 2 fkoji Banana, apple, Apple, lemon
> 3 dotinstall Grape, apple, strawberry
> 4 takahashi cherry, pear, kiwi
> 5 yasuda cherrry, Cherry

# 検索した結果は&に入る
sed 's/[0-5]/【&】/' items.txt
> 【1】 taguchi Apple, apple, apple, grape
> 【2】 fkoji Banana, apple, Apple, lemon
> 【3】 dotinstall Grape, apple, strawberry
> 【4】 takahashi cherry, pear, kiwi
> 【5】 yasuda cherrry, Cherry

# 複数個検索した結果を用いる場合は
# ()で包んでから, \1, \2...を使う
sed 's/\([0-5]\) \(.*\)/\2 【\1】/' items.txt
# 1-5までの数字は\1に, 文字列は\2に入っている
> taguchi Apple, apple, apple, grape【1】
> fkoji Banana, apple, Apple, lemon【2】
> dotinstall Grape, apple, strawberry【3】
> takahashi cherry, pear, kiwi 【4】
> yasuda cherrry, Cherry 【5】
```

## ホールドスペースを使う
ホールドスペースとはパターンスペースのさらに奥にある裏バッファーのようなもの  

- h(hold): パターンスペースの内容をホールドスペースにコピー
- g(get): ホールドスペースの内容をパターンスペースにコピー
- x(exchange): パターンスペースとホールドスペースの内容を交換

パターンスペースは1行ずつどんどん更新されていく.  
ホールドスペースに一時的に情報を格納しておく.  
そうすることでより複雑な文字列操作を行う.
```bash
cat style.css
> main {
>   color: red;
>   font-weight: bold;
>   font-size: 14px;
>   background: green;
> }

vim ex2.sed
i
# color change
/color: / {
  h // holdが起こり, ホールドスペースに color: red; がコピーされる
  s/color: /background: / // 置換が起こり, パターンスペースで, color: red; が background: red; に置換される
  x // exchangeが起こり, ホールドスペースに background: red; が, パターンスペースに color: red; が格納される
} // パターンスペースの color: red; が表示される
/background: / {
  g // getが起こり, ホールドスペースにある background: red; が, パターンスペースにコピーされる
} // パターンスペースの backgroud: red; が表示される
ESC
:wq

sed -f ex2.sed style.css
> #main {
>   color: red;
>   font-weight: bold;
>   font-size: 14px;
>   background: red;
> }
```
