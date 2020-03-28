# Hugo とは
- 静的な html を生成する事ができる静的サイトジェネレータの1つ．
- Go 言語で書かれてる．
- 記事を書くだけなら Go 言語を知らなくても良い．
- 記事は Markdown で書ける．
- **GitHub リポジトリ:** https://github.com/gohugoio/hugo

## .md を削除した時に .html も同様に削除するオプション
```bash
hugo server -D --cleanDestinationDir
```

## Mac で Hugo と GitHub Pages を使ってのサイトの作り方
[こちら](/posts/how-to-make-this-site.md)

## Hugo のコードの色選択
- https://help.farbox.com/pygments.html
- [Chroma Style Gallery](https://xyproto.github.io/splash/docs/longer/index.html)

## 全文検索
[Search for your Hugo Website](https://gohugo.io/tools/search/)

## 見た目変更
このサイトだと `/サイトのディレクトリ/themes/book-fork/assets/` の中の .scss ファイルを操作する．

## イラストの追加
1. このサイトだと `/ブログのディレクトリ/layouts/shortcodes/fontawesome.html` に
    ```html
    <span class="inline-svg" >
    {{- $fname:=print "fontawesome/" ( .Get 0 ) ".svg" -}}
    {{- $path:="<path" -}}
    {{- $fill:="<path fill=\"currentColor\"" -}}
    {{ replace (readFile $fname) $path $fill | safeHTML }}
    </span>
    ```
    を追加する．

2. svg ファイルをどこからかダウンロードしてくる．
オススメは [Fontawesome](https://fontawesome.com) の [GitHub リポジトリ](https://github.com/FortAwesome/Font-Awesome)から目当ての [svg ファイル](https://github.com/FortAwesome/Font-Awesome/blob/master/svgs/brands/github.svg)を見つけて，curl とかで `/ブログのディレクトリ/content/fontawesome/` にダウンロードしてくる．
    ```bash
    # GitHub のイラストだとこんな感じ．
    curl -O https://raw.githubusercontent.com/FortAwesome/Font-Awesome/master/svgs/brands/github.svg
    ```

2. このサイトだと `/ブログのディレクトリ/thems/book-fork/assets/custom.scss` に
    ```css
    .inline-svg {
      display: inline-block;
      height: 1.15rem;
      width: 1.15rem;
      top: 0.15rem;
      position: relative;
    }
    ```
    を追加する．

3. .md ファイルの中で
    ```markdown
    {{\% fontawesome github %}}
    ```
    と使う．

4. **Reference:** [Using Font Awesome Icons in Hugo](https://www.client9.com/using-font-awesome-icons-in-hugo/)

## GitHub Actions による GitHub Pages への自動デプロイ
### 1. YAML を作成する
- 以下の YAML ファイルを master ブランチの `.github/workflows/gh-pages.yml` として Push すると自動で gh-pages で公開してくれるようになる．
    ```yaml
    name: github pages

    on:
      push:
        branches:
          - master

    jobs:
      build-deploy:
        runs-on: ubuntu-18.04
        steps:
          - uses: actions/checkout@v2
            with:
              submodules: true

          - name: Setup Hugo
            uses: peaceiris/actions-hugo@v2
            with:
              hugo-version: '0.64.0'

          - name: Build
            run: hugo --minify

          - name: Deploy
            uses: peaceiris/actions-gh-pages@v3
            with:
              deploy_key: ${{ secrets.ACTIONS_DEPLOY_KEY }}
              publish_dir: ./public
    ```
- **References:**
    - GitHub: [peaceiris/actions-gh-pages](https://github.com/peaceiris/actions-gh-pages)
    - GitHub: [peaceiris/actions-hugo](https://github.com/peaceiris/actions-hugo)

### 2. デプロイトークンを設定する
- 以下のコマンドでデプロイトークン用の秘密鍵と公開鍵のペアを作成する．
    ```bash
    ssh-keygen -t rsa -b 4096 -C "$(git config user.email)" -f gh-pages -N ""
    ```
- 出来上がった `gh-pages.pub` （公開鍵）を GitHub リポジトリの settings の `Deploy keys` に `Allow write access` オプションと共に登録する．
- 出来上がった `gh-pages` （秘密鍵）を GitHub リポジトリの settings の `Secrets` に `ACTIONS_DEPLOY_KEY` として登録する．

### 3. Reference
- Qiita: [GitHub Actions による GitHub Pages への自動デプロイ](https://qiita.com/peaceiris/items/d401f2e5724fdcb0759d)
