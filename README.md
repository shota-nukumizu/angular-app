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

## AppModule

Angularでは、アプリケーションの商品がどのように合わさるか、アプリケーションが必要としている他のファイルやライブラリを知る必要がある。この情報をメタデータと呼ぶ。

一部のメタデータは、コンポーネントクラスに追加した`@Component`デコレータ内にある。最も重要な`@NgModule`デコレータは、トップレベルのAppModuleクラスに注釈をつける。

Angular CLIでは、プロジェクトを新規作成する際に`src/app/app.module.ts`に`AppModule`クラスを作成する。ここで`FormsModule`をオプトインする。

### FormsModuleのインポート

`AppModule`(`app.module.ts`)を開いて、`@angular/forms`ライブラリから`FormsModule`シンボルをインポートする。

`app.module.ts`

```typescript
import { FormsModule } from '@angular/forms';
```

それから、`FormModule`を`@NgModule`メタデータを`imports`配列に追加する。この配列には、アプリケーションに必要な外部モジュールのリストが含まれる。

`app.module.ts`

```typescript
imports: [
  BrowserModule,
  FormsModule
],
```

## ヒーローの編集

`HeroesComponent`テンプレートの詳細エリアをリファクタリングすると、以下のようになる。

`heroes.component.html`

```html
<div>
    <label for="name">Hero name: </label>
    <input id="name" [(ngModel)]="hero.name" placeholder="name">
</div>
```

`[(ngModel)]`はAngularの双方向データバインディング構文である。

これで`hero.name`プロパティをHTMLのテキストボックスにバインドするので、`hero.name`プロパティからテキストボックスへ、テキストボックスから`hero.name`プロパティへ双方向へデータを流せる。

## HeroesComponentの宣言

すべてのコンポーネントは、たった一つのNgModuleで宣言される必要がある。しかし、`HeroesComponent`を宣言していないのに、どうしてアプリは作動したのか？

それは、Angularが`HeroesComponent`を生成した際に、`AppModule`でそのコンポーネントの宣言を行っていたから。

`src/app/app.module.ts`

```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { FormsModule } from '@angular/forms';

import { AppRoutingModule } from './app-routing.module';
import { AppComponent } from './app.component';
import { NgbModule } from '@ng-bootstrap/ng-bootstrap';
import { HeroesComponent } from './heroes/heroes.component'; // 自動追加

@NgModule({
  declarations: [
    AppComponent,
    HeroesComponent
  ],
  imports: [
    BrowserModule,
    FormsModule,
    AppRoutingModule,
    NgbModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

## ヒーローのモックを作成

最終的には、リモートのデータサーバからそれらのヒーローを取得する。

`src/app/mock-heroes.ts`を作成し、以下のプログラムを書く。

```typescript
import { Hero } from './hero';

export const HEROES: Hero[] = [
  { id: 11, name: 'Dr Nice' },
  { id: 12, name: 'Narco' },
  { id: 13, name: 'Bombasto' },
  { id: 14, name: 'Celeritas' },
  { id: 15, name: 'Magneta' },
  { id: 16, name: 'RubberMan' },
  { id: 17, name: 'Dynama' },
  { id: 18, name: 'Dr IQ' },
  { id: 19, name: 'Magma' },
  { id: 20, name: 'Tornado' }
];
```

## ヒーローを表示する

`HeroesComponent`クラスのファイルを開いて、`HEROES`モックをインポートする。

`heroes.component.ts`

```typescript
import { Component, OnInit } from '@angular/core';
import { HEROES } from '../mock-heroes';

@Component({
  selector: 'app-heroes',
  templateUrl: './heroes.component.html',
  styleUrls: ['./heroes.component.css']
})
export class HeroesComponent implements OnInit {
  heroes = HEROES
  constructor() {}

  ngOnInit(): void {
  }

}
```

### `*ngFor`でヒーローを表示

`HeroesComponent`テンプレートを開き、次のように変更する。

`heroes.component.html`

```html
<h2>My Heroes</h2>
<ul class="heroes">
    <li>
        <span class="badge">{{ hero.id }}</span> {{ hero.name }}
    </li>
</ul>
```

これは一つのヒーローしか表示しないので、リスト化して表示するにはヒーローのリストを反復処理する必要がある。`*ngFor`を`<li>`要素に追加。

```html
<li *ngFor="let hero of heroes">
```

この際、プロパティ`hero`が存在しないので、エラーが表示されます。**この際、`ngFor`の前の`*`(アスタリスク)を必ずつける。これがないと動かないので注意しよう。**

### ヒーローの装飾

ヒーローをCSSファイル等で装飾する際には、CSSファイルとして特定の`@Component.styleUrls`配列の中で識別されるCSSファイルとして定義する。

`src/app/heroes/heroes.component.ts`

```typescript
@Component({
  selector: 'app-heroes',
  templateUrl: './heroes.component.html',
  styleUrls: ['./heroes.component.css']
})
```

## 詳細の表示

### クリックイベントのバインディング

クリックイベントのバインディングを`<li>`にこのように追加する。

`heroes.component.html`

```html
<h2>My Heroes</h2>
<ul class="heroes">
  <!--追加-->
  <li *ngFor="let hero of heroes" (click)="onSelect(hero)">
      <span class="badge">{{ hero.id }}</span> {{ hero.name }}
  </li>
</ul>
```

### クリックイベントのハンドラー

コンポーネントの`hero`プロパティを`selectedHero`にリネームするが、この場合はまだ割り当てない。

以下のようにして`onSelect()`メソッドを追加し、クリックされたヒーローをテンプレートからコンポーネントの`selectedHero`に割り当てる。

`src/app/heroes/heroes.component.ts`

```typescript
import { Component, OnInit } from '@angular/core';
import { Hero } from '../hero';
import { HEROES } from '../mock-heroes';

@Component({
  selector: 'app-heroes',
  templateUrl: './heroes.component.html',
  styleUrls: ['./heroes.component.css']
})
export class HeroesComponent implements OnInit {
  // 追加
  heroes = HEROES
  selectedHero?: Hero
  onSelect(hero: Hero):void {
    this.selectedHero = hero
  }
  // 追加ここまで
  constructor() {}

  ngOnInit(): void {
  }

}
```

### 詳細セクションの追加

現在、コンポーネントテンプレートにはリストがある。リストのヒーローをクリックして、そのヒーローの詳細を表示するには、それをテンプレートでレンダリングするための詳細セクションを追加する必要がある。

`heroes.component.html`のリストセクションの下に以下を追加。

```html
<h2>{{selectedHero.name | uppercase}} Details</h2>
<div><span>id: </span>{{selectedHero.id}}</div>
<div>
  <label for="hero-name">Hero name: </label>
  <input id="hero-name" [(ngModel)]="selectedHero.name" placeholder="name">
</div>
```

アプリケーションを実行すると、エラーが表示されてしまう。これを修正するには、`*ngIf`を使って空の`details`を非表示にする必要がある。**この際も、`ngIf`の前にある`*`を忘れないようにする。**

`src/app/heroes/heroes.component.html`

```html
<div *ngIf="selectedHero">

  <h2>{{selectedHero.name | uppercase}} Details</h2>
  <div><span>id: </span>{{selectedHero.id}}</div>
  <div>
    <label for="hero-name">Hero name: </label>
    <input id="hero-name" [(ngModel)]="selectedHero.name" placeholder="name">
  </div>

</div>
```

この際、ブラウザを更新すると名前の一覧が再度表示される。詳細のエリアは空白になっている。ヒーローのリストの中からヒーローをクリックし、詳細を表示する。

### 挙動原理

`seelctedHero`が定義されていない時、`ngIf`はDOMからヒーローの詳細を削除する。心配する`selectedHero`へのバインディングは存在しない。

ユーザがヒーローを選択すると`selectedHero`は値を持って`ngIf`はヒーローの詳細をDOMの中に挿入する。

### 選択されたヒーローを装飾する

選択されたヒーローを装飾するために、先に追加したスタイルの中にある`.selected`というCSSクラスを追加できる。ユーザがクリックした時に`.selected`クラスを`<li>`に適用するためには、クラスバインディングを使用する。

**Angularのクラスバインディングは条件に応じてCSSクラスを追加したり削除したりできる。装飾したい要素に`[class.some-css-class]="some-condition"`**を追加するだけで動く。

`heroes.component.html`

```html
<h2>My Heroes</h2>
<ul class="heroes">
    <li *ngFor="let hero of heroes" [class.selected]="hero === selectedHero" (click)="onSelect(hero)">
        <span class="badge">{{hero.id}}</span> {{hero.name}}
    </li>
</ul>

<div *ngIf="selectedHero">

    <h2>{{selectedHero.name | uppercase}} Details</h2>
    <div><span>id: </span>{{selectedHero.id}}</div>
    <div>
        <label for="hero-name">Hero name: </label>
        <input id="hero-name" [(ngModel)]="selectedHero.name" placeholder="name">
    </div>

</div>
```

`src/app/heroes/heroes.component.css`

```css
.heroes {
  margin: 0 0 2em 0;
  list-style-type: none;
  padding: 0;
  width: 15em;
}
.heroes li {
  cursor: pointer;
  position: relative;
  left: 0;
  background-color: #EEE;
  margin: .5em;
  padding: .3em 0;
  height: 1.6em;
  border-radius: 4px;
}
.heroes li:hover {
  color: #2c3a41;
  background-color: #e6e6e6;
  left: .1em;
}
.heroes li.selected {
  background-color: black;
  color: white;
}
.heroes li.selected:hover {
  background-color: #505050;
  color: white;
}
.heroes li.selected:active {
  background-color: black;
  color: white;
}
.heroes .badge {
  display: inline-block;
  font-size: small;
  color: white;
  padding: 0.8em 0.7em 0 0.7em;
  background-color:#405061;
  line-height: 1em;
  position: relative;
  left: -1px;
  top: -4px;
  height: 1.8em;
  margin-right: .8em;
  border-radius: 4px 0 0 4px;
}

input {
  padding: .5rem;
}
```

これで選択されたヒーローをハイライト表示し、その詳細を表示するSPAを簡単に開発できる。


## フィーチャーコンポーネントの作成

単一のコンポーネントにすべての機能を保持しておくと、アプリケーションが成長するにつれて維持できなくなる。大きなコンポーネントを、特定のタスクやワークフローに焦点を当てた小さなサブコンポーネントに分割したいと考えることがある。

### `HeroDetailComponent`の表示

Angular CLIを使用して、`hero-detail`という新しい名前のコンポーネントを作成する。

```
$ ng generate component hero-detail
```

### templateの記述

ヒーローの詳細が記されている`HeroesComponent`のテンプレートの下部から切り取り、`HeroDetailComponent`テンプレートに生成されたボイラープレートへ貼り付ける。

この際に貼り付けられたHTMLは`selectedHero`を参照する。新しい`HeroDetailComponent`は、選択されたヒーローだけではなく、どんなヒーローにも表示される。したがって、テンプレート内すべての`selectedHero`を`hero`に置換してください。

`src/app/hero-detail/hero-detail.component.html`

```html
<div *ngIf="hero">

    <h2>{{ hero.name | uppercase }} Details</h2>
    <div><span>id: </span>{{ hero.id }}</div>
    <div>
        <label for="hero-name">Hero name: </label>
        <input id="hero-name" [(ngModel)]="hero.name" placeholder="name">
    </div>

</div>
```

### `@Input()`heroプロパティを追加

`HerodetailComponent`クラスのファイルを追加して、`Hero`シンボルをインポートする。

`src/app/hero-detail/hero-detail.component.ts`

```typescript
import { Hero } from '../hero';
```

`hero`プロパティは`@Input()`デコレータで注釈されたinputプロパティでなければなりません。これは、外側の`HeroesComponent`がこのようにバインドするため。

```html
<app-hero-detail [hero]="selectedHero"></app-hero-detail>
```

`Input`シンボルを含めるために、`@angular/core`のimport文を修正する。

`hero-detail.component.ts`

```typescript
import { Component, OnInit, Input } from '@angular/core';
```

`@Input`デコレータが前についた`hero`プロパティを追加する。

`src/app/hero-detail/hero-detail.component.ts`

```typescript
import { Component, OnInit, Input } from '@angular/core';
import { Hero } from '../hero';

@Component({
  selector: 'app-hero-detail',
  templateUrl: './hero-detail.component.html',
  styleUrls: ['./hero-detail.component.css']
})
export class HeroDetailComponent implements OnInit {
  @Input() hero?: Hero; // 追加

  constructor() { }

  ngOnInit(): void {
  }

}

```