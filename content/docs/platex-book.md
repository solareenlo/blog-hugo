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
- zathuraをインストールする

```bash
sudo apt update
sudo apt upgrade -y
sudo apt install zathura
```
- [zathuraをソースコードからビルドしてインストールする方法]({{< relref "/docs/zathura-install.md" >}})

### vimプラグイン
- [lervag/vimtex](https://github.com/lervag/vimtex)
- [lervag/vimtexが良い](https://ymatz.net/journal/20180428/)
