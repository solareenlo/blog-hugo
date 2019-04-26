# シェルスクリプトとは
シェルとはOSと対話するためのインターフェースであり, それのスクリプトがシェルスクリプト.  
シェルスクリプトモードで実行するのと`filename.sh`を作って,
```bash
bash filename.sh
```
で実行する2パターンあります.

## シェルスクリプトモードに突入
```bash
/bin/sh
$
```
または
```bash
sh
$
```

## Hello World
```bash
sh
$ echo Hello World
Hello World
$
```

## シェルスクリプトモードから脱出
`CTL`と`d`を押す.

## 初めてのシェルスクリプト
```bash
# まず, bashがどこにあるか調べる
which bash
> /bin/bash
```
- #!のことを「シェバン」もしくは「シバン」という.
- shell scriptファイルの頭に`#! /bin/bash`と書いてあげる.

```bash
vim hello
i
#i /bin/bash
echo hello
Esc
:wq

ls -l
> -rw-rw-r--. 1 solareenlo solareenlo 24 日付 hello
chmod +x hello
ls -l
> -rwxrwxr-x. 1 solareenlo solareenlo 24 日付 hello
bash hello
> hello
```

## 文字列を表示
```bash
# helloの中身
#!/bin/bash
# コメントは#を使うと文末までコメントアウトしてくれる
echo "hello world"
echo 'hello world'
echo "foo"; echo "bar";
```
上記を実行すると
```bash
bash hello
> hello world
> hello world
> foo
> bar
```

## 変数
```bash
# 02_variabelの中身
#!/bin/bash
# 変数を使う

# name = "taguchi" はエラーになる. =の両脇にスペースを入れるとエラーになる.
name="taguchi"
readonly name2="taro" # 読み取り専用になる
# name2="hanako" エラーになる

echo "morning $name"
echo "hello $name"
echo "by ${name}san"
echo 'by ${name}san' # シングルクォーテーションだと${name}がそのまま表示される
echo "hello $name2"
```
上記を実行すると
```bash
bash 02_variable
> morning taguchi
> hello taguchi
> by taguchisan
> by ${name}san
> hello taro
```

## 特殊変数
```bash
# シェルスクリプトにおける特殊変数の定義.
$1 = 第1引数
$2 = 第2引数
$0 = コマンド名
$# = 引数の個数
$@ = 全部の引数
```
```bash
# 03_variableの中身
#!/bin/bash

# 第１引数 = $1, 第2引数 = $2, ...
echo "hello $1"

# ./03_variable a aa aaa
echo $0 # ./03_variable
echo $# # 3 # = 引数の個数
echo $@ # a aa aaa @ = 引数全部
```
上記を実行すると
```bash
bash 03_variable a aa aaa
> hello a
> bash 03_variable
> 3
> a aa aaa
```

## ユーザーに入力してもらう
```bash
# 1.
#!/bin/bash
read name
echo "hello $name"

# 上記を実行すると
bash 04_read taro
> hello taro

# 2.
#!/bin/bash
read -p "Name: " name
echo "hello $name"

# 上記を実行すると
bash 04_read
> Name:
taro
> hello taro

# 3.
#!/bin/bash
read -p "Pick 3 colors: " c1 c2 c3
echo $c1
echo $c2
echo $c3

# 上記を実行すると
bash 04_read
> Pick 3 colors:
red green blue
> red
> green
> blue

# 色を多く入力したり, 少なく入力すると
# 多い場合はc3に複数個の文字列が代入され,
# 少ない場合はc3が表示されない.
```


## 配列
```bash
# 05_arrayの中身
#!/bin/bash
# 配列

colors=(red green blue)
echo ${colors[0]} # red
echo ${colors[1]} # green
echo ${colors[2]} # blue
echo ${colors[@]} # red green blue
echo ${#colors[@]} # 3

# 配列の要素に代入
colors[1]=silver
# 配列の末尾に代入
colors+=(white black)
echo ${colors[@]} # red silver blue white black
echo ${#colors[@]} # 5
```
上記を実行すると
```bash
bash 05_array
> red
> green
> blue
> red green blue
> 3
> red silver blue white black
> 5
```

## 数値演算
```bash
#!/bin/bash

echo 5+2
# > 5+2

echo `expr 5 + 2`
# > 7

echo $((5 + 2))
# > 7

n=5
echo $((n=n+5))
# > 10
# (($n=$n+5))とnに$を$を付ける必要はないです

echo $n
# > 10

# 演算子は
# + - * / % ** ++ --

# bashでは実数の計算はできない.
echo $((10/3))
# > 3
```

## if分
```bash
#!/bin/bash

read -p "Name? " name
# if test "$name" = "taro"
if [ "$name" = "taro" ]
then
  echo "welcome"
elif [ "$name" = "hanako" ]
then
  echo "welcome too"
else
  echo "you are not allowed"
fi

# 以下のように; thenという書き方もあります.
if [ "$name" = "taro" ]; then
  echo "welcome"
elif [ "$name" = "hanako" ]; then
  echo "welcome too"
else
  echo "you are not allowed"
fi
```
上記を実行すると
```bash
bash 07_if
> Name?
taro
> welcom
```
または
```bash
bash 07_if
> Name?
hanako
> welcome too
```
または
```bash
bash 07_if
> Name?
test
> you are net allowed
```

## 文字列の比較
[08_compare_strings](https://github.com/solareenlo/shellscript-practice/blob/master/08_compare_strings)の中身をご覧ください.
```bash
#!/bin/bash
# 文字列の比較には[]も使われますが,
# 最近では[[]]がよく使われる.

read -p "Name? " name
if [ "$name" = "taro" ]; then
  echo "welcome"
fi

# と書くところを,
if [[ $name = "taro" ]]; then
  echo "welcome"
fi
# と書く.

# = or == が文字列が等しいかどうかを調べる
# != が文字列が等しくないかどうかを調べる
# -z が文字列が0かどうかを調べる
# =~ が正規表現を使う

if [[ -z $name ]]; then
  echo "Name is empty ..."
fi
# 上記を実行すると,
# bash 08_compare_strings
# > Name?
# ENTER
# > Name is empty ...
# となる.

if [[ $name =~ ^t ]]; then
  echo "Name is starting with t..."
fi
# 上記を実行すると,
# bash 08_compare_strings
# > Name?
# taro
# > Name is starting with t...
# となる.
```

## ファイルや数値の比較
```bash
#!/bin/bash
# ifを使うよ

# -e : 種類を問わずに対象物が存在するかどうかを判定
# -f : ファイルが存在しているかどうかを判定
# -d : ディレクトリが存在しているかどうかを判定

# このファイルが存在するのかどうかの判定
if [[ -f $0 ]]; then
  echo "file exists ..."
fi
# 出力は
# file exists ...

if [[ -d $0 ]]; then
  echo "dir exists ..."
fi
# 出力は何もされない.
# このファイルはディレクトリでないから.

# 数値の比較は
read -p "Number? " n
if ((n > 10)); then
  echo "bigger than 10"
fi
# 比較演算子は
# ==
# !=
# >
# >=
# <
# <=
# が使える.

# 条件を組み合わせることもできる.
# && : 積集合
# || : 和集合
# ! : not
```

## for文
[10_for](https://github.com/solareenlo/shellscript-practice/blob/master/10_for)をご覧ください.
```bash
#!/bin/bash

# 1 2 3 4 5 と縦に表示
for i in 1 2 3 4 5
do
  echo $i
done

# 1 2 3 4 5 と縦に表示
for i in {1..5}
do
  echo $i
done

# 1 2 3 4 5 と縦に表示
for i in {1..5}; do
  echo $i
done

# 1 2 3 4 5 と縦に表示
for ((i=1; i<=5; i++)); do
  echo $i
done

# red green blue と縦に表示
colors=(red green blue)
for color in ${colors[@]}; do
  echo $color
done

# スペースで区切られた日付の要素を縦に表示
# for item in `date`; do
for item in $(date); do
  echo $item
done
```

## while文
```bash
#!/bin/bash

i=0
1 2 3 と縦に表示される
while ((i < 3)); do
  ((i++))
  echo $i
done

# continue でwhile処理をスキップ
# break でwhile処理を強制終了

# 1 2 3 5 6 7 と縦に表示される
while((i < 10));do
  ((i++))
  if ((i == 4)); then
    continue
  fi
  if((i == 8)); then
    break
  fi
  echo $i
done

# : はnullコマンドと言われていて, いつでも条件が成立するコマンド
# これを使って無限ループを作れる
# quit と打ち込まれると終了するプログラム
while :
do
  read -p "Command? " cmd
  if [[ $cmd == "quit" ]]; then
    break
  else
    echo "$cmd"
  fi
done
```

## catでテキストファイルの中身に文字を保存する
```bash
cat > colors.txt
red
green
blue
# CTL + d
# で保存
```

## whileを使って, テキストファイルの中身を1行ずつ読み込む
```bash
#!/bin/bash

i=1
# colors.txtの中身が1行ずつ読み込まれて標準出力に表示される
while read line; do
  echo $i "$line"
  ((i++))
done < colors.txt
```

## catとwhileを使って, catで表示するものに行番号を付ける
```bash
# 13_add_numberの中身
#!/bin/bash

i=1
while read line; do
  echo $i "$line"
  ((i++))
done
```
上記を実行すると
```bash
cat colors.txt | bash 13_add_number
> 1 red
> 2 green
> 3 blue
```

## caseを使って条件分岐
```bash
# 14_caseの中身
#!/bin/bash

read -p "Signal color? " color
case "$color" in
  red)
    echo "stop"
    ;;
  blue|green)
    echo "go"
    ;;
  yellow)
    echo "caution"
    ;;
  *)
    echo "wrong signal"
    ;;
esac
```
上記を実行すると
```bash
bash 14_case
> Signal color?
blue
> go
```
または
```bash
bash 14_case
> Signal color?
yellow
> caution
```
または
```bash
bash 14_case
> Signal color?
red
> stop
```

## selectを使ってメニューを作る
```bash
# 15_selectの中身
#!/bin/bash

# select の中身はループになってるのでbreakで終わらせてあげる
select color in red blue green yellow; do
  case "$color" in
    red)
      echo "stop"
      ;;
    blue|green)
      echo "go"
      ;;
    yellow)
      echo "caution"
      ;;
    *)
      echo "wrong signal"
      break
  esac
done
```
上記を実行すると
```bash
bash 15_select
> 1) red
> 2) blue
> 3) green
> 4)yello
> #?
1
> stop
2
> go
3
> go
4
> caution
5
> wrong signal
```

## 関数を使う
```bash
#!/bin/bash

# function hello() {
#   echo "hello ..."
# }
# function は省略することもでき
hello() {
  echo "hello ..."
}

hello
# > hello ...

# function へは引数を渡すことももちろんでき
hello2() {
  echo "hello $1"
}
hello2 taro
# > hello taro

# 関数の中でifももちろん使える
hello3() {
  if [[ $1 == "taro" ]]; then
    return 0 # 0 がtrue
  else
    return 1 # 1 がfalse
  fi
}

# 直前の終了ステータスは$?で調べることができる
hello3 taro; echo $?
# > 0
hello3 sola; echo $?
# > 1
```

## 変数スコープ
```bash
#!/bin/bash

hello() {
  name="taro"
  echo "hello ..."
}

hello
# > hello ...
echo $name
# > taro
# 関数の中の変数にアクセスできる
# それをさせないようにするには

hello2() {
  local name2="sola"
  echo "hello world"
}

hello2
# > hello world
echo $name2
# 何も表示されない
# name2はlocalが付いているので,
# hello2の関数内だけで使える
```
