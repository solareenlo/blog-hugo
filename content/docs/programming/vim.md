# Vimとは
- viから派生したテキストエディタ.
- プラグインを導入したり, .vimrcの設定を変更したりして, 自分好みにカスタマイズがどんどんできる.
- **GitHubリポジトリ:** https://github.com/vim/vim

## ソースからインストール
### スクラッチからインストール
```bash
git clone git@github.com:vim/vim.git
cd vim
./configure
make
sudo make install
```

### インストールし直す
```bash
cd vim
make distclean
rm src/auto/config.cache
./configure
make
sudo make install
```

### clipboard機能とclientserver機能を付けてインストール
```bash
cd /mnt/md0/github
git clone git@github.com:vim/vim.git
cd vim
make distclean
rm src/auto/config.cache
# 必要なパッケージをインストール
sudo apt install \
  lua5.2 \
  liblua5.2-dev \
  luajit \
  libluajit-5.2 \
  ruby-dev \
  xorg-dev
# /usr/include/lua5.2/の中身を/usr/include/lua5.2/include/へコピーする
cd /usr/include/lua5.2
sudo mkdir include
cp *.h include/
# libluaのシンボリックリンクを張る
sudo ln -s /usr/lib/x86_64-linux-gnu/liblua5.3.so /usr/local/lib/liblua.so
cd /mnt/md0/github/vim
# 必要なオプションを付けて./configureを行う
./configure \
  --with-features=huge \
  --with-x \
  --enable-multibyte \
  --enable-luainterp=dynamic \
  --enable-gpm \
  --enable-cscope \
  --enable-fontset \
  --enable-fail-if-missing \
  --prefix=/usr/local \
  --enable-pythoninterp=dynamic \
  --enable-python3interp=dynamic \
  --enable-rubyinterp=dynamic \
  --enable-gui=auto \
  --enable-gtk2-check \
  --disable-darwin \
  --with-lua-prefix=/usr/local
  # --disable-darwin は, macOSでmakeするときに必要.
  # macOSの場合, 最後の '--with-lua-prefix' を指定しないとconfigure時に 'configure: error: could not configure lua' が出てしまう.
# makeする
make
# インストールする
sudo make install
vim --version
```

### References
- [vimtex on macOS でハマったときのメモ](https://qiita.com/tYohei/items/8b047b6566bc0c946abd)
- [Linuxでのビルド方法](https://vim-jp.org/docs/build_linux.html)
- [WSLにVim8を入れてみる](https://qiita.com/sat0ken/items/82fee34035ce1e6960ae)
- [vimでclipboardを+にしたいけどならない人向け](https://qiita.com/Nikkely/items/7bfa4e71a6eb1e3d7bed)
- [Mac上のVimを最新にした際のメモ(LuaJIT対応)](https://liquidz.github.io/2013/12/vim.html)
- [Vim can't build the athena GUI on macOS High Sierra #2444](https://github.com/vim/vim/issues/2444)

## .vimrcの例
- .vimrcとはvimの設定を書いてあるファイル.
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

### 外部コマンド実行
`:!`を使う.
```bash
vim # vimモードに突入
:!ls # lsを表示してくれる
```

### 新規にファイル作成
`:e`を使う.
```bash
vim # vimモードに突入
:e test.txt # test.txtが作られる
```

### References
- [さっさと帰りたい怠け者エンジニアは vim をマスターしましょう その2 - 編集](http://wolfbash.hateblo.jp/entry/2017/09/05/234143)
- [さっさと帰りたい怠け者エンジニアは vim をマスターしましょう その1 - 基本と移動](http://wolfbash.hateblo.jp/entry/2017/08/25/121711)
- [新人達を1ヶ月でガチvimmerにした方法](https://qiita.com/nyantera/items/4bf29ca6f11bc797a9cb)

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
:call dein#install() # プラグインをインストール
:call dein#update() # プラグインをアップデート
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

## アイコン表示
### vim-devicons
- [ryanoasis/vim-devicons](https://github.com/ryanoasis/vim-devicons)

### 手順
1. Nerdフォント(アイコンが含まれているフォント)をダウンロードして設定
  - Linuxだと[GitHub](https://github.com/ryanoasis/nerd-fonts#patched-fonts)から好きなフォントをダウンロード
  - MacだとHomebrewを使ってフォントをダウンロードが簡単
      - Homebrewでインストールしたフォントだと`hack-nerd-font`を設定する
2. `vim-devicons`プラグインをダウンロード&インストール

## プラグインランキング
- [VimAwesome](https://vimawesome.com)

## Vim Script
- [モテる男のVim script短期集中講座](https://mattn.kaoriya.net/software/vim/20111202085236.htm)
