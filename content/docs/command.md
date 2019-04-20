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

```
ps aux
> USER               PID  %CPU %MEM      VSZ    RSS   TT  STAT STARTED      TIME COMMAND
> solareenlo       96263   4.3  2.0  5500012 168620   ??  S    水07PM  21:03.04 /Applications/Utilities/Terminal.app/Contents/MacOS/Terminal
> その他たくさん
```
こんな感じで標準出力される.

**Reference:** [ps auxの見方がよく判らない・・・](https://usado.jp/spdsk/2018/03/26/post-3479/)
