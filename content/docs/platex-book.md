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
- ここでは[pwmt/zathura](https://github.com/pwmt/zathura)をインストールします.
- zathuraはvimのキーバインドで操作できる軽量なpdfビューアで, 印刷はできません.

- meson(オープンソースビルドシステム)をインストールする
```bash
# python3やもろもろをインストールする
sudo apt-get install python3 python3-pip python3-setuptools python3-wheel ninja-build
# pipを使ってmersonをインストールする
pip3 install --user meson
# PATHを通す(bash向け)
PATH="${PATH}:$(python -c 'import site; print(site.USER_BASE);')/bin"
```
- cmakeをクローン・メイク・インストールする
```bash
sudo apt remove cmake
git clone git@gitlab.kitware.com:cmake/cmake.git
cd cmake
./bootstrap && make && sudo make install
```
- libmount-devをインストールする
```bash
sudo apt update
sudo apt upgrade -y
sudo apt install libmount-dev
```
- glibをクローン・ビルド・インストールする
```bash
git clone https://gitlab.gnome.org/GNOME/glib.git
cd glib
meson build
ninja -C build
sudo ninja -C build install
```
```bash
# もしくは
sudo apt update
sudo apt upgrade -y
sudo apt install libglib2.0-dev
```
- gtkをクローン・ビルド・インストールする
```bash
git clone https://gitlab.gnome.org/GNOME/gtk.git
# まだ依存関係を調査中
```
```bash
# または
sudo apt update
sudo apt upgrade -y
sudo apt install libgtk-3-dev
```
- giraraをクローン・メイク・インストールする
```bash
git clone git@github.com:pwmt/girara.git
cd girara
meson build && cd build
ninja
sudo ninja install
```
```bash
# または
sudo apt update
sudo apt upgrade -y
sudo apt install libgirara-gtk3-3
```
- zathuraをクローン・ビルド・インストールする
```bash
git clone git@github.com:pwmt/zathura.git
cd zathura
meson build && cd build
ninja
sudo ninja install
```

### vimプラグイン
- [lervag/vimtex](https://github.com/lervag/vimtex)
- [lervag/vimtexが良い](https://ymatz.net/journal/20180428/)
