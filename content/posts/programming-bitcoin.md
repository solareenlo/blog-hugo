---
title: "Programming BitcoinをHTMLで読んでみる"
date: 2019-04-21T08:00:00+09:00
author: "solareenlo"
tags: [
    "Bitcoin",
    "Python",
    "Ruby"
]
categories: [
  "Bitcoin",
  "Programming"
]
menu:
  main:
    parent: tutorials
---

## 前提条件
- RubyがPCにインストールされている.
- RubyのパッケージマネージャーのgemがPCにインストールされている.

## HTML作成
```bash
git clone git@github.com:jimmysong/programmingbitcoin.git
cd programmingbitcoin
gem install asciidoctor
find . -name \*.asciidoc -print0 | xargs -0 -n1 asciidoctor
```
これで.asciidocが.htmlに変換されて出力されるので, 任意のブラウザで開いて読む.

## References
- [「Programming Bitcoin」を読んだ](https://kiririmode.hatenablog.jp/entry/20190120/1547938372)
- [jimmysong/programmingbitcoin](https://github.com/jimmysong/programmingbitcoin)
- [脱Word、脱Markdown、asciidocでドキュメント作成する際のアレコレ](https://qiita.com/tamikura@github/items/5d3f62dae55617ee42bb)
