# pLaTeXで本を作成する方法
- 総合開発環境を使う方法とコマンドラインを使う方法の2種類がある.

## Mac編
- [TexShop](https://pages.uoregon.edu/koch/texshop/)というlatexの総合開発環境を使うのが簡単.

## Ubuntu編
- Ubuntuでは総合開発環境を使わずに, TeX Live(TeXのディストリビューション)とzathura(pdfビューア)を使って, コマンドラインで作成していく.

### TeX Liveのインストール
```bash
# ミラーサイトからinstall-tl-unx.tar.gzをダウンロードする
# wget を使用する場合
wget http://mirror.ctan.org/systems/texlive/tlnet/install-tl-unx.tar.gz
# curl を使用する場合
curl -O http://mirror.ctan.org/systems/texlive/tlnet/install-tl-unx.tar.gz

# install-tl-unx.tar.gzを展開する
tar xvf install-tl-unx.tar.gz

# 展開したディレクトリに移動する
cd install-tl*

# root権限でインストーラを実行する
# Iを入力してインストールを開始する
sudo ./install-tl -no-gui -repository http://mirror.ctan.org/systems/texlive/tlnet/

# インストールが途中でストップした場合は, 以下のコマンドで再開する
sudo ./install-tl -no-gui -profile installation.profile

# インストールが終了したら /usr/local/bin ディレクトリ配下にシンボリックリンクを追加する
# ????はバージョンを入力する
sudo /usr/local/texlive/????/bin/*/tlmgr path add
```
- **Reference:** https://texwiki.texjp.org/?Linux

### Tex Liveのアップデート
```bash
sudo tlmgr update --self --all
```

### pdfビューアのインストール
- ここでは[pwmt/zathura](https://github.com/pwmt/zathura)をインストールする.
- zathuraはvimのキーバインドで操作できる軽量なpdfビューアで, 印刷はできない.
- zathuraをインストールする

```bash
sudo apt update
sudo apt upgrade -y
sudo apt install zathura
```
- [zathuraをソースコードからビルドしてインストールする方法](/docs/latex/zathura-install.md)

### TeX Liveの使い方
```bash
# texからdviを作成する
latex book.tex
platex book.tex
# dviからpdfを作成する
dvipdfmx book.dvi
```

### zathuraの使い方
```bash
zathura bool.pdf
# バックグラウンドでpdfを表示する
zathura --frok book.pdf
```
- 操作方法はvimのキーバインド.

### zathurarcの設定
- `zathurarc`にzathuraの設定をすることができる.
```bash
# zathurarcのhelpを表示する
man zathurarc
# zoomとscrollとclipboardを設定する
cat ~/.config/zathura/zathurarc
set zoom-step 30
set scroll-step 100
set selection-clipboard clipboard
```

### vimプラグイン
- [lervag/vimtex](https://github.com/lervag/vimtex)
- [lervag/vimtexが良い](https://ymatz.net/journal/20180428/)
