---
title: "このサイトの作り方"
date: 2019-04-16T06:49:32+09:00
author: "solareenlo"
menu:
  main:
    parent: tutorials
---
***
> Macユーザー用

ものすごい初歩からこのサイトの作り方を説明しています.
この手順で作成するとGitHubに全ての内容/更新履歴/更新内容が公開されますので, 適宜読み替えてください.


## Macにgitをインストールする
https://git-scm.com/download/mac


## GitHubにアカウントを作成する
https://github.com


## MacにHomebrewをインストールする
```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
- **References:**
 - https://brew.sh/index_ja.html
 - https://github.com/Homebrew/brew


## MacにHugo(静的サイトジェネレーター)をインストールする
```bash
brew install hugo
```
- Reference: [Install Hugo](https://gohugo.io/getting-started/installing/#homebrew-macos)


### HugoのExtendedバージョンをインストールする
以下内容を`hugo_latest.sh`名で保存する.
```bash
# hugo_latest.sh
# Find the latest Hugo from GitHub
echo '🐹 Starting Hugo Install / Update 🐹'
echo '      Note: Please be sure to have curl and grep installed'
echo ''

url=$(curl -s "https://api.github.com/repositories/11180687/releases/latest" | grep -o 'https://.*hugo_extended.*_macOS-64bit.tar.gz')
echo '✅ Found latest version'

curl -s $url -L -o hugo_latest.tar.gz
echo '✅ Download complete: ' $url

tar -zxf hugo_latest.tar.gz -C /usr/local/bin
rm /usr/local/bin/README.md
rm /usr/local/bin/LICENSE
echo '✅ Extracted to /usr/local/bin'

rm hugo_latest.tar.gz
echo '✅ Removed downloaded artifacts'

echo ''
echo '👉 Current Version' $(hugo version)

echo ''
echo '🎉🎉🎉 Happy Hugo-ing! 🎉🎉🎉'
```
そして, コンソールで以下を実行する.
```bash
bash hugo_latest.sh
```
- Reference: [Install Hugo (Extended) Latest With Shell Script For macOS](https://rimdev.io/hugo-extended-latest-install-script-for-macos/)


## Hugoのテンプレートテーマであるbookをインストールする
```bash
cd
hugo new site my-site
cd my-site
git init
git submodule add https://github.com/alex-shpak/hugo-book themes/book
```

### 試しにサイトをローカルで動かしてみる.
```bash
hugo server --theme book
```
- References:
 - https://themes.gohugo.io/hugo-book/
 - https://github.com/alex-shpak/hugo-book


## GitHubにファイルを上げてみる


## 記事を更新する


## References
色の参考文献

- [aubm/hugo-code-editor-theme](https://github.com/aubm/hugo-code-editor-theme)
- [altercation/vim-colors-solarized](https://github.com/altercation/vim-colors-solarized)

形状の参考文献

- [alex-shpak/hugo-book](https://github.com/alex-shpak/hugo-book)
