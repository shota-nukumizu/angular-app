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

AugularではVueと同様に`{{}}`で変数を挿入する。この際、ブラウザがページを更新して新しいアプリのタイトルが表示される。

## アプリケーションのスタイルを追加する

大半のアプリケーションは、アプリ全体で一貫した見た目を担保している。その際、空の`src/style.css`を追加する。

`src/styles.css`：アプリ全体のデザインを書く。

```css
/* Application-wide Styles */
h1 {
  color: #369;
  font-family: Arial, Helvetica, sans-serif;
  font-size: 250%;
}
h2, h3 {
  color: #444;
  font-family: Arial, Helvetica, sans-serif;
  font-weight: lighter;
}
body {
  margin: 2em;
}
body, input[type="text"], button {
  color: #333;
  font-family: Cambria, Georgia, serif;
}
/* everywhere else */
* {
  font-family: Arial, Helvetica, sans-serif;
}
```

## エディタの作成

### heroesコンポーネントの作成

Angular CLIを活用して、`heroes`という名前の新しいコンポーネントを生成する。

```
$ ng generate component heroes
```

CLIは`src/app/heroes/`という新しいフォルダを作成し、`HeroesComponent`に関する3つのファイルをテストファイルと一緒に生成する。

`HeroesComponent`のクラスファイルは以下の通り。

`app/heroes/heroes.component.ts`

```typescript
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-heroes',
  templateUrl: './heroes.component.html',
  styleUrls: ['./heroes.component.css']
})
export class HeroesComponent implements OnInit {

  constructor() { }

  ngOnInit(): void {
  }

}
```

このとき、常にAngularコアライブラリから`Component`シンボルをインポートし、コンポーネントクラスに`@Component`で注釈をつける。

`@Component`はコンポーネントのAngularメタデータを指定するデコレータ関数である。

CLIは次の３つのメタデータプロパティを生成する。

* `selector`―コンポーネントのCSS要素セレクタ
* `templateUrl`―コンポーネントのテンプレートファイルの場所
* `styleUrls`―コンポーネントのプライベートCSSスタイルの場所

`ngOnInit()`はライフサイクルフックである。**ライフサイクルフックとは、Angularがコンポーネントクラスをインスタンス化してコンポーネントビューとその子ビューをレンダリングするときに開始する機能**である。

### heroプロパティの追加

`src/app/heroes/heroes.component.ts`

```typescript
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-heroes',
  templateUrl: './heroes.component.html',
  styleUrls: ['./heroes.component.css']
})
export class HeroesComponent implements OnInit {
  hero = 'Windstorm'
  constructor() {}

  ngOnInit(): void {
  }

}
```

### ヒーローを表示する

`hero.component.html`テンプレートファイルを開く。Angular CLIで生成されたデフォルトのテキストを削除し、それを新しい`hero`プロパティへのデータバインディングに置換する。

```html
<p>heroes works!</p>
<h2>{{ hero }}</h2>
```

## HeroesComponentビューの表示

`HeroesComponent`を表示するには、それをアプリケーションシェルの`AppComponent`のテンプレートに追加する必要がある。

`AppComponent`のテンプレートファイルで、タイトルの直下に`<app-heroes>`要素を追加する。

```html
<h1>{{ title }}</h1>
<app-heroes></app-heroes>
```

これでコンポーネントビューを表示できる。

## Heroインターフェイスの作成

`src/app`フォルダ内に`hero.ts`を作成し、Heroインターフェイスを作成する。それに`id`と`name`をそれぞれ与える。

`src/app/hero.ts`

```typescript
export interface Hero {
    id: number
    name: string
}
```

`HeroesComponent`クラスに戻って、`Hero`インターフェイスをインポート。コンポーネントの`hero`プロパティを`Hero`型に**リファクタリング(ソフトウェアの挙動を変えることなく、その内部構造を整理すること)**する。それを`1`という`id`と`Windstorm`という`name`で初期化する。

`HeroesComponent`のクラスファイルは以下の通り。

`src/app/heroes/heroes.component.ts`

```typescript
import { Component, OnInit } from '@angular/core';
import { Hero } from '../hero'; //hero.tsからHeroインターフェイスをインポート

@Component({
  selector: 'app-heroes',
  templateUrl: './heroes.component.html',
  styleUrls: ['./heroes.component.css']
})
// この際、ヒーローを文字列からオブジェクトに変更したので、ページが正しく表示されない。
export class HeroesComponent implements OnInit {
  hero: Hero = {
    id: 1,
    name: 'Windstorm'
  }
  constructor() {}

  ngOnInit(): void {
  }

}
```

## ヒーローオブジェクトの表示

`heroes.component.html`

```html
<p>heroes works!</p>
<h2>{{ hero.name }} Details</h2>
<div>
    <span>id: </span>{{ hero.id }}
</div>
<div>
    <span>name: </span>{{ hero.name }}
</div>
```

## UppercasePipeで書式設定

`hero.name`のバインディングを以下のように修正する。

`heroes.component.html`

```html
<h2>{{ hero.name | uppercase }} Details</h2>
```

ブラウザが更新され、ヒーローの名前が大文字で表示されるようになる。

パイプは、文字列、通貨金額、日付やその他の表示データの書式設定に最適。Angularには複数のパイプが備わっているので、オリジナルのパイプを作成できる。