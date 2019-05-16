# [Angular](https://github.com/angular/angular)とは
Typescript/JavaScriptやその他の言語を使用してモバイルおよびデスクトップWebアプリケーションを構築するための開発プラットフォーム.

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
docker run -it --rm -w /app -v $(pwd):/app solareenlo/angular-cli ng g component sample-component
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
