# Angularとは
- Typescript/JavaScriptやその他の言語を使用してモバイルおよびデスクトップWebアプリケーションを構築するための開発プラットフォーム.
- **GitHubリポジトリ:** https://github.com/angular/angular

## 用語
|用語|意味|
|---|---|
|モジュール|アプリを構成するコンポーネントを束ねたもの|
|ルートモジュール|アプリが起動する時に呼びされるモジュール|
|コンポーネント|各要素|
|デコレーター|モジュールやクラスなどの要素に対してメタ情報を付与するもの|

### デコレーターのパラメーター
|パラメーター名|意味|
|---|---|
|imports|現在のモジュールで利用する他のモジュール／コンポーネント|
|exports|現在のモジュールで外部に公開するコンポーネントなど|
|declarations|モジュール配下のコンポーネント|
|bootstrap|最初に起動すべき最上位のコンポーネント(＝ルートコンポーネント)|
|templateUrl|描画するhtml|
|template|描画する内容|
|styleUrls|どのcssを使うか|
|styles|直接cssの内容を記述する|
|selector|コンポーネントの適応先を表す(html, css, classなどとして指定できる)|


## DockerでAngular
`Dockerfile`の中身
```dockerfile
FROM node:10.15.3-alpine

RUN apk update \
  && npm install -g @angular/cli@7.3.8 \
  && rm -rf /tmp/* /var/cache/apk/* *.tar.gz ~/.npm \
  && npm cache clear --force \
  && yarn cache clean \
  && sed -i -e "s/bin\/ash/bin\/sh/" /etc/passwd

WORKDIR /app
```
`docker-compose.yml`の中身
```yaml
version: "3"
services:
  angular:
    image: solareenlo/angular-cli
    build: .
    command: ash -c "ng serve --host=0.0.0.0"
    volumes:
      - .:/app
    ports:
      - "4200:4200"
```
として, `Dockerfile`だけでコンテナを動かすときは,
```bash
# first-appを作成
docker run -it --rm -w /app -v $(pwd):/app solareenlo/angular-cli ng new first-app
# first-appディレクトリに移動
cd first-app
# コンテナに入って作業する
docker run -it --rm -w /app -v $(pwd):/app solareenlo/angular-cli sh
# コンポーネントを作成
docker run -it --rm -w /app -v $(pwd):/app solareenlo/angular-cli ng generate component sample-component
docker run -it --rm -w /app -v $(pwd):/app solareenlo/angular-cli ng g c sample-component
# コンテナを立ち上げる
docker run -d -w /app -v $(pwd):/app -p 4200:4200 solareenlo/angular-cli ng serve --host 0.0.0.0
```
で, `localhost:4200`を開く.

`Dockerfile`と`docker-compose.yml`でコンテナを動かすときは,
```bash
# first-appを作成
docker run -it --rm -w /app -v $(pwd):/app solareenlo/angular-cli ng new first-app
# first-appディレクトリに移動
cd first-app
# コンテナに入って作業する
docker run -it --rm -w /app -v $(pwd):/app solareenlo/angular-cli sh
# docker-compose を使ってコンテナを立ち上げる
docker-compose up -d
# コンテナの中に入って作業する
docker-compose exec angular sh
# コンポーネント作成
docker-compose exec angular ng generate component sample-component
docker-compose exec angular ng g c sample-component
# テストは作らずにコンポーネント作成
docker-compose exec angular ng g c sample-component --spec false
# コンポーネントの中にコンポーネントを作成
docker-compose exec angular ng g c sample-component/test --spec false
# 関連するコンテンを全て止める
docker-compose stop
# 関連するコンテナを全削除
docker-compose rm
```
で, `localhost:4200`を開く.

**コード例:** [solareenlo/angular-cli](https://github.com/solareenlo/angular-cli)

## npmの脆弱性を指摘されたら
[Angularでtarの脆弱性（Arbitrary File Overwrite）を指摘されたので修正する](https://qiita.com/disneyduffy/items/383ac95bb6a568f6360f)

## 基本的な使い方
- コンポーネント単位で作っていく.
    以下の様な感じで`xxx.component.ts`にコンポーネントの名前を`selector: 'app-servers'`で指定して, `xxx.component.html`の中で`<app-server></app-server>`と書いてどんどん使っていく.

    ```ts
    @Component({
    selector: 'app-servers',
    templateUrl: './servers.component.html',
    // template: '<app-server></app-server>',
    styleUrls: ['./servers.component.css']
    })
    ```
- 必要なコンポーネント・モジュールができたら`app.module.ts`に追加していく.
- `xxx.component.html`にhtmlを, `xxx.component.cs`にcssを, `xxx.component.ts`にjsをどんどん書いてく.

### `input`の仕方
`xxx.component.html`に以下の様に書く.
```html
<!-- eventのbindを使う方法 -->
<input
  type="text"
  class="form-control"
  (input)="onUpdateServerName($event)">
<p>{{ serverName }}</p> <!-- serverNameが表示される -->

<!--
ngModelモジュールを使う方法
これを使う場合はapp.module.tsに
---
import { FormsModule } from '@angular/forms';
@NgModule({
  imports: [
    ngModel
  ]
})
---
を追加する必要がある.
-->
<input
 type="text"
 class="form-control"
 [(ngModel)]="serverName">
<p>{{ serverName }}</p> <!-- serverNameが表示される -->
```

### ngIf else
特定の状況下でのみアプリケーションがビューまたはビューの一部を表示する様にする.
```html
<!--  *.htmlの中身 -->
<p *ngIf="serverCreated">Server was created, server name is {{ serverName }}</p>
```
```javascript
// *.tsの中身
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-servers',
  templateUrl: './servers.component.html',
  styleUrls: ['./servers.component.css']
})
export class ServersComponent implements OnInit {
  serverCreated = false;
  constructor() {}
  ngOnInit() {
  }
  onCreateServer() {
    this.serverCreated = true;
    this.serverCreationStatus = 'Server was created!';
  }
}
```

### ngStyle
コンポーネントのスタイルを操作する.
```html
<!--  *.htmlの中身
Hello World!が緑になったり赤になったりする.-->
<p [ngStyle]="{backgroundColor: getColor()}">Hello World!</p>
```
```javascript
// *.tsの中身
import { Component } from '@angular/core';
@Component({
  selector: 'app-server',
  templateUrl: './server.component.html'
})
export class ServerComponent {
  serverStatus = 'offline';
  constructor() {
    this.serverStatus = Math.random() > 0.5 ? 'online' : 'offline';
  }
  getServerStatus() {
    return this.serverStatus;
  }
  getColor() {
    return this.serverStatus === 'online' ? 'green' : 'red';
  }
}
```

### ngClass
コンポーネントのクラスを扱う.
```html
<!--  *.htmlの中身
Hello World!の背景が緑になったり赤になったりする.
背景が緑の時に文字が白色になる.
-->
<p
  [ngStyle]="{backgroundColor: getColor()}"
  [ngClass]="{online: serverStatus === 'online'}">
  Hello World!
</p>
```
```javascript
// *.tsの中身
import { Component } from '@angular/core';
@Component({
  selector: 'app-server',
  templateUrl: './server.component.html',
  styles: [`.online { color: white; }`]
})
export class ServerComponent {
  serverStatus = 'offline';
  constructor() {
    this.serverStatus = Math.random() > 0.5 ? 'online' : 'offline';
  }
  getServerStatus() {
    return this.serverStatus;
  }
  getColor() {
    return this.serverStatus === 'online' ? 'green' : 'red';
  }
}
```

### @input(), @output()
- 親→子 が`@input()`で, データを渡す
- 子→親 が`@output()`で, データを渡す

### 入力された値をコンポーネント内で操作する
`*.html`内において,
```html
<!-- ↓は親にそのまま入力した値がnewServerNameとして渡る -->
<input type="text" class="form-control" [(ngModel)]="newServerName">
<!-- ↓は子に入力された値が#serverNameInputとして残ってる -->
<input type="text" class="form-control" #serverNameInput>
```

### 要素を渡す
Aコンポーネントが利用させるときに,
Aコンポーネントのタグに挟まっている要素をAコンポーネントが取得してきて,
Aコンポーネント内の`<ng-content></ng-content>`の部分に展開する.

### コンポーネントのライフサイクル
|フック|目的とタイミング|
|---|---|
|ngOnChanges()|<li>Angularがデータバインドされた入力プロパティを(再)設定するときに応答する.<li>このメソッドは, 現在および以前のプロパティ値のSimpleChangesオブジェクトを受け取る.<li>ngOnInit()の前に呼び出され, データバインドされた入力プロパティが変更されるたびに呼び出される.|
|ngOnInit()|<li>Angularがデータバインドされたプロパティを最初に表示し, ディレクティブ/コンポーネントの入力プロパティを設定した後で, ディレクティブ/コンポーネントを初期化する.<li>最初のngOnChanges()の後に一度呼び出される.|
|ngDoCheck()|<li>Angularが検出できない, または検出できない変更を検出して, それに基づいて実行する.<li>変更検知の実行中に毎回, そしてngOnChanges()とngOnInit()の直後に呼び出される.|
|ngAfterContentInit()|<li>Angularがコンポーネントのビューあるいはディレクティブが存在するビューに, 外部コンテンツを投影した後に応答する.<li>最初のngDoCheck()の後に1度呼び出される.|
|ngAfterContentChecked()|<li>Angularがディレクティブ/コンポーネントに投影された外部コンテンツをチェックした後に応答する. <li>ngAfterContentInit()とその後全てのngDoCheck()の後に呼び出される.|
|ngAfterViewInit()|<li>Angularがコンポーネントのビューと子のビュー, あるいはディレクティブが存在するビューを初期化した後に応答する.<li>最初のngAfterContentChecked()の後に1度呼び出される.|
|ngAfterViewChecked()|<li>Angularがコンポーネントのビューと子のビュー, あるいはディレクティブが存在するビューをチェックした後に応答する.<li>ngAfterViewInit()とその後のすべてのngAfterContentChecked()の後に呼び出される.|
|ngOnDestroy()|<li>Angularがディレクティブ/コンポーネントを破棄する直前に, クリーンアップする.<li>メモリリークを回避するためにObservableの購読を解除し, イベントハンドラをデタッチしましょう.<li>Angularがディレクティブ/コンポーネントを破棄する直前に呼び出される.|
**Reference:** [ライフサイクル・フック](https://angular.jp/guide/lifecycle-hooks)
