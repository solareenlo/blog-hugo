---
title: MySQL
---

# MySQLとは
- オープンソースで公開されている関係データベース管理システム(RDBMS)の1つ.
- **GitHubリポジトリ:** https://github.com/mysql/mysql-server

# MySQL(v14.14)の使い方
**(Ver 14.14 Distrib 5.7.25, for Linux (x86_64))**

## MySQLのデータ構造
```bash
Database
  -> 複数のTable
    -> その中にid, title, bodyを持つ表があって,
    -> 行をRecord, Row, 列をField, Columと言う.
```
こういったDatabaseやTableやFieldやRecordを扱う言語をSQL(Structured Query Language)と言う.

## SQLの実行順番
|順番|操作名|命令文|
|---|---|---|
|1|テーブルの指定|from|
|2|結合|on, join|
|3|取得条件|where|
|4|グループ化|group by|
|5|関数|count, sum, avg, min, max|
|6|having|having|
|7|検索|select, distinct|
|8|順序|order by|
|9|limit|limit|
## MySQLの基本的な使い方
```bash
# Ubuntuへのインストール
sudo apt install mysql-server mysql-client

# 起動確認
sudo service mysql status
> mysql.service - MySQL Community Server

# MySQLの基本設定を行う
sudo mysql_secure_instalation
> Securing the MySQL server deployment.
# で, 色々と設定していく.

# コンソールからMySQLサーバに接続
sudo mysql -u root -p
> Enter password:
# password入力して, MySQLサーバに接続
> mysql>

# helpを見る
mysql> help;

# 状態を見る
mysql> status

# 現在のユーザーを表示
mysql> select user();
> +----------------+
> | user()         |
> +----------------+
> | root@localhost |
> +----------------|
> 1 row in set (0.00 sec)

# ; を忘れると
mysql> select user()
    ->
# 続きを打てと催促されるので, 慌てずに ; を打つ.
    ->;
> +----------------+
> | user()         |
> +----------------+
> | root@localhost |
> +----------------|
> 1 row in set (0.00 sec)

# 現在のコマンドをキャンセルするには \c を打つ.
mysql> select user()
    -> \c
mysql>

# MySQLサーバーへの接続を終了する
mysql> quit;
mysql> \q
>Bye
```

## DBを表示・新規作成・削除・操作対象にする
mysql> の後に打ち込むコマンドをQuery(クエリ)と言う.
MySQLでのクエリは大文字小文字の区別がない.

```bash
# DBを表示する
mysql> show databases;
> +--------------------+
> | Database           |
> +--------------------+
> | information_schema |
> | mysql              |
> | performance_schema |
> | sys                |
> +--------------------|
> 4 row in set (0.00 sec)
# これらはシステムが用いているDBなのでうっかり消さないように注意する.

# 新規にDBを作成する
mysql> create database mydb01;
> Query OK, 1 row affected (0.00 sec)
# OKが出れば成功
mysql> create database mydb02;
> Query OK, 1 row affected (0.00 sec)
mysql> create database mydb03;
> Query OK, 1 row affected (0.00 sec)

# DBを削除する
mysql> drop database mydb03;
> Query OK, 0 row affected (0.00 sec)
# OKが出れば成功

# 操作対象のDBを確認する
mysql> select database();
> +--------------------+
> | database()         |
> +--------------------+
> | NULL               |
> +--------------------|
> 1 row in set (0.00 sec)
# NULLが帰ってきたので, 操作対象のDBがないと言うこと

# 操作対象のDBを選択する
mysql> use mydb02;
> Database changed
mysql> select database();
> +--------------------+
> | database()         |
> +--------------------+
> | mydb02             |
> +--------------------|
> 1 row in set (0.00 sec)
```

## 作業用ユーザーを新規作成・削除する
rootユーザーでうっかりをすると大変なことになるのでDBごとに作業用ユーザーを作成する.
```bash
# userを新規作成
mysql> create user dbuser01@localhost identified by '6AVAkig2@#';
> Query OK, 0 raws affected (0.00 sec)

# userにDBの権限を付与する
>mysql grant all on mydb01.* to dbuser01@localhost;
# grant は権限を与える.
# all は全ての権限を与える.
# mydb01.* はmydb01にあるテーブル全てに対して と言う意味.
# to dbuser@localhost はdbuser@localhostに対して と言う意味.
> Query OK, 0 raws affected (0.00 sec)

# 一度rootでログアウトして, もう一度dbuser01@localhostでログインする.
mysql> quit;
> Bye
mysql -u dbuser01 -p mydb01
> Enter password:
6AVAkig2@#
> Welcome to the MySQL monitor. Commands end with ; or \g.
# で, 無事にdbuser01ユーザーでログインができた.

# 一応確認.
mysql> select user();
> +--------------------+
> | user()             |
> +--------------------+
> | dbuser01@localhost |
> +--------------------|
> 1 row in set (0.00 sec)
# ユーザーがdbuser01になってる.

# アクセスできるDBも確認.
mysql> show databases;
> +--------------------+
> | Database           |
> +--------------------+
> | information_schema |
> | mydb01             |
> +--------------------|
> 2 row in set (0.00 sec)
# きちんとmydb01にだけアクセスするようになってる.

# ユーザーの削除
# rootユーザーでログインする
mysql> quit;
> Bye
sudo mysql -u root -p
> Enter password:
# rootユーザーのpasswordを入力して, エンター
mysql> drop user dbuser01@localhost;
> Query OK, 0 raws affected (0.00 sec)
```

## 外部ファイルを実行する
先ずは, 外部ファイル[create_myapp.sql](https://github.com/solareenlo/mysql-practice/blob/master/create_myapp.sql)を作成し, rootユーザーでログインするときに外部ファイルを読み込ます方法.
```bash
mysql -u root < create_myapp.sql
>
# 何も反応がないけど, きちんとできてます.
```

もしくは, rootユーザーでログインした後に, 外部ファイルを読み込ます方法.
```bash
sudo mysql -u root
> Welcome to the MySQL monitor. Commands end with ; or \g.
mysql> source ./create_myapp.sql
# もしくは
mysql> \. ./create_myapp.sql
```

## Tableを新規作成・一覧表示・中身表示・削除する
Talbeを外部ファイルで作成して, 削除する.
```bash
mysql -u myapp_user -p myapp
> Enter password:
2VNAhigo@#
> Welcome to the MySQL monitor. Commands end with ; or \g.

# tableを外部ファイルを使って作成する
mysql> \. ./create_table.sql
> Query OK, 0 rows affected (0.00 sec)

# tableの一覧を見る
mysql> show tables;
> tableの一覧が見れる

# usersというtableの中身を見る
mysql> desc users;
> userというtableの中身が表示される
> 3 rows in set (0.00 sec)

# usersというtableの削除方法
mysql> drop table users;
> Query OK, 0 rows affected (0.00 sec)
mysql> show tables;
> Empty set (0.00 sec)
```

## MySQLが扱えるデータ型
```bash
number:
- int # 整数型
- float # 実数型
- double # 倍精度実数型
- int unsigned # 正の整数型

string:
- char # 固定長の文字列
- char(4) # 4文字固定の文字列
- varchar # 可変長の文字列
- varchar(255) # 255バイトまでの可変長文字列
- text # 可変長の文字列

data/time;
- date # 日付
- time # 時間
- datetime # 日時 '2020-02-22 20:22:33'と表示することができる

true/false;
- boolean # booleanは1桁の整数の型であるtinyint(1)で返される
  true -> 1 # 空文字を含むNull以外は全てtrueになる
  false -> 0 # Nullがfalse
```

## Recordの挿入
[insert_record.sql](https://github.com/solareenlo/mysql-practice/blob/master/insert_record.sql)をご覧ください.
```bash
# まずDBにアクセス
mysql -u myapp_user -p myapp
> Enter password:
2VNAhigo@#
> Welcome to the MySQL monitor. Commands end with ; or \g.

mysql> \. ./insert_record.sql
> 実行結果が返ってくる.
```

## Fieldに制限をかける
[restricted_field.sql](https://github.com/solareenlo/mysql-practice/blob/master/restricted_field.sql)をご覧ください.
```bash
# まずDBにアクセス
mysql -u myapp_user -p myapp
> Enter password:
2VNAhigo@#
> Welcome to the MySQL monitor. Commands end with ; or \g.

mysql> \. ./restricted_field.sql
> 実行結果が返ってくる.
```

## Tableの構造を変える
[change_table.sql](https://github.com/solareenlo/mysql-practice/blob/master/change_table.sql)をご覧ください.
```bash
# まずDBにアクセス
mysql -u myapp_user -p myapp
> Enter password:
2VNAhigo@#
> Welcome to the MySQL monitor. Commands end with ; or \g.

# column(field)を追加する
# 任意の場所にcolmun(field)を追加する(今回はnameの後ろにemailを追加)
# column(field)を削除する
# column(field)名の変更する
# tableの名前を変更する
mysql> \. ./change_table.sql
> 実行結果が返ってくる
```

## Recordを抽出する
[extract_record.sql](https://github.com/solareenlo/mysql-practice/blob/master/extract_record.sql)をご覧ください.
```bash
# まずDBにアクセス
mysql -u myapp_user -p myapp
> Enter password:
2VNAhigo@#
> Welcome to the MySQL monitor. Commands end with ; or \g.
mysql> \. ./extract_record.sql
> 実行結果が返ってくる
```

## 文字列を抽出条件にする
[extract_using_character.sql](https://github.com/solareenlo/mysql-practice/blob/master/extract_using_character.sql)をご覧ください.
```bash
# まずDBにアクセス
mysql -u myapp_user -p myapp
> Enter password:
2VNAhigo@#
> Welcome to the MySQL monitor. Commands end with ; or \g.
mysql> \. ./extract_using_character.sql
> 実行結果が返ってくる
```

## 数字で並び替えたり, 抽出条件を制限したりする
[extract_using_number.sql](https://github.com/solareenlo/mysql-practice/blob/master/extract_using_number.sql)をご覧ください.
```bash
# まずDBにアクセス
mysql -u myapp_user -p myapp
> Enter password:
2VNAhigo@#
> Welcome to the MySQL monitor. Commands end with ; or \g.
mysql> \. ./extract_using_number.sql
> 実行結果が返ってくる
```

## Recordを更新, 削除する
[update_record.sql](https://github.com/solareenlo/mysql-practice/blob/master/update_record.sql)をご覧ください.
```bash
# まずDBにアクセス
mysql -u myapp_user -p myapp
> Enter password:
2VNAhigo@#
> Welcome to the MySQL monitor. Commands end with ; or \g.
mysql> \. ./update_record.sql
> 実行結果が返ってくる
```

## 数値の演算を行ったり, 活用したりする
[calculate.sql](https://github.com/solareenlo/mysql-practice/blob/master/calculate.sql)をご覧ください.
```bash
# まずDBにアクセス
mysql -u myapp_user -p myapp
> Enter password:
2VNAhigo@#
> Welcome to the MySQL monitor. Commands end with ; or \g.
mysql> \. ./calculate.sql
> 実行結果が返ってくる
```

## 文字列の演算を行う
[calculate_using_character.sql](https://github.com/solareenlo/mysql-practice/blob/master/calculate_using_character.sql)をご覧ください.
```bash
# まずDBにアクセス
mysql -u myapp_user -p myapp
> Enter password:
2VNAhigo@#
> Welcome to the MySQL monitor. Commands end with ; or \g.
mysql> \. ./calculate_using_character.sql
> 実行結果が返ってくる
```

## enum型を使う
enum型とは複数の文字列の中から１つだけを格納できるデータ型.  
enumを使うことで有効なデータ以外は無効なデータとして扱うことができる.  
[enum.sql](https://github.com/solareenlo/mysql-practice/blob/master/enum.sql)をご覧ください.
```bash
# まずDBにアクセス
mysql -u myapp_user -p myapp
> Enter password:
2VNAhigo@#
> Welcome to the MySQL monitor. Commands end with ; or \g.
mysql> \. ./enum.sql
> 実行結果が返ってくる
```

## set型を使う
set型を使うと複数の選択肢から複数個選べる.  
[set.sql](https://github.com/solareenlo/mysql-practice/blob/master/set.sql)をご覧ください.
```bash
# まずDBにアクセス
mysql -u myapp_user -p myapp
> Enter password:
2VNAhigo@#
> Welcome to the MySQL monitor. Commands end with ; or \g.
mysql> \. ./set.sql
> 実行結果が返ってくる
```

## if, caseの使い方
[if_case.sql](https://github.com/solareenlo/mysql-practice/blob/master/if_case.sql)をご覧ください.
```bash
# まずDBにアクセス
mysql -u myapp_user -p myapp
> Enter password:
2VNAhigo@#
> Welcome to the MySQL monitor. Commands end with ; or \g.
mysql> \. ./if_case.sql
> 実行結果が返ってくる
```

## 抽出結果をtableにする
- caseで抽出した結果で新たにtableを作成する.
- 既存のtableをそのままコピーして新たなtableを作成する.
- 既存のtableの構造だけをコピーして新たなtableを作成する.
- 詳しくは[make_table_using_extracted_data.sql](https://github.com/solareenlo/mysql-practice/blob/master/make_table_using_extracted_data.sql)をご覧ください.
```bash
# まずDBにアクセス
mysql -u myapp_user -p myapp
> Enter password:
2VNAhigo@#
> Welcome to the MySQL monitor. Commands end with ; or \g.
mysql> \. ./make_table_using_extracted_data.sql
> 実行結果が返ってくる
```

## Dataの集計処理を行う
- talbe内のnull以外のdataの個数を調べる.
- table内の全てのdataの個数を調べる.
- fieldの合計値, 最小値, 最大値, 平均値を調べる.
- table内の重複しない値のみを抽出する.
- table内の重複しない値の個数を調べる.
- 詳しくは[aggregate_data.sql](https://github.com/solareenlo/mysql-practice/blob/master/aggregate_data.sql)をご覧ください.
```bash
# まずDBにアクセス
mysql -u myapp_user -p myapp
> Enter password:
2VNAhigo@#
> Welcome to the MySQL monitor. Commands end with ; or \g.
mysql> \. ./aggregate_data.sql
> 実行結果が返ってくる
```

## Group集計
- groupごとに値を集計するには group by を用いる.
- group by で集計した後の data に対して条件を付ける場合には where ではなく having を用いる.
- having はグループ化に使った値や集計した値しか条件に使えない.
- where と group by を一緒に使った場合は, where の条件でデータの抽出を行った後に group by でその集計を行うことになる.
- 詳しくは[aggregate_group.sql](https://github.com/solareenlo/mysql-practice/blob/master/aggregate_group.sql)をご覧ください.
```bash
# まずDBにアクセス
mysql -u myapp_user -p myapp
> Enter password:
2VNAhigo@#
> Welcome to the MySQL monitor. Commands end with ; or \g.
mysql> \. ./aggregate_group.sql
> 実行結果が返ってくる
```

## Sub Queryを使う
- 一時的にしか使わない tabale だと sub query を用いて, 新たな table を作らずに表示できる.
- 詳しくは[sub_query.sql](https://github.com/solareenlo/mysql-practice/blob/master/sub_query.sql)をご覧ください.
```bash
# まずDBにアクセス
mysql -u myapp_user -p myapp
> Enter password:
2VNAhigo@#
> Welcome to the MySQL monitor. Commands end with ; or \g.
mysql> \. ./sub_query.sql
> 実行結果が返ってくる
```

## Viewを使う
- 抽出条件に名前を付けて table の用に扱える view.
- 詳しくは[view.sql](https://github.com/solareenlo/mysql-practice/blob/master/view.sql)をご覧ください.
```bash
# まずDBにアクセス
mysql -u myapp_user -p myapp
> Enter password:
2VNAhigo@#
> Welcome to the MySQL monitor. Commands end with ; or \g.
mysql> \. ./view.sql
> 実行結果が返ってくる
```

## Transactionを使う
- 複数の処理をひとまとめにできる transaction.
- 詳しくは[transaction.sql](https://github.com/solareenlo/mysql-practice/blob/master/transaction.sql)をご覧ください.
```bash
# まずDBにアクセス
mysql -u myapp_user -p myapp
> Enter password:
2VNAhigo@#
> Welcome to the MySQL monitor. Commands end with ; or \g.
mysql> \. ./transaction.sql
> 実行結果が返ってくる
```

## Indexを使う
- index(索引)の設定をしておくと data の抽出が速くなる.
- でも, index は data の追加や更新処理を行うたびに作り直されるので, あまり index を付けすぎると処理が遅くなる.
- 詳しくは[index.sql](https://github.com/solareenlo/mysql-practice/blob/master/index.sql)をご覧ください.
```bash
# まずDBにアクセス
mysql -u myapp_user -p myapp
> Enter password:
2VNAhigo@#
> Welcome to the MySQL monitor. Commands end with ; or \g.
mysql> \. ./index.sql
> 実行結果が返ってくる
```

## Inner join(内部結合)を使う
- inner join = 2つの table に共通の data だけを取得する方法
- 詳しくは[inner_join.sql](https://github.com/solareenlo/mysql-practice/blob/master/inner_join.sql)をご覧ください.
```bash
# まずDBにアクセス
mysql -u myapp_user -p myapp
> Enter password:
2VNAhigo@#
> Welcome to the MySQL monitor. Commands end with ; or \g.
mysql> \. ./inner_join.sql
> 実行結果が返ってくる
```

## Outter join(外部結合)を使う
- outer join (外部結合) = 2つの table で一致しない data も含めて data を取得する方法
- 2つの内どちらを軸にするかを決める必要がある.
- 詳しくは[outter_join.sql](https://github.com/solareenlo/mysql-practice/blob/master/outter_join.sql)をご覧ください.
```bash
# まずDBにアクセス
mysql -u myapp_user -p myapp
> Enter password:
2VNAhigo@#
> Welcome to the MySQL monitor. Commands end with ; or \g.
mysql> \. ./outter_join.sql
> 実行結果が返ってくる
```

## Foreign keyを使う
- 外部キー制約(foreign key)を使うと先に作られた table の field にない data の挿入を許さない書き方ができる.
- 紐付ける値同士の型は一致していないといけない.
- 外部キー制約に合わないものはエラーになって table に書き込まれない.
- 外部キー制約を設定すると, 関連する data がある場合には data の削除や更新が簡単にはできなくなる.
- 詳しくは[foreign_key.sql](https://github.com/solareenlo/mysql-practice/blob/master/foreign_key.sql)をご覧ください.
```bash
# まずDBにアクセス
mysql -u myapp_user -p myapp
> Enter password:
2VNAhigo@#
> Welcome to the MySQL monitor. Commands end with ; or \g.
mysql> \. ./foreign_key.sql
> 実行結果が返ってくる
```

## last_insert_id()を使う
- last_insert_id() を使うと直前に挿入した record の id を引っ張ってきてくれる.
- 詳しくは[last_insert_id.sql](https://github.com/solareenlo/mysql-practice/blob/master/last_insert_id.sql)をご覧ください.
```bash
# まずDBにアクセス
mysql -u myapp_user -p myapp
> Enter password:
2VNAhigo@#
> Welcome to the MySQL monitor. Commands end with ; or \g.
mysql> \. ./last_insert_id.sql
> 実行結果が返ってくる
```

## triggerを使う
- ある table で何らかの処理が行われたら, それを trigger にして, またべつの処理を走らせることができる機能を trigger という.
- insert, delete, updateなどの処理に対して trigger を発動できる.
- after, before で設定できる.
- field を縦表示するには`show triggers \G`のように, 後ろに`\G`を付けてあげれば良い.
- 詳しくは[trigger.sql](https://github.com/solareenlo/mysql-practice/blob/master/trigger.sql)をご覧ください.
```bash
# まずDBにアクセス
mysql -u myapp_user -p myapp
> Enter password:
2VNAhigo@#
> Welcome to the MySQL monitor. Commands end with ; or \g.
mysql> \. ./trigger.sql
> 実行結果が返ってくる
```

## triggerで複数の処理を行う
- trigger で複数の処理を行うには begin end を使えば良い.
- 詳しくは[trigger2.sql](https://github.com/solareenlo/mysql-practice/blob/master/trigger2.sql)をご覧ください.
```bash
# まずDBにアクセス
mysql -u myapp_user -p myapp
> Enter password:
2VNAhigo@#
> Welcome to the MySQL monitor. Commands end with ; or \g.
mysql> \. ./trigger2.sql
> 実行結果が返ってくる
```

## current_timestampを使う
- on update current_timestamp で更新時にその時の日時で field を自動更新してくれる.
- 詳しくは[current_timestamp.sql](https://github.com/solareenlo/mysql-practice/blob/master/current_timestamp.sql)をご覧ください.
```bash
# まずDBにアクセス
mysql -u myapp_user -p myapp
> Enter password:
2VNAhigo@#
> Welcome to the MySQL monitor. Commands end with ; or \g.
mysql> \. ./current_timestamp.sql
> とりあえずの table が表示される.
mysql> update posts set title = 'updated' where id = 2;
mysql> select * from posts;
> +----+---------+--------+---------------------+---------------------+
> | id | title   | body   | created             | updated             |
> +----+---------+--------+---------------------+---------------------+
> |  1 | title 1 | body 1 | 2019-02-28 15:31:16 | 2019-02-28 15:31:16 |
> |  2 | updated | body 2 | 2019-02-28 15:31:16 | 2019-02-28 15:38:20 |
> |  3 | title 3 | body 3 | 2019-02-28 15:31:16 | 2019-02-28 15:31:16 |
> +----+---------+--------+---------------------+---------------------+
```

## 日付を扱う
- MySQLで日付を扱うには日付っぽい書式を使えば自動で認識してくれる.
- 日付の変更, 日付の条件指定, 日付の計算, 日付の書式変更
- 詳しくは[date.sql](https://github.com/solareenlo/mysql-practice/blob/master/date.sql)をご覧ください.
```bash
# まずDBにアクセス
mysql -u myapp_user -p myapp
> Enter password:
2VNAhigo@#
> Welcome to the MySQL monitor. Commands end with ; or \g.
mysql> \. ./date.sql
> 実行結果が返ってくる
```

## Backupを行い復元できるようにする
- backup を作成する方法はいろいろありますが, 簡単な方法は mysqldump を使うこと.
```bash
# まず backup する data を作成する.
mysql -u myapp_user -p myapp
> Enter password:
2VNAhigo@#
> Welcome to the MySQL monitor. Commands end with ; or \g.
mysql> \. ./date.sql # とかで data を作ってあげて,
mysql> quit; # 一度 mysql を抜ける.
```
```bash
# backupを取る.
mysqldump -u myapp_user -p myapp > 201902_myapp.backup.sql
> Enter password:
2VNAhigo@#
# これで backup が取れたことになる.
```
```bash
# 次に一番はじめに作成した data を更新してから, 先程 backup した data で復元を行ってみる.
mysql -u myapp_user -p myapp
> Enter password:
2VNAhigo@#
> Welcome to the MySQL monitor. Commands end with ; or \g.
mysql> select * from posts;
> +----+---------+--------+---------------------+---------------------+
> | id | title   | body   | created             | updated             |
> +----+---------+--------+---------------------+---------------------+
> |  1 | title 1 | body 1 | 2019-02-28 16:26:27 | 2019-02-28 16:26:27 |
> |  2 | title 2 | body 2 | 2016-12-31 10:10:10 | 2019-02-28 16:26:27 |
> |  3 | title 3 | body 3 | 2019-02-28 16:26:27 | 2019-02-28 16:26:27 |
> +----+---------+--------+---------------------+---------------------+
```
```bash
# うっかり table の data を消す.
mysql> delete from posts where id > 1;
mysql> select * from posts;
> +----+---------+--------+---------------------+---------------------+
> | id | title   | body   | created             | updated             |
> +----+---------+--------+---------------------+---------------------+
> |  1 | title 1 | body 1 | 2019-02-28 16:26:27 | 2019-02-28 16:26:27 |
> +----+---------+--------+---------------------+---------------------+
```
```bash
# backup data を読み込む.
mysql> \. ./201902_myapp.backup.sql
# たくさん > Query OK, が出る.
```
```bash
mysql> select * from posts;
> +----+---------+--------+---------------------+---------------------+
> | id | title   | body   | created             | updated             |
> +----+---------+--------+---------------------+---------------------+
> |  1 | title 1 | body 1 | 2019-02-28 16:26:27 | 2019-02-28 16:26:27 |
> |  2 | title 2 | body 2 | 2016-12-31 10:10:10 | 2019-02-28 16:26:27 |
> |  3 | title 3 | body 3 | 2019-02-28 16:26:27 | 2019-02-28 16:26:27 |
> +----+---------+--------+---------------------+---------------------+
# backup の復元完了.
mysql> quit;
```
