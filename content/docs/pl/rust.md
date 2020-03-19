# Rustとは
- Mozillaが支援するオープンソースのシステムプログラミング言語のこと.
- 速度, 安全性, 平行性の3つのゴールにフォーカスしている.
- 主にC, C++に置き換わるものとされている.
- **GitHubリポジトリ:** https://github.com/rust-lang/rust

## Rustの特徴
### 概要
- C++に匹敵する実行速度や詳細なメモリ管理が実現できる.
- 不正なメモリ領域を指すポインタなどを許容しないといったメモリ安全性を保証している.
- マルチスレッドによる並列実行時のデータ競合をコンパイル時に排除する.
- 関数型言語由来の便利な機能, 代数的データ型とパターンマッチ, 型クラスによる多相関数, コンパイラによる型推論などを取り入れている.
- ガーベージコレクタのような複雑なランタイムをもっていない.
- 多言語関数インターフェースを用いて, 他のプログラミング言語との間で相互に関数を呼び出せる.

### 速度面
- **マシンコードへコンパイル**
    - コンパイラがプログラムをマシンコード(プロセッサが理解できる機械語)へ変換する.
- **静的型付け言語**
    - 静的型付け言語は, 変数および関数の引数や戻り値などすべての値について, その型をコンパイル時に決定する.
    - 動的型付け言語は, プログラムの実行時に実際の値を見て型を決定する.
- **ゼロコスト抽象化**
    - ゼロコスト抽象化とは, プログラム言語が持つ抽象化(対象から注目すべき要素を重点的に抜き出して, 他は無視する手法)の仕組みが実行時のコスト(実行速度やメモリ使用量など)なしに動作すること.
- **ガーベージコレクションを行わない軽量なランタイム**

### 安全面
- Rustはコンパイル時の静的解析により以下の安全性を保証している.
- **型安全性**
    - 型安全性とは, 正しく型付けされたプログラムが不正な動作(未定義動作とも言う)をしないよう言語が定義されていること.
- **メモリ安全性**
    - RustはCのようにメモリ内容への柔軟なアクセスができるが, Cと違い以下のようなメモリ安全性を保証する.
    - データの転記の際のメモリ領域あふれを防ぐ.
    - ポインタによる誤ったメモリ境域へのアクセスを防ぐ.
    - 初期化前のメモリ領域へのアクセスを防ぐ.
    - 解放後のメモリ領域へのアクセスを防ぐ.
- **マルチスレッドプログラミングにおけるデータ競合の回避**
    - Rustでは型安全性とメモリ安全性に用いられるコンパイラの静的解析機能を使い, データ競合の可能性を検出できる.
    - 故にコンパイルに成功したら, そのプログラムにはデータ競合が無いことが保証される.
    - Rustではスレッド間でデータを受け渡したり共有したりするために, `チャネル`・`ロック`・`配列などの範囲`・`イミュータブルな参照`などの方法が用意されている.
- **アンセーフなコードのサポート**
    - FFI(多言語関数インターフェース)経由で多言語の関数を呼び出した時, コンパイルはその多言語の関数の安全性を確認できない.
    - 内向きのミュータビリティのような, 安全性の検査をコンパイル時ではなく実行時に延期する仕組みがライブラリとして実行されてる.
    - これらに対するサポートも用意されている.

### 生産性を高めるモダンな機能
- 以下はRustのモダンな機能の一部

| 機能 | 概要 |
| --- | ---- |
| 強力な型推論 | Rustコンパイルには強力な型推論が備わっており, 変数を導入するlet文だけでなく, 途中の式で型が決まる状況なら, 型を明示しなくて良い. |
| 代数的データ型 | 代数的データ方はHaskellなどの関数型言語にみられる機能でリッチなデータ構造が表現できる.<br>Rustの列挙型(enum)はその名前とは裏腹に, 代数的データ型を構成する列挙型, 直積型, 直和型のすべてを表現できる. |
| パターンマッチ | 多くの言語に存在するswitch文の代わりに, 関数型言語由来で表現力の高いmatch式を持つ.<br>列挙型のバリアントの判定と同時に各データフィールドの値を複数の変数に分配できる. |
| トレイトによるポリフォーリズム | トレイトは型が持つべき性質(実装すべきメソッド)を定義する.<br>一見するとJavaのinterfaceeに似ているが, 実際にはHaskellの型クラスに近く, 柔軟性が高い. |

### シングルバイナリ・クロスバイナリ
- アプリケーションをビルドするとシングルバイナリ(単一の実行可能ファイル)が生成される.
- Rustはクロスコンパイルをして幅広いプラットフォームに向けたバイナリも生成できる.
- Rustは以下のプラットフォームに対応したバイナリを生成できる.

| プロセッサ・アーキテクチャ | オペレーティングシステム |
| -------------------------- | ------------------------ |
| x86系 | Linux, macOX, Windows, BSD系(FreeBSD, NetBSD, OpenBSDなど), Solaris, RedoxOS, Fuchsia |
| ARM系 | Android, iOS, Linux, Fuchsia |
| WebAssembly, asm.js | WebブラウザやNode.jsのようなJavaScriptランタイム |
| マイコン(ARM Cortex-Mシリーズなど) | ベアメタル(OSなし) |
| 汎用GPU | NVIDIA PTX中間命令 |
| MIPS, S390x, PowerPC | Linux |
| SPARC | Solaris |

### 他言語との連携が用意
- FFI(他言語関数インターフェース)が用意されている.
- GCなどの複雑なランタイムを持っていないので, Python, Ruby, Node.jsなどのランタイムからでもRustの関数を簡単に呼び出せる.

## インストール
### ツールチェーンのインストール
```bash
# 以下でstable版のrustがインストールされる
curl https://sh.rustup.rs -sSf | sh
```

### リンカのインストール
```bash
sudo apt install gcc # Ubuntu
```

### デバッガのインストール
```bash
sudo apt install lldb # Ubuntu
# Macのデバッガはディベロッパーツールにインストールされてる
```

## ツールチェーン
### ツールチェーンとは
- Rustで書かれたソースコードをコンパイルするのに使われるプログラミングツール群のこと.

### 構成要素
- rustcコマンド(Rustのコンパイラ)
- cargoコマンド(Rustのビルドマネージャ兼パッケージマネージャ)
- std(Rustの標準ライブラリ)

## リンカ
### リンカとは
- 正式にはリンケージエディタ.
- rustcや他の言語のコンパイラが出力したオブジェクトファイルやライブラリを結合して, ターゲット環境のABI(Application Binary Interface)に準拠した実行可能ファイルを生成するもの.

## トレイト境界
### トレイト境界とは
- トレイト境界(trait bound)の役割は, ジェネリクスの型パラメータとして受け取れる型の範囲に境界を定めること.
- ある型がトレイト境界の範囲を超えていなければ型パラメータの実引数として受け取れ, 超えているなら受け取らない.

## rustup
### rustupとは
- rustupとはRustプロジェクトが公式にサポートしているコマンドラインツールのこと.

### 機能
- 複数バージョンのRustツールチェーンのインストールと管理
- クロスコンパイル用のターゲットのインストール
- RLSなどの開発支援ツールのインストール

### 基本コマンド
```bash
rustup --version # rustupのバージョンを表示する
rustup show # インストール済みのツールチェーンを表示する
rustup install nightly # 最新のnightly版をインストールする
rustup update # インストール済みのツールチェーンとrustup自体をアップデートする
rustup default <ツールチェーン名> # デフォルトのツールチェーンを指定する
rustup run nightly rustc -V # nightly版のrustcを実行してバージョンを表示する
rustup target add x86_64-unknown-linux-musl # クロスコンパイル用のターゲットを追加する
```

## Cargo
### Cargoとは
- Rustのビルドマネージャ兼パッケージマネージャのこと.

### 基本コマンド
```bash
cargo -h # 基本的なコマンドを表示する
cargo --list # すべてのコマンドを表示する
cargo command <コマンド名> # コマンドのヘルプドキュメントを表示する
```
| 種類 | 基本コマンド | 機能 |
| ---- | ------------ | ---- |
| パッケージの作成 | new | テンプレートを元に新しいパッケージを作成する. |
| 〃 | init | カレントディレクトリをパッケージとして初期化する.<br>(Cargo.tomlの追加とVCSの初期化のみを行う.) |
| パッケージのビルド, テスト, 実行 | check | ソースコードのエラーチェックを行う. |
| 〃 | build | ソースコードのエラーチェックを行い, OKならばバイナリまたはライブラ入りを生成する. |
| 〃 | run | buildを実行し, OKならば生成されたバイナリを実行する. |
| 〃 | test | テストを実行する. |
| 〃 | bench | ベンチマークプログラムを実行する. |
| 〃 | doc | このパッケージ自体と依存するクレートのドキュメントを生成する. |
| 〃 | clean | targetディレクトリを削除する. |
| 依存クレートの管理 | update | Cargo.lockでロックされた依存クレートのバージョンを, レジストリ(crates.io)で公開されている最新のバージョンに更新する. |
| クレートの検索, 公開 | search | レジストリに登録されたクレートを検索する. |
| 〃 | publish | このパッケージ(クレート)をレジストリで公開する. |
| Rustバイナリの管理 | install | Rustバイナリをインストールする. |
| 〃 | uninstall | Rustバイナリをアンインストールする. |

### カスタムサブコマンド
- Cargoにはコマンドを追加できる.
- [Cargoのwikiページ](https://github.com/rust-lang/cargo/wiki/Third-party-cargo-subcommands)にはサードパーティによる代表的なカスタムサブコマンドが掲載されている.
- 以下は代表的なカスタムサブコマンドの例.

| クレート名 | 追加されるコマンド | 機能 |
| ---------- | ------------------ | ---- |
| cargo-generate | gen | 自作テンプレートからパッケージを作成する. |
| cargo-modules | modules | パッケージ内のモジュールのアトリビュートや依存関係を表示する. |
| cargo-count | count | パッケージ内のソースコードの行数やunsafeなコードの割合を集計する. |
| cargo-expand | expand | マクロや#[derive]が展開された後のソースコードを表示する. |
| cargo-edit | add, upgrade, rm | Cargo.tomlに依存クレートのエントリを追加する. |
| cargo-license | license | パッケージが依存しているクレートのオープンソースライセンスを表示する |
| cargo-tree | tree | パッケージが依存してるクレートの情報をツリー形式で表示する. |
| cargo-outdated | outdated | パッケージが依存しているクレートに新しいバージョンがあるかチェックして, その情報を表示する. |
| cargo-kcov | kcov | テストカバレージ情報を収集する(コードカバレージテスターにはkcovを使用). |
| cargo-tarpaulin | tarpaulin | テストカバレージ情報を収集する(Ruby製のコードカバレージテスターを使用). |
| cargo-readme | readme | main.rsやlib.rsのdocコメントからREADME.mdファイルを生成する. |
| cargo-release | release | パッケージのリリース作業を定型化する. |
| cargo-xbuild | xbuild | クロスコンパイル用のターゲットを管理する. |
| cargo-asm | asm, llvm-ir | Rustソースコードをコンパイルして得られたアセンブリコードやLLVM-IRコードを表示する. |
| cargo-profiler | profile callgrind, profile cachegrind | valgrindを使用してバイナリ実行時のプロファイル情報を収集する. |
| cargo-bloat | bloat | 生成されるバイナリファイルの中で大きなスペースを占める関数やクレートを表示する. |
| cargo-local-registry | local-registry | ローカルのクレートリポジトリを管理する. オフラインビルドに便利. |
| cargo-clone | clone | クレートのソースコードを`git clone`で取得する. |
| cargo-update | install-update | `cargo install`でインストールしたRustバイナリに新しいバージョンがあったらアップグレードする. |

### カスタムサブコマンド追加
```bash
# cargo-editをインストールしてみる
sudo apt install pkg-config libssl-dev # Ubuntuではこれらが必要
cargo install cargo-edit # cargo-editカスタムサブコマンドをインストールする
cargo new hello # 新しいパッケージhelloを作成する
cd hello # helloディレクトリに移動する
cargo add rand@0.6 # バージョン0.6のrandクレートをインストールする
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
// ~/rust/hello/src/main.rsの中身
fn main() {
  println!("Hello, world!");
}
```

## ターミナルからのデバッグ
- デバッグにはLLVMのLDBとGNUのGDBが使える.
- Rust特有のデータ構造がきれいに表示されないことがあるため, Rustツールチェーンに含まれている`rust-lldb`と`rust-gdb`コマンドを使用すると良い.

```bash
cd rpn # プロジェクトディレクトリに移動する
cargo build # プロジェクトをビルドする
rust-lldb ./target/degub/rpn # バイナリを対象にLLDBを実行する
(lldb) list # コードを10行ごとに表示する
(lldb) br set -f main.rs -l 27 # main.rsの27行目にブレークポイントを設置する
(lldb) br li # ブレークポイントの位置を確認する
(lldb) br disable 1 # ブレークポイントをオフにする
(lldb) r # プログラムを実行すると, ブレークポイントで停止する
(lldb) po 変数名 # ブレークポイント時点の変数が束縛されている値を見る
(lldb) c # プログラムの続きを実行する
(lldb) q # lldbを終了する
```

## 所有権の解説
- [Rustの所有権（ownership）を語義から理解する](https://igaguri.hatenablog.com/)

## ドキュメントの日本語訳
- [Rustの日本語ドキュメント/Japanese Docs for Rust](https://doc.rust-jp.rs)
- [The Embedded Rust Book](https://tomoyuki-nakabayashi.github.io/book/intro/index.html)

## rust-lang.orgの日本語化
- https://pontoon.rust-lang.org

## チートシート
- [Rust Language Cheat Sheet](https://cheats.rs)

## RustでOSを書く
- [Writing an OS in Rust (Second Edition)](https://os.phil-opp.com)

## リンク
- [Rustのリンク集](https://qiita.com/mosh/items/7e327dafbe53b72ad99d)