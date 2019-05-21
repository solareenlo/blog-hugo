# Reactとは
- Facebookとコミュニティによって開発されているUIに特化したJavaScriptライブラリ.
- Facebookはコアの部分だけを作成して, コミュニティがその周りをどんどん作っている.
- Virtual DOM(仮想DOM)と呼ばれるレンダリング機構を採用している.
- Virtual DOMを採用しているので, ページに表示されている広いエリアの情報をJavaScriptで管理しておき、なにか変更があった場合に変更箇所のみを再描画するUIなどに向いている.
- **GitHubリポジトリ:** https://github.com/facebook/react/
- **Reference:** [Reactとは? – React入門](https://www.to-r.net/media/react-tutorial01/)

## 用語
|名前|意味|
|---|---|
|Virtual DOM|<li>DOMを作るために必要な情報を持っているオブジェクト.<li>Virtual DOMの内容を元にHTMLを作成してブラウザに描画する.|
|JSX|<li>Virtual DOMの元となる構文.<li>Babelがscript要素の内容をブラウザが解釈できるように変換を行う.<li>JSV -> JavaScriptと変換される.<li>基本的にはHTMLと同じ記法.|
|コンポーネント|<li>個々の部品<li>HTMLのパーツをコンポーネント単位で管理し組み合わせてUIの作成を行う.|
|props|<li>親コンポーネントから渡されたプロパティ<li>不変のデータ|
|state|<li>そのコンポーネントが持っている状態<li>可変のデータ|
