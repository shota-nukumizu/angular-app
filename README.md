# Angular

Angularで簡単なアプリを開発。(チュートリアル)

公式サイト：[Angular](https://angular.jp/)

# インストール

```
$ ng new angular-app
$ cd angular-app
$ ng serve --open
```

これで`http://localhost:4200/`にアクセスできる。(開発者サーバの立ち上げ)

# Bootstrapのインストール

公式サイト：[Angular powered Bootstrap](https://ng-bootstrap.github.io/#/home)

```
$ ng add @ng-bootstrap/ng-bootstrap
```

# Angularアプリの仕組み

## アプリケーションのタイトル変更

`src/app/app.component.ts`：アプリケーションのタイトルを変更

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'Tour of Heroes';
}
```

`src/app/app.component.html`：コンポーネントのテンプレートファイル。

Angular CLIで生成されたデフォルトのテンプレートを削除する。代わりに以下のHTMLを置く。

```html
<h1>{{ title }}</h1>
```