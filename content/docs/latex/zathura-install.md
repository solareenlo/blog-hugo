# zathuraのインストール方法
- 以下zathuraインストールに必要なものをどんどんインストールしていきます.
- まだ未完.

## mesonをインストール
- mesonはオープンソースビルドシステムのこと.
```bash
# python3やもろもろをインストールする
sudo apt-get install python3 python3-pip python3-setuptools python3-wheel ninja-build
# pipを使ってmersonをインストールする
pip3 install --user meson
# PATHを通す(bash向け)
PATH="${PATH}:$(python -c 'import site; print(site.USER_BASE);')/bin"
```

## cmakeをインストール
```bash
sudo apt remove cmake
git clone git@gitlab.kitware.com:cmake/cmake.git
cd cmake
./bootstrap && make && sudo make install
```

## libmount-devをインストール
```bash
sudo apt update
sudo apt upgrade -y
sudo apt install libmount-dev
```

## glibをインストール
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

## gtkをインストール
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

## giraraをインストール
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

## mupdfをインストール
```bash
git clone --recursive git://git.ghostscript.com/mupdf.git
cd mupdf
git submodule update --init
make prefix=/usr/local install
```
```bash
# または
sudo apt update
sudo apt upgrade -y
sudo apt install libmupdf-dev mupdf mupdf-tools
```

## zathuraをインストール
```bash
git clone git@github.com:pwmt/zathura.git
cd zathura
meson build && cd build
ninja
sudo ninja install
```

## zathura-pdf-mupdfをインストール
```bash
git clone git@github.com:pwmt/zathura-pdf-mupdf.git
cd zathura-pdf-mupdf
meson build && cd build
ninja
sudo ninja install
```

## zathura-pdf-popplerをインストール
```bash
git clone git@github.com:pwmt/zathura-pdf-poppler.git
cd zathura-pdf-poppler
meson build && cd build
ninja
sudo ninja install
```
