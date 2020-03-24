# Rust とは
- Mozilla が支援するオープンソースのシステムプログラミング言語のこと．
- 速度，安全性，平行性の3つのゴールにフォーカスしている．
- 主に C，C++ に置き換わるものとされている．
- **GitHub リポジトリ:** https://github.com/rust-lang/rust

## Rust の特徴
### 概要
- C++ に匹敵する実行速度や詳細なメモリ管理が実現できる．
- 不正なメモリ領域を指すポインタなどを許容しないといったメモリ安全性を保証している（メモリセーフ）．
- マルチスレッドによる並列実行時のデータ競合をコンパイル時に排除する（スレッドセーフ）．
- 関数型言語由来の便利な機能，代数的データ型とパターンマッチ，型クラスによる多相関数，コンパイラによる型推論などを取り入れている．
- ガーベージコレクタのような複雑なランタイムをもっていない．
- 多言語関数インターフェースを用いて，他のプログラミング言語との間で相互に関数を呼び出せる．

### 速度面
- **マシンコードへコンパイル**
    - コンパイラがプログラムをマシンコード（プロセッサが理解できる機械語）へ変換する．
- **静的型付け言語**
    - 静的型付け言語は，変数および関数の引数や戻り値などすべての値について，その型をコンパイル時に決定する．
    - 動的型付け言語は，プログラムの実行時に実際の値を見て型を決定する．
- **ゼロコスト抽象化**
    - ゼロコスト抽象化とは，プログラム言語が持つ抽象化（対象から注目すべき要素を重点的に抜き出して，他は無視する手法）の仕組みが実行時のコスト（実行速度やメモリ使用量など）なしに動作すること．
- **ガーベージコレクションを行わない軽量なランタイム**

### 安全面
- Rust はコンパイル時の静的解析により以下の安全性を保証している．
- **型安全性**
    - 型安全性とは，正しく型付けされたプログラムが不正な動作（未定義動作とも言う）をしないよう言語が定義されていること．
- **メモリ安全性**
    - Rust は C のようにメモリ内容への柔軟なアクセスができるが，C と違い以下のようなメモリ安全性を保証する．
    - データの転記の際のメモリ領域あふれを防ぐ．
    - ポインタによる誤ったメモリ境域へのアクセスを防ぐ．
    - 初期化前のメモリ領域へのアクセスを防ぐ．
    - 解放後のメモリ領域へのアクセスを防ぐ．
- **マルチスレッドプログラミングにおけるデータ競合の回避**
    - Rust では型安全性とメモリ安全性に用いられるコンパイラの静的解析機能を使い，データ競合の可能性を検出できる．
    - 故にコンパイルに成功したら，そのプログラムにはデータ競合が無いことが保証される．
    - Rust ではスレッド間でデータを受け渡したり共有したりするために，`チャネル`・`ロック`・`配列などの範囲`・`イミュータブルな参照`などの方法が用意されている．
- **アンセーフなコードのサポート**
    - FFI（多言語関数インターフェース）経由で多言語の関数を呼び出した時，コンパイルはその多言語の関数の安全性を確認できない．
    - 内向きのミュータビリティのような，安全性の検査をコンパイル時ではなく実行時に延期する仕組みがライブラリとして実行されてる．
    - これらに対するサポートも用意されている．

### 生産性を高めるモダンな機能
- 以下は Rustのモダンな機能の一部

| 機能 | 概要 |
| --- | ---- |
| 強力な型推論 | Rust コンパイルには強力な型推論が備わっており，変数を導入する let 文だけでなく，途中の式で型が決まる状況なら，型を明示しなくて良い． |
| 代数的データ型 | 代数的データ方は Haskell などの関数型言語にみられる機能でリッチなデータ構造が表現できる．<br>Rust の列挙型（enum）はその名前とは裏腹に，代数的データ型を構成する列挙型，直積型，直和型のすべてを表現できる． |
| パターンマッチ | 多くの言語に存在する switch 文の代わりに，関数型言語由来で表現力の高い match 式を持つ．<br>列挙型のバリアントの判定と同時に各データフィールドの値を複数の変数に分配できる． |
| トレイトによるポリフォーリズム | トレイトは型が持つべき性質（実装すべきメソッド）を定義する．<br>一見すると Java の interfacee に似ているが，実際には Haskell の型クラスに近く，柔軟性が高い． |

### シングルバイナリ・クロスバイナリ
- アプリケーションをビルドするとシングルバイナリ（単一の実行可能ファイル）が生成される．
- Rustはクロスコンパイルをして幅広いプラットフォームに向けたバイナリも生成できる．
- Rustは以下のプラットフォームに対応したバイナリを生成できる．

| プロセッサ・アーキテクチャ           | オペレーティングシステム                                                              |
| --------------------------           | ------------------------                                                              |
| x86 系                                | Linux, macOX, Windows, BSD系(FreeBSD, NetBSD, OpenBSDなど), Solaris, RedoxOS, Fuchsia |
| ARM 系                                | Android, iOS, Linux, Fuchsia                                                          |
| WebAssembly, asm.js                  | Web ブラウザや Node.js のような JavaScript ランタイム                                      |
| マイコン（ARM Cortex-M シリーズなど） | ベアメタル（OS なし）                                                                  |
| 汎用 GPU                              | NVIDIA PTX 中間命令                                                                    |
| MIPS, S390x, PowerPC                 | Linux                                                                                 |
| SPARC                                | Solaris                                                                               |

### 他言語との連携が用意
- FFI（他言語関数インターフェース）が用意されている．
- GC などの複雑なランタイムを持っていないので，Python，Ruby，Node.js などのランタイムからでも Rust の関数を簡単に呼び出せる．

## インストール
### ツールチェーンのインストール
```bash
# 以下で stable 版の Rust がインストールされる
curl https://sh.rustup.rs -sSf | sh
```

### リンカのインストール
```bash
sudo apt install gcc # Ubuntu
```

### デバッガのインストール
```bash
sudo apt install lldb # Ubuntu
# Mac のデバッガはディベロッパーツールにインストールされてる
```

## ツールチェーン
### ツールチェーンとは
- Rust で書かれたソースコードをコンパイルするのに使われるプログラミングツール群のこと．

### 構成要素
- rustc コマンド（Rust のコンパイラ）
- cargo コマンド（Rust のビルドマネージャ兼パッケージマネージャ）
- std（Rust の標準ライブラリ）

## リンカ
### リンカとは
- 正式にはリンケージエディタ．
- rustc や他の言語のコンパイラが出力したオブジェクトファイルやライブラリを結合して，ターゲット環境の ABI（Application Binary Interface）に準拠した実行可能ファイルを生成するもの．

## トレイト境界
### トレイト境界とは
- トレイト境界（trait bound）の役割は，ジェネリクスの型パラメータとして受け取れる型の範囲に境界を定めること．
- ある型がトレイト境界の範囲を超えていなければ型パラメータの実引数として受け取れ，超えているなら受け取らない．

## rustup
### rustup とは
- rustup とは Rust プロジェクトが公式にサポートしているコマンドラインツールのこと．

### 機能
- 複数バージョンの Rust ツールチェーンのインストールと管理
- クロスコンパイル用のターゲットのインストール
- RLS などの開発支援ツールのインストール

### 基本コマンド
```bash
rustup --version # rustup のバージョンを表示する
rustup show # インストール済みのツールチェーンを表示する
rustup install nightly # 最新の nightly 版をインストールする
rustup update # インストール済みのツールチェーンと rustup 自体をアップデートする
rustup default <ツールチェーン名> # デフォルトのツールチェーンを指定する
rustup run nightly rustc -V # nightly 版の rustc を実行してバージョンを表示する
rustup target add x86_64-unknown-linux-musl # クロスコンパイル用のターゲットを追加する
```

## Cargo
### Cargo とは
- Rust のビルドマネージャ兼パッケージマネージャのこと．

### 基本コマンド
```bash
cargo -h # 基本的なコマンドを表示する
cargo --list # すべてのコマンドを表示する
cargo command <コマンド名> # コマンドのヘルプドキュメントを表示する
```

| 種類                             | 基本コマンド | 機能                                                                                                                     |
| ----                             | ------------ | ----                                                                                                                     |
| パッケージの作成                 | new          | テンプレートを元に新しいパッケージを作成する．                                                                           |
| 〃                               | init         | カレントディレクトリをパッケージとして初期化する．<br>（Cargo.toml の追加と VCS の初期化のみを行う．）                   |
| パッケージのビルド，テスト，実行 | check        | ソースコードのエラーチェックを行う．                                                                                     |
| 〃                               | build        | ソースコードのエラーチェックを行い，OKならばバイナリまたはライブラ入りを生成する．                                       |
| 〃                               | run          | build を実行し，OK ならば生成されたバイナリを実行する．                                                                  |
| 〃                               | test         | テストを実行する．                                                                                                       |
| 〃                               | bench        | ベンチマークプログラムを実行する．                                                                                       |
| 〃                               | doc          | このパッケージ自体と依存するクレートのドキュメントを生成する．                                                           |
| 〃                               | clean        | target ディレクトリを削除する．                                                                                          |
| 依存クレートの管理               | update       | Cargo.lock でロックされた依存クレートのバージョンを，レジストリ（crates.io）で公開されている最新のバージョンに更新する． |
| クレートの検索，公開             | search       | レジストリに登録されたクレートを検索する．                                                                               |
| 〃                               | publish      | このパッケージ（クレート）をレジストリで公開する．                                                                       |
| Rustバイナリの管理               | install      | Rust バイナリをインストールする．                                                                                        |
| 〃                               | uninstall    | Rust バイナリをアンインストールする．                                                                                    |

### カスタムサブコマンド
- Cargo にはコマンドを追加できる．
- [Cargo の wiki ページ](https://github.com/rust-lang/cargo/wiki/Third-party-cargo-subcommands)にはサードパーティによる代表的なカスタムサブコマンドが掲載されている．
- 以下は代表的なカスタムサブコマンドの例．

| クレート名           | 追加されるコマンド                    | 機能                                                                                             |
| ----------           | ------------------                    | ----                                                                                             |
| cargo-generate       | gen                                   | 自作テンプレートからパッケージを作成する．                                                       |
| cargo-modules        | modules                               | パッケージ内のモジュールのアトリビュートや依存関係を表示する．                                   |
| cargo-count          | count                                 | パッケージ内のソースコードの行数や unsafe なコードの割合を集計する．                             |
| cargo-expand         | expand                                | マクロや #[derive] が展開された後のソースコードを表示する．                                      |
| cargo-edit           | add, upgrade, rm                      | Cargo.toml に依存クレートのエントリを追加する．                                                  |
| cargo-license        | license                               | パッケージが依存しているクレートのオープンソースライセンスを表示する                             |
| cargo-tree           | tree                                  | パッケージが依存してるクレートの情報をツリー形式で表示する．                                     |
| cargo-outdated       | outdated                              | パッケージが依存しているクレートに新しいバージョンがあるかチェックして，その情報を表示する．     |
| cargo-kcov           | kcov                                  | テストカバレージ情報を収集する（コードカバレージテスターには kcov を使用）．                     |
| cargo-tarpaulin      | tarpaulin                             | テストカバレージ情報を収集する（Ruby 製のコードカバレージテスターを使用）．                      |
| cargo-readme         | readme                                | main.rs や lib.rs の doc コメントから README.md ファイルを生成する．                             |
| cargo-release        | release                               | パッケージのリリース作業を定型化する．                                                           |
| cargo-xbuild         | xbuild                                | クロスコンパイル用のターゲットを管理する．                                                       |
| cargo-asm            | asm, llvm-ir                          | Rust ソースコードをコンパイルして得られたアセンブリコードや LLVM-IR コードを表示する．           |
| cargo-profiler       | profile callgrind, profile cachegrind | valgrind を使用してバイナリ実行時のプロファイル情報を収集する．                                  |
| cargo-bloat          | bloat                                 | 生成されるバイナリファイルの中で大きなスペースを占める関数やクレートを表示する．                 |
| cargo-local-registry | local-registry                        | ローカルのクレートリポジトリを管理する．オフラインビルドに便利．                                 |
| cargo-clone          | clone                                 | クレートのソースコードを `git clone` で取得する．                                                |
| cargo-update         | install-update                        | `cargo install` でインストールした Rust バイナリに新しいバージョンがあったらアップグレードする． |

### カスタムサブコマンド追加
```bash
# cargo-edit をインストールしてみる
sudo apt install pkg-config libssl-dev # Ubuntu ではこれらが必要
cargo install cargo-edit # cargo-edit カスタムサブコマンドをインストールする
cargo new hello # 新しいパッケージ hello を作成する
cd hello # hello ディレクトリに移動する
cargo add rand@0.6 # バージョン0.6の rand クレートをインストールする
```

## Hello World!
```bash
mkdir rust
cd rust
cargo new hello
cd hello
# バイナリを作成する
cargo build
# バイナリを走らせる
./target/debug/hello
# もしくは
cargo run
# バイナリを削除する
cargo clean
```

```rust
// ~/rust/hello/src/main.rs の中身
fn main() {
  println!("Hello, world!");
}
```

## ターミナルからのデバッグ
- デバッグには LLVM の LDB と GNU の GDB が使える．
- Rust 特有のデータ構造がきれいに表示されないことがあるため，Rust ツールチェーンに含まれている `rust-lldb` と `rust-gdb` コマンドを使用すると良い．

```bash
cd rpn # プロジェクトディレクトリに移動する
cargo build # プロジェクトをビルドする
rust-lldb ./target/degub/rpn # バイナリを対象に LLDB を実行する
(lldb) list # コードを10行ごとに表示する
(lldb) br set -f main.rs -l 27 # main.rs の27行目にブレークポイントを設置する
(lldb) br li # ブレークポイントの位置を確認する
(lldb) br disable 1 # ブレークポイントをオフにする
(lldb) r # プログラムを実行すると，ブレークポイントで停止する
(lldb) po 変数名 # ブレークポイント時点の変数が束縛されている値を見る
(lldb) c # プログラムの続きを実行する
(lldb) q # lldb を終了する
```

## Links
### 所有権の解説
  - [Rust の所有権（ownership）を語義から理解する](https://igaguri.hatenablog.com/entry/2019/08/17/184205)

### ドキュメントの日本語訳
- [Rust の日本語ドキュメント/Japanese Docs for Rust](https://doc.rust-jp.rs)
- [The Embedded Rust Book](https://tomoyuki-nakabayashi.github.io/book/intro/index.html)

### rust-lang.org の日本語化
- https://pontoon.rust-lang.org

### チートシート
- [Rust Language Cheat Sheet](https://cheats.rs)

### Rust で OS を書く
- [Writing an OS in Rust (Second Edition)](https://os.phil-opp.com)

### Rust で組込み/ベアメタルプログラミング
- https://tomoyuki-nakabayashi.github.io/embedded-rust-techniques/

### Rust 製のドキュメントビルダー
- [rust-lang/mdBook](https://github.com/rust-lang/mdBook)

### リンク
- [Rust のリンク集](https://qiita.com/mosh/items/7e327dafbe53b72ad99d)
