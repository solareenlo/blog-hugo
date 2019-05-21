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

# Reduxとは
- Reactが扱うUIのstateを管理するためのフレームワーク.
- ReactはFluxを採用しているが, ReduxはFluxの概念を拡張してより扱いやすくしたもの.
- Reduxはstateを管理するフレームワークなのでReact以外にもAngularやjQueryとも併用できるがReactが一番相性が良い.

## 3大原則
- Single source of truth (信頼できる唯一の情報源)
- State in read-only (stateは常に読み取り専用にする)
- Changes are made with pure functions (actionがstateを変更する際にreducerを通して行う)

## 要素
|名前|機能|
|---|---|
|Action|入力内容を元にデータを作成する|
|ActionCreator||
|Store|データを貯める|
|State||
|Reducer|前の状態から新しい状態への純粋な関数|

## 図解
Flux

<img src="/images/redux/flux.jpg" width="60%" height="60%">

Redux

<img src="/images/redux/flow1.jpg" width="60%" height="60%">
<img src="/images/redux/flow2.png" width="60%" height="60%">

**Reference:** [UNIDIRECTIONAL USER INTERFACE ARCHITECTURES](https://staltz.com/unidirectional-user-interface-architectures.html)
