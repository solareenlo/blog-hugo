# Angularとは
- Typescript/JavaScriptやその他の言語を使用してモバイルおよびデスクトップWebアプリケーションを構築するための開発プラットフォーム.
- **GitHubリポジトリ:** https://github.com/angular/angular

## 用語
|用語|意味|
|---|---|
|module|アプリを構成するコンポーネントを束ねたもの|
|root module|アプリが起動する時に呼びされるモジュール|
|component|各要素|
|Decorators|モジュールやクラスなどの要素に対してメタ情報を付与するもの|
|constructor|TS, JSに付随するものでClassの初期化時に発動する|

### Decoratorsのパラメーター
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
# マテリアルデザインをインストール
docker-compose exec angular ng add @angular/material
# 関連するコンテンを全て止める
docker-compose stop
# 関連するコンテナを全削除
docker-compose rm
```
で, `localhost:4200`を開く.

**コード例:** [solareenlo/angular-cli](https://github.com/solareenlo/angular-cli)

## npmの脆弱性を指摘されたら
[Angularでtarの脆弱性（Arbitrary File Overwrite）を指摘されたので修正する](https://qiita.com/disneyduffy/items/383ac95bb6a568f6360f)

## TravisCI経由でGitHub Pagesに公開
1. 新規リポジトリを作成
- GitHub APIトークンを生成
- Travi CIの設定
- APIトークンの設定
- リポジトリをクローン
- Angular CLIで新規アプリを作成
- (単体テスト)karma.conf.jsの修正
- (総合テスト)protractor.conf.jsの修正
- .travis.ymlの追加
- GitHub Pagesにアクセス

**Reference:** [AngularアプリをTravis CIからGitHub Pagesへデプロイする](https://qiita.com/puku0x/items/0143af73c4d7af948fc8)

## マテリアルデザイン
- Googleが提唱した新しいデザインの方向性.
- 優れた古典と最新の技術と科学を組み合わせたもの.
- Angular用にもデザインコンポーネントがある.
- **公式HP:** https://material.angular.io

### インストール
```bash
npm i --save @angular/material
# or
ng add @angular/material
```

### 簡単な使い方
`app.module.ts`に以下を追加し,
```javascript
import { MatInputModule, MatCardModule, MatButtonModule } from '@angular/material';
@NgModule ({
  imports: [
    MatInputModule,
    MatCardModule,
    MatButtonModule
  ]
})
```
`.html`で, 以下の様に使用し,
```html
<mat-car>
  <mat-form-field>
    <textarea matInput [(ngModel)]="enteredValue"></textarea>
  </mat-form-field>
  <button
    mat-raised-button
    color="primary"
    (click)="onAddPost()">
    Save Post
  </button>
</mat-card>
```
`.css`に以下の様に書く.
```css
mat-card{
  width: 80;
  margin: auto;
}
mat-form-field,
textarea {
  width: 100%;
}
```

### ファイル選択ボタンをMaterial化
```css
/* .cssファイル */
.file-select-button, .file-name {
  display: inline-block;
  margin: 8px;
  }
```
```html
<!-- .htmlファイル -->
<div class="container">
  <input type="file" style="display: none" #fileInput accept="image/*" (change)="onChangeFileInput()" />
  <button mat-raised-button color="primary" class="file-select-button" (click)="onClickFileInputButton()">
    <mat-icon>attach_file</mat-icon>
    ファイルを選択
  </button>

  <p class="file-name" *ngIf="!file; else fileName">ファイルが選択されていません</p>
  <ng-template #fileName>
    <p class="file-name">{{ file?.name }}</p>
  </ng-template>
</div>
```
```javascript
// .tsファイル
import { Component, ViewChild } from '@angular/core';

@Component({
  selector: 'my-app',
  templateUrl: './app.component.html',
  styleUrls: [ './app.component.css' ]
})
export class AppComponent  {
  @ViewChild('fileInput')
  fileInput;

  file: File | null = null;

  onClickFileInputButton(): void {
    this.fileInput.nativeElement.click();
  }

  onChangeFileInput(): void {
    const files: { [key: string]: File } = this.fileInput.nativeElement.files;
    this.file = files[0];
  }
  }
```
```javascript
// app.module.tsファイル
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { FormsModule } from '@angular/forms';
import { MatButtonModule, MatIconModule } from '@angular/material';

import { AppComponent } from './app.component';
import { HelloComponent } from './hello.component';

@NgModule({
  imports:      [ BrowserModule, FormsModule, MatButtonModule, MatIconModule ],
  declarations: [ AppComponent, HelloComponent ],
  bootstrap:    [ AppComponent ]
})
export class AppModule { }
```
**Reference:** [Angular Materialでファイル選択ボタン](https://qiita.com/daikiojm/items/d744d63300915b2d8c4b)

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

## 値を渡す
### `.ts`から`.html`へ値を渡す
`.ts`側
```javascript
export class TestComponent implements OnInit {
  newPost = 'Yes';
  constructor() {}
  ngOnInit() {}
}
```
`.html`側
```html
<h1>{{ newPost }}</h1>
```

### `.ts`から`.html`の`<>`の中に値を渡す
`.ts`側
```javascript
export class TestComponent implements OnInit {
  newPost = 'Yes';
  constructor() {}
  ngOnInit() {}
}
```
`.html`側
```html
<textarea [value]="newPost"></textarea>
```

### ユーザーからの`input`を受け付ける.
1文字ずつ受け付けるのは2種類ある.  
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
    FormsModule
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

ボタンを押して受け付けるのは1つある.  
`.html`側
```html
<textarea #postInput></textarea>
<button (click)="onAddPost(postInput)></button>
<p>{{ newPost }}</p>
```
`.ts`側
```javascript
export class TestComponent implements OnInit {
  newPost = '';
  constructor() {}
  ngOninit() {}
  onAddPost(postInput: HTMLTextAreaElement) {
    this.newPost = postInput;
  }
}
```

### 入力された値をコンポーネント内で操作する
`*.html`内において,
```html
<!-- ↓は親にそのまま入力した値がnewServerNameとして渡る -->
<input type="text" class="form-control" [(ngModel)]="newServerName">
<!-- ↓は子に入力された値が#serverNameInputとして残ってる -->
<input type="text" class="form-control" #serverNameInput>
```

### 子A > 親 > 子Bと渡す
- 子A > 親 が`@output()`で, データを渡す
- 親 > 子B が`@input()`で, データを渡す

```javascript
// 子Aの.tsの中身
import { Component, EventEmitter, Output } from '@angular/core';
@Component({
  selector: 'app-achild'
})
export class AchildComponent {
  @Output() postCreated = new EventEmitter();
  onAddPost() {
    const post = 'test';
    this.postCreated.emit(post);
  }
}
```
```html
<!-- 親の.htmlの中身 -->
<!-- ここ↓は@Outoutなので, 子の要素postCreatedから親の要素onPostAddedに情報が行く -->
<app-achild (postCreated)="onPostAdded($event)"></app-achild>
<!-- ここ↓は@Input()なので, 子の要素postsに親の要素storedPostsから情報が行く -->
<app-bchild [posts]="storedPosts"></app-bchild>
```
```javascript
// 親の.tsの中身の一部
export class AppComponent {
  storedPosts = [];
  onPostAdded(post) {
    this.storedPosts.push(post);
  }
}
```
```javascript
// 子Aの.tsの中身
import { Component, Input } from '@angular/core';
@Component({
  selector: 'app-bchild'
})
export class AchildComponent {
  @Input() posts = [];
}
```

### EventEmitter
- コンポーネントからその上位コンポーネントへの, 任意の値の通知を提供する.

下位コンポーネントの`.ts`に以下の様にして使う.
```javascript
import { EventEmitter, Output } from '@angular/core';
export class TestComponent {
  @output() postCreated = new EventEmitter();
  onAddPost() {
    const post = test;
    this.postCreated.emit(post);
  }
}
```

### コンポーネント間で値を渡す
- `xxx.service.ts`を使えば良い.
- その際には`Injectable`コンポーネントを使用する.
- `xxx.service.ts`で, 渡す`class`を作って,
  それを2つのコンポーネントの`constructor`でそれぞれ作って,
  渡される方の子コンポーネントの`ngOnInit`で値を渡してあげる.
- `RxJS`ライブラリを使って, streamで非同期でコンポーネント間でデータを渡せる.

```javascript
// xxx.service.tsの例
import { Injectable } from '@angular/core';
import { Post } from './post.model';
@Injectable({providedIn: 'root'})
export class PostsService {
  private posts: Post[] = [];
  getPosts() {
    return [...this.posts];
  }
  addPosts(title: string, content: string) {
    const post: Post = {title, content};
    this.posts.push(post);
  }
}
```

## ngIf else
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

## ngStyle
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

## ngClass
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

## 要素を渡す
Aコンポーネントが利用させるときに,
Aコンポーネントのタグに挟まっている要素をAコンポーネントが取得してきて,
Aコンポーネント内の`<ng-content></ng-content>`の部分に展開する.

## コンポーネントのライフサイクル
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

## formを使う
`NgForm`モジュールを使えば良い.
```javascript
// .tsの例
import { NgForm } from `@angular/forms`;
export class TestComponent {
  onAddPost(form: NgForm) {
    if (form.invalid) {
      return ;
    }
    const post = {
      title: form.value.title;
    }
  }
}
```
```html
<!-- .htmlの例 -->
<form (submit)="onAddPost(postForm)" #postForm="ngForm">
  <mat-form-field>
    <input
      matInput
      type="text"
      name="title"
      ngModel
      required
      #title="ngModel">
    <mat-error *ngIf="title.invalid">タイトルを入力してください.</mat-error>
  </mat-form-field>
</form>
```

## デプロイ
- [超簡単にサイト公開できるAWS Amplify Consoleを使ってAngularのCI/CD環境を作ってみる](https://dev.classmethod.jp/cloud/aws-amplify-console-angular-ci-cd/)
- [AngularアプリをTravis CIからGitHub Pagesへデプロイする](https://qiita.com/puku0x/items/0143af73c4d7af948fc8)
- [Angularで作ったWebアプリをGitHubで管理してS3に自動デプロイする](https://s8a.jp/angular-github-aws-s3-auto-deploy)
