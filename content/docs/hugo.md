# [Hugo](https://github.com/gohugoio/hugo)とは
静的なhtmlを生成する事ができる静的サイトジェネレータの1つ.
Go言語で書かれてる.
記事を書くだけならGo言語を知らなくても良い.
記事はMarkdownで書ける.

## .mdを削除した時に.htmlも同様に削除するオプション
```bash
hugo server -D --cleanDestinationDir
```

## MacでHugoとGitHub Pagesを使ってのサイトの作り方
[こちら]({{< relref "/posts/how-to-make-this-site.md" >}})

## Hugoのコードの色選択
- https://help.farbox.com/pygments.html
- [Chroma Style Gallery](https://xyproto.github.io/splash/docs/longer/index.html)

## 全文検索
[Search for your Hugo Website](https://gohugo.io/tools/search/)

## 見た目変更
このサイトだと`/サイトのディレクトリ/themes/book-fork/assets/`の中の.scssファイルを操作する.

## イラストの追加
1. このサイトだと`/ブログのディレクトリ/layouts/shortcodes/fontawesome.html`に
```html
<span class="inline-svg" >
{{- $fname:=print "fontawesome/" ( .Get 0 ) ".svg" -}}
{{- $path:="<path" -}}
{{- $fill:="<path fill=\"currentColor\"" -}}
{{ replace (readFile $fname) $path $fill | safeHTML }}
</span>
```
を追加する.

2. svgファイルをどこからかダウンロードしてくる.
オススメは[Fontawesome](https://fontawesome.com)の[GitHubリポジトリ](https://github.com/FortAwesome/Font-Awesome)から目当ての[svgファイル](https://github.com/FortAwesome/Font-Awesome/blob/master/svgs/brands/github.svg)を見つけて, curlとかで`/ブログのディレクトリ/content/fontawesome/`にダウンロードしてくる.
```bash
# GitHubのイラストだとこんな感じ.
curl -O https://raw.githubusercontent.com/FortAwesome/Font-Awesome/master/svgs/brands/github.svg
```

2. このサイトだと`/ブログのディレクトリ/thems/book-fork/assets/custom.scss`に
```css
.inline-svg {
  display: inline-block;
  height: 1.15rem;
  width: 1.15rem;
  top: 0.15rem;
  position: relative;
}
```
を追加する.

3. .mdファイルの中で
```markdown
{{\% fontawesome github %}}
```
と使う.

4. **Reference:** [Using Font Awesome Icons in Hugo](https://www.client9.com/using-font-awesome-icons-in-hugo/)
