# Rustとは
- Mozillaが支援するオープンソースのシステムプログラミング言語のこと.
- 速度, 安全性, 平行性の3つのゴールにフォーカスしている.
- 主にC, C++に置き換わるものとされている.
- **GitHubリポジトリ:** https://github.com/rust-lang/rust

## Rustの特徴
- C++に匹敵する実行速度や詳細なメモリ管理が実現できる.
- 不正なメモリ領域を指すポインタなどを許容しないといったメモリ安全性を保証している.
- マルチスレッドによる並列実行時のデータ競合をコンパイル時に排除する.
- 関数型言語由来の便利な機能, 代数的データ型とパターンマッチ, 型クラスによる多相関数, コンパイラによる型推論などを取り入れている.
- ガーベージコレクタのような複雑なランタイムをもっていない.
- 多言語関数インターフェースを用いて, 他のプログラミング言語との間で相互に関数を呼び出せる.

### 速度面
- マシンコードへコンパイル
    - コンパイラがプログラムをマシンコード(プロセッサが理解できる機械語)へ変換する.
- 静的型付け言語
    - 静的型付け言語は, 変数および関数の引数や戻り値などすべての値について, その型をコンパイル時に決定する.
    - 動的型付け言語は, プログラムの実行時に実際の値を見て型を決定する.
- ゼロコスト抽象化
    - ゼロコスト抽象化とは, プログラム言語が持つ抽象化(対象から注目すべき要素を重点的に抜き出して, 他は無視する手法)の仕組みが実行時のコスト(実行速度やメモリ使用量など)なしに動作すること.
- ガーベージコレクションを行わない軽量なランタイム

## インストール
### ツールチェーンのインストール
```bash
# 以下でstable版のrustがインストールされる
curl https://sh.rustup.rs -sSf | sh
```

### リンカのインストール
```bash
# Ubuntu
sudo apt install gcc
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
