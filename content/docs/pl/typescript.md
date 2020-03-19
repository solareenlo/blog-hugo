# TypeScriptとは
- マイクロソフトによって開発されているオープンソースのプログラミング言語のこと.
- JavaScriptに, 静的型付け・クラスベースオブジェクト指向を加えた.
- **GitHubリポジトリ:** https://github.com/Microsoft/TypeScript

## 初めてのTypeScript
[solareenlo/Typescript-Practice/01_Getting_Started](https://github.com/solareenlo/Typescript-Practice/tree/master/01_Getting_Started)

### インストール
```bash
npm i --save-dev typescript
```

## TypeScript + Node.js + Docker + Circle CI
- [TypeScript + Node.js プロジェクトのはじめかた2019](https://qiita.com/notakaos/items/3bbd2293e2ff286d9f49)
- [TypeScriptでExpress.js開発するときにやることまとめ (docker/lint/format/tsのまま実行/autoreload)](https://qiita.com/yuukive/items/012bdf1b9ff3881546b3)
- [bitjson/typescript-starter](https://github.com/bitjson/typescript-starter)
- [microsoft/TypeScript-Node-Starter](https://github.com/Microsoft/TypeScript-Node-Starter)

## objectのkeyにstringを設定したら, object[key]でエラーになった時の対処法
1. tsconfigに`--suppressImplicitAnyIndexErrors`を付け加える.
   ブラケット記法でプロパティにアクセスした時に`any`型を許容するのでとっても非推奨.
- 以下のように`key`は`string`型と明示する.
  問題点は`key`が全て`string`型になること.
    ```typescript
    interface ISomeObject {
        firstKey:      string;
        secondKey:     string;
        [key: string]: string; // <-この行を追加!
    }

    const obj = {
      firstKey: "a",
      secondKey: "b",
    } as ISomeObject;

    const key: string = 'secondKey';
    const secondValue: string = obj[key];
    ```
    なのでこうする.
    が, `secondValue`の型が`string|boolean|number`型と複数の型になる.
    ```typescript
    interface ISomeObject {
        firstKey:      string;
        secondKey:     number;
        thirdKey: boolean
        [key: string]: string|boolean|number;  //<-or条件で型を指定
    }

    const key: string = 'secondKey';
    const secondValue = obj[key]; // secondValue => string|boolean|number型
    ```
- `keyof`を使う.
    ```typescript
    interface ISomeObject {
      firstKey:      string;
      secondKey:     number;
      thirdKey:      boolean;
    }

    const obj = {
      firstKey: "a",
      secondKey: 2,
      thirdKey: false
    } as ISomeObject;

    const key: keyof ISomeObject  = 'secondKey'; // ここを変更
    const secondValue = obj[key]; // secondValueがnumber型に!
    ```
  ジェネリクスを使って、type safeにブラケット記法が使える関数を作ると以下の感じになる.
    ```typescript
    const accessByBracket = <S, T extends keyof S>(obj: S, key: T) => {
      return obj[key];
    };

    const obj = {
      firstKey: "a",
      secondKey: 2,
      thirdKey: false
    } as ISomeObject;

    const value = accessByBracket(obj, 'secondKey'); // value => 2
    ```

**Reference:** [Typescript ブラケット記法(Object[key])でno index signatureエラーをtype safeに解決したい。](https://aknow2.hatenablog.com/entry/2018/11/14/143033)
