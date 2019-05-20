# Vimとは
- viから派生したテキストエディタ.
- プラグインを導入したり, .vimrcの設定を変更したりして, 自分好みにカスタマイズがどんどんできる.
- **GitHubリポジトリ:** https://github.com/vim/vim

## 外部コマンド実行
`:!`を使う.
```bash
vim # vimモードに突入
:!ls # lsを表示してくれる
```

## .vimrcの例
- [solareenlo/vim-config](https://github.com/solareenlo/vim-config)
- [よく使われているvimrcの設定ランキング](https://qiita.com/reireias/items/230c77b3ff5575832654)

## 使い方
### 削除
|コマンド|動作|
|---|---|
|D|カーソル以降を削除|

### 置換
|コマンド|動作|
|---|---|
|:%s/old/new/c|ファイル上の`old`を1つずつ確認しながら`new`に置換|
|:%s/old/new/g|ファイル上の全ての`old`を`new`に置換|
|:s/old/new/g|カーソル行の全ての`old`を`new`に置換|
|:10,20s/old/new/g|10~20行目の全ての`old`を`new`に置換|
|:`'<,'>`s/old/new/g|ビジュアルモードで選択中の範囲の全ての`old`を`new`に置換.<br>`'<,'>`の部分は, 範囲を選択中に`:`を押すと自動的に出る.|

- **References:**
  - [さっさと帰りたい怠け者エンジニアは vim をマスターしましょう その2 - 編集](http://wolfbash.hateblo.jp/entry/2017/09/05/234143)
  - [さっさと帰りたい怠け者エンジニアは vim をマスターしましょう その1 - 基本と移動](http://wolfbash.hateblo.jp/entry/2017/08/25/121711)

## オススメのプラグイン
- [よく使われているvimのプラグイン top20](https://qiita.com/reireias/items/5364dcaada1a5b88a206)
- [オレ的vimプラグイン10選](https://qiita.com/reireias/items/beaa3bb0e299ae934217)

## プラグインマネージャー
たくさんのプラグインを簡単にインストールできるマネージャー.

### dein.vim
- [Shougo/dein.vim](https://github.com/Shougo/dein.vim)

```bash
curl https://raw.githubusercontent.com/Shougo/dein.vim/master/bin/installer.sh > installer.sh
# For example, we just use `~/.cache/dein` as installation directory
sh ./installer.sh ~/.cache/dein
```
そして, vimを開いて,
```bash
:call dein#install()
```

## コメントアウト
### tcomment
- [tomtom/tcomment_vim](https://github.com/tomtom/tcomment_vim)

```bash
# 複数行コメントアウト
# SHIFT + V で複数行を選択してから,
gc
# 1行コメントアウト
gcc
```

## シンボル置換
### surround.vim
- [tpope/vim-surround](https://github.com/tpope/vim-surround)

"Hello World!"
```sh
# 単語の終始の置換('→")
cs'"
# 単語の終始の"削除
ds"
```

## ツリー表示
### NERDTree
- [scrooloose/nerdtree](https://github.com/scrooloose/nerdtree)

#### ファイル操作
|o(enter)|ファイルを開く|
|---|---|
|go|ファイルを開き、カーソルはツリーに保持する|
|t|タブで開く|
|T|タブで開き、移動はしない|
|i|水平分割して開く|
|gi|水平分割して開き、移動はしない|
|s|垂直分割して開く|
|gs|垂直分割して開き、移動はしない|

#### ディレクトリ操作
|コマンド|説明|
|---|---|
|o(enter)|フォルダを開く|
|O|再帰的にディレクトリをすべて開く|
|x|親ディレクトリを閉じる|
|X|再帰的にすべての子ディレクトリを閉じる|
|e|新しいツリーを生成する|

#### ツリー操作
|コマンド|説明|
|---|---|
|P|ルートディレクトリへ移動|
|p|親ディレクトリへ移動|
|K|一番上へ移動|
|J|一番下へ移動|
|Ctrl+k|一つ上へ移動|
|Ctrl+j|一つ下へ移動|

#### ファイルシステム
|コマンド|説明|
|---|---|
|C|ツリーのルートを選択したディレクトリに変更|
|u|ツリーのルートを上の階層にする|
|U|変更前のツリーの状態を保持して、ツリーのルートを上の階層にする|
|r|選択したディレクトリをリフレッシュする|
|R|ツリーのルートをリフレッシュする|
|m|メニューを表示する|
|cd|選択したディレクトリにcwdを変更する|
|CD|cwdをツリールートに変更する|

#### その他
|コマンド|説明|
|---|---|
|I|隠しファイルの表示、非表示|
|B|ブックマークの表示・非表示|
|F|ファイルの表示・非表示|

## プラグインランキング
- [VimAwesome](https://vimawesome.com)

## Vim Script
- [モテる男のVim script短期集中講座](https://mattn.kaoriya.net/software/vim/20111202085236.htm)
