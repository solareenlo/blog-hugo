# ターミナルとは
GUIの上でCUIの操作をしたいときに使用するアプリケーションのこと.

## コンソール/ターミナル/シェルの違い
- コンソールとは入出力をつかさどる端末ハードウェア
- ターミナルとはGUIでCUI環境を再現するもの
- シェルとはCUI環境における対話的インターフェイス
 - **Reference:** [【初心者向け】シェル・ターミナル・コンソールの違いとは？](https://eng-entrance.com/linux-basic-shell-terminal-console)

下地にシェルがありコンソールやターミナルから命令をOSへ伝達してくれる.
コンソールでアクセスしてもターミナルでアクセスしても対話するものはシェルとなる.  
コンソールの場合はハードウェア的に直にアクセスしているがターミナルの場合はGUI環境でソフトウェア的にシェルを呼び出し命令を実行するようになっている.

## ターミナルのJSON出力に色付けする
[jq](https://github.com/stedolan/jq)を使う.

```bash
# こんな感じ
echo '{"items":[{"item_id":1,"name":"すてきな雑貨","price":2500},{"item_id":2,"name":"格好いい置物","price":4500}]}' | jq .
> {
>   "items": [
>     {
>       "item_id": 1,
>       "name": "すてきな雑貨",
>       "price": 2500
>     },
>     {
>       "item_id": 2,
>       "name": "格好いい置物",
>       "price": 4500
>     }
>   ]
> }
```
**Reference:** [jq コマンドを使う日常のご紹介](https://qiita.com/takeshinoda@github/items/2dec7a72930ec1f658af)
