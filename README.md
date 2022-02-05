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

<b>Angularのクラスバインディングは条件に応じてCSSクラスを追加したり削除したりできる。装飾したい要素に`[class.some-css-class]="some-condition"`を追加するだけ</b>で動く。

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

## `HeroDetailComponent`を表示する

### `HeroesComponent`テンプレートの更新

`HeroDetailComponent`のセレクターは`app-hero-detail`だ。

ヒーローの詳細ビューがかつて存在した`HeroesComponent`テンプレートの下部に`<app-hero-detail>`を追加する。

以下のように、`HeroesComponent.selectedHero`を、この要素の`hero`プロパティにバインドさせる。

`heroes.component.html`

```html
<app-hero-detail [hero]="selectedHero"></app-hero-detail> 
```

`[hero]="selectedHero"`はAngularのプロパティバインディング。

これは、`HeroesComponent`の`selectedHero`プロパティから、ターゲット要素の`hero`プロパティへの単方向データバインディングである。これは、`HeroDetailComponent`の`hero`プロパティがマッピングされる。

ユーザがリスト内のヒーローをクリックすると、`selectedHero`が変更される。`selectedHero`が変更されると、プロパティバインディングは`hero`を更新して、`HeroDetailComponent`は新しいヒーローを表示する。

`heroes.component.html`

```html
<h2>My Heroes</h2>

<ul class="heroes">
  <li *ngFor="let hero of heroes"
    [class.selected]="hero === selectedHero"
    (click)="onSelect(hero)">
    <span class="badge">{{hero.id}}</span> {{hero.name}}
  </li>
</ul>

<app-hero-detail [hero]="selectedHero"></app-hero-detail>
```

### 変更点

以前は、ユーザがヒーロー名をクリックする際に、ヒーローのリストの下にヒーローの詳細が表示されていた。

今では`HeroDetailComponent`が`HeroesComponent`の代わりにそれらの詳細を示している。

* `HeroComponent`のスコープを減少。
* 親の`HeroesComponent`に触れることなく、`HeroDetailComponent`をリッチなヒーローエディタに進化。
* ヒーローの詳細ビューに触れることなく。`HeroesComponent`を進化。
* 将来のコンポーネントのテンプレートで、`HeroDetailComponent`を再利用。

## サービスの追加

アプリケーションを開発する際に、コンポーネント内で直接データの保存や取得を行うべきではない。コンポーネントはデータの受け渡しだけに集中し、その他の処理はサービスクラスへ委譲するべき。

## `HeroService`の作成

Angular CLIを作成し、`Hero Service`を作成する。

```
$ ng generate service hero
```

このコマンドは`HeroService`のスケルトンファイルを`src/app/hero.serive.ts`に以下のように保存する。

```typescript
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root'
})
export class HeroService {

  constructor() { }
}
```

### `@Injectable()`サービス

生成されたファイルの中でAngularのInjectableシンボルがインポートされ、`@Injectable()`デコレータとしてクラスを注釈することに注目する。これは、クラスを依存関係システムに参加するものとしてマークする。`HeroSerive`クラスは、注入可能なサービスを提供する予定で、それ自身が依存関係を持てる。

### ヒーローデータの取得

`HeroService`は様々な場所からヒーローデータを取得することがある。

コンポーネントからデータ取得ロジックを切り離すことで、そのようなサービス側の事情に関係なくいつでも実装方針の変更ができます。コンポーネント側は、サービスがどのように動いているのか関係ありません。

`Hero`と`HEROES`をインポート。

`src/app/hero.service.ts`

```typescript
import { Hero } from './hero';
import { HEROES } from './mock-heroes';
```

`getHeroes`メソッドを追加し、モックヒーローを完成する。

`src/app/hero.service.ts`

```typescript
import { Injectable } from '@angular/core';
import { Hero } from './hero';
import { HEROES } from './mock-heroes';

@Injectable({
  providedIn: 'root'
})
export class HeroService {

  // 追加
  getHeroes(): Hero[] {
    return HEROES
  }

  constructor() { }
}
```

## `HeroService`の提供

Angularが`HeroesComponent`へ注入する前に、プロバイダを登録することで`HeroService`が依存性の注入システムで利用できる必要がある。プロバイダーとは、サービスを作成あるいは提供できるもの。この場合は、`HeroService`クラスをインスタンス化してサービスを提供する。

`HeroService`をインジェクター(必要な場所でプロバイダーを選択して注入するためのオブジェクト)に登録することで、サービスを提供できるようになる。

デフォルトでは、Angular CLIコマンド`ng generate service`は、プロバイダーのメタデータ、**言い換えれば`providedIn: 'root'`を`@Injectable()`デコレータに含めることで、プロバイダーをサービスのルートインジェクターに登録できる。**

```typescript
@Injectable({
  providedIn: 'root',
})
```

ルートレベルでサービスを提供すると、Angularは`HeroService`の単一の共有インスタンスを作成し、それを要求する任意のクラスに注入する。`@Injectable`メタデータでプロバイダーを登録すると、Angularはサービスが使用されなくなった際にそれを削除することでアプリケーションを最適化できる。

## `HeroComponent`の更新

`heroes.component.ts`

```typescript
import { Component, OnInit } from '@angular/core';
import { Hero } from '../hero';
import { HeroService } from '../hero.service'; // HEROESのインポートを削除し、HeroServiceを代わりにインポートする。

@Component({
  selector: 'app-heroes',
  templateUrl: './heroes.component.html',
  styleUrls: ['./heroes.component.css']
})
export class HeroesComponent implements OnInit {
  heroes: Hero[] = [] // heroesプロパティの定義を宣言に置換する
  selectedHero?: Hero
  onSelect(hero: Hero):void {
    this.selectedHero = hero
  }
  constructor(private heroService: HeroService) {} // HeroServiceの注入

  getHeroes(): void {
    this.heroes = this.heroService.getHeroes() // サービスからヒーローデータを取得するためのメソッドを作成する
  }

  ngOnInit(): void {
    this.getHeroes() // 作成したメソッドはngOnInit()で呼び出す。
  }

}
```

【補足】<br>
**`getHeroes()`はコンストラクターでも呼び出せるが、これは最適な方法ではない。**

原則、Angularにおけるコンストラクターではプロパティ定義の簡単な初期化だけを行い、それ以外は何もするべきではない。実際にデータを取得する際に行うサーバへのHTTPリクエストを行う関数は呼び出すべきではない。

**`getHeroes()`はコンストラクターではなく、`ngOnInit`ライフサイクルフックで呼び出す。**

## Observableデータ

`HeroService.getHeroes()`は同期的なメソッドで、これは`HeroService`が即座にヒーローデータを取得できることを意味する。

また、`HeroesComponent`は`getHeroes()`の返り値がまるで同期的に取得できるかのように扱える。

`src/app/heroes/heroes.component.ts`

```typescript
this.heroes = this.heroService.getHeroes();
```

**しかし、これは本番環境のアプリケーションでは機能しない。**

今のアプリケーションはモックヒーローを返しているのでこれを免れていますが、リモートサーバからヒーローデータを取得するにあたってこの処理は非同期ということに気づく。

**`HeroService`はサーバのレスポンスを持つ必要があり、`getHeroes()`は即座にヒーローデータを返せない。**そしてそのサービスが待機している間は、ブラウザはブロックされないだろう。

**`HeroService.getHeroes()`は何らかの非同期処理を実装する必要がある。**


### Observable `HeroService`

`Observable`は[RxJSライブラリ](https://rxjs.dev/)で重要なクラスの一つ。RxJSの`of()`を使ってサーバからのデータ取得を行う。

`HeroService`を開いて、`Observable`及び`of`を`RxJS`からインポートする。

`src/app/hero.service.ts`

```typescript
import { Observable, of } from 'rxjs';
```

`getHeroes()`を以下のように書き直す。

`src/app/hero.service.ts`

```typescript
getHeroes(): Observable<Hero[]> {
  const heroes = of(HEROES);
  return heroes;
}
```

`of(HEROES)`は一つの値、すなわちモックヒーローの配列を出力する`Observable<Hero[]>`を返す。


### `HeroesComponent`でのSubscribe

`HeroService.getHeroes`メソッドは`Hero[]`を返していたが、現在の返り値は`Observable<Hero[]>`である。そのため、これらの違いを修正する必要がある。

`getHeroes`を開いて、以下のコードに変更する。

`heroes.component.ts`(Observable)

```typescript
getHeroes(): void {
  this.heroService.getHeroes()
    .subscribe(heroes => this.heroes = heroes)
}
```

`heroes.component.ts`(Original)

```typescript
getHeroes(): void {
  this.heroes = this.heroService.getHeroes();
}
```

**`Observable.subscribe()`は重要な違い。**

変更後のバージョンでは、`Observable`がヒーローの配列を出力するのを待つ。これは現在あるいは数分後に起こる可能性が高い。

そのとき、`subscribe()`メソッドは出力された配列をコールバックに渡し、コンポーネントの`heroes`プロパティを設定する。

この非同期的手法は、`HeroService`がサーバからヒーローを取得する際に正常に動作。

## メッセージの表示

このセクションでは、以下の方法について詳細に説明する。

* メッセージを表示するための`MessageComponent`を画面下部に表示
* 表示するメッセージを送信するために、アプリケーション全体で注入可能な`MessageService`を作成
* `HeroService`に`MessageService`を注入
* `HeroService`のデータ取得成功時にメッセージを表示

### `MessagesComponent`の作成

Angular CLIで`MessagesComponent`の作成。

```
$ ng generate component messages
```

Angular CLIは`src/app/messages`配下にコンポーネントファイル群を生成し、`AppModule`内に`MessagesComponent`を宣言する。

作成した`MessagesComponent`を表示するために、`AppComponent`のテンプレートを修正する。

`src/app/app.component.html`

```html
<h1>{{title}}</h1>
<app-heroes></app-heroes>
<app-messages></app-messages>
```

### `MessageService`の作成

Angular CLIを作成し、`src/app`配下に`MessageService`を作成する。

```
$ ng generate service message
```

`MessageService`を開いて、以下のコードへ修正する。

`src/app/message.service.ts`

```typescript
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root',
})
export class MessageService {
  messages: string[] = [];

  add(message: string) {
    this.messages.push(message);
  }

  clear() {
    this.messages = [];
  }
}
```

### `HeroService`への注入

`HeroService`で`MessageService`をインポート。

`src/app/hero.service.ts`

```typescript
import { MessageService } from './message.service';
```

プライベートな`messageService`プロパティを宣言するパラメータを使ってコンストラクタを変更する。Angularは`HeroService`を生成する際に、そのプロパティへシングルトンな`MessageService`を注入する。

`src/app/hero.service.ts`

```typescript
constructor(private messageService: MessageService) { }
```

### `HeroService`からメッセージを送る

ヒーローが取得された時にメッセージを送信するように`getHeroes()`メソッドを変更。

`src/app/hero.service.ts`

```typescript
getHeroes(): Observable<Hero[]> {
  const heroes = of(HEROES);
  this.messageService.add('HeroService: fetched heroes');
  return heroes;
}
```

### `HeroService`からのメッセージを表示する

`MessagesComponent`は`HeroService`がヒーローを取得した際に送信するメッセージを含めて、全てのメッセージを表示しなければならない。

`MessagesComponent`を開いて、`MessageService`をインポート。

`src/app/messages/messages.component.ts`

```typescript
import { MessageService } from '../message.service';
```

### `MessageService`へのバインド

Angular CLIで生成された`MessagesComponent`のテンプレートを下記コードへ置き換えましょう。

`src/app/messages/messages.component.html`

```html
<div *ngIf="messageService.messages.length">

  <h2>Messages</h2>
  <button class="clear"
          (click)="messageService.clear()">Clear messages</button>
  <div *ngFor='let message of messageService.messages'> {{message}} </div>

</div>
```

`messages.component.css`をコンポーネントのスタイルに追加すると、このメッセージのUIの外観はより良いものになるだろう。

## `HeroService`にメッセージの追加

ユーザがヒーローをクリックする際に、メッセージを送信、表示してユーザの選択履歴を表示する方法を示す。

`src/app/heroes/heroes.component.ts`

```typescript
import { Component, OnInit } from '@angular/core';

import { Hero } from '../hero';
import { HeroService } from '../hero.service';
import { MessageService } from '../message.service';

@Component({
  selector: 'app-heroes',
  templateUrl: './heroes.component.html',
  styleUrls: ['./heroes.component.css']
})
export class HeroesComponent implements OnInit {

  selectedHero?: Hero;

  heroes: Hero[] = [];

  constructor(private heroService: HeroService, private messageService: MessageService) { }

  ngOnInit(): void {
    this.getHeroes();
  }

  onSelect(hero: Hero): void {
    this.selectedHero = hero;
    this.messageService.add(`HeroesComponent: Selected hero id=${hero.id}`);
  }

  getHeroes(): void {
    this.heroService.getHeroes()
        .subscribe(heroes => this.heroes = heroes);
  }
}
```

ヒーローリストを見るためにブラウザを更新し、一番下までスクロールすると`HeroService`からのメッセージが表示される。

ヒーローをクリックするたびに、新しいメッセージが選択を登録して表示されるようになる。

## ルーティングを使ったナビゲーションの追加

以下の機能を実装する。

* ダッシュボードビューを追加
* ヒーローズビューとダッシュボードビューのあいだで行き来できる機能を追加
* ユーザが各ビューでヒーロー名をクリックした際に、選択されたヒーローの詳細ビューを表示
* ユーザがemail上でリンクをクリックした時、特定のヒーローの詳細ビューを表示

## `AppRoutingModule`の追加

Angularのベストプラクティスは、ルートの`AppModule`からインポートされるルーティング専用のトップレベルモジュールで、ルーターをロードして管理すること。

モジュールのクラス名は`AppRoutingModule`とし、`src/app`フォルダの`app-routing.module.ts`に書く。

CLIで生成できる。

```
$ ng generate module app-routing --module=app
```

生成されたファイルは以下のようになる

`src/app/app-routing.module.ts`

```typescript
import { NgModule } from '@angular/core';
import { CommonModule } from '@angular/common';


@NgModule({
  declarations: [],
  imports: [
    CommonModule
  ]
})
export class AppRoutingModule { }

```

こちらのプログラムを以下のように書き換える。

```typescript
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { HeroesComponent } from './heroes/heroes.component';

const routes: Routes = [
  { path: 'heroes', component: HeroesComponent }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

最初にアプリケーションにルーティング機能をもたせられる`RouterModule`と`Routes`をインポートする。次のインポートである`HeroesComponent`は、ルートを設定することでルーターに向かう場所を教える。

`CommonModule`の参照と`declarations`配列は不要。


### ルート

ファイルの次の部分は、ルートを構成する場所である。ルートは、ユーザがリンクをクリックした際に、またはURLをブラウザのアドレスバーに貼り付けた際に表示するビューをルーターに伝える。

`app-routing.module.ts`はすでに`HeroesComponent`をインポートしているので、`routes`配列で使える。

`src/app/app-routing.module.ts`

```typescript
const routes: Routes = [
  { path: 'heroes', component: HeroesComponent }
];
```


典型的なAngularの`Route`は2つの要素を持つ。

* `path`：ブラウザのアドレスバーにあるURLにマッチする配列
* `component`：そのルートに遷移する際にルーターが作成すべきコンポーネント

これによって、ルーターはそのURLを`path: 'heroes'`に一致させて、URLが`localhost:4200/heroes`のようなものに限って`HeroesComponent`を表示できる。

### [`RouterModule.forRoot()`](https://angular.jp/api/router/RouterModule#forRoot)

[`@NgModule`](https://angular.jp/api/core/NgModule)メタデータはルーターを初期化してブラウザのロケーションの変更を待機する。

以下の行は、[`RouterModule`](https://angular.jp/api/router/RouterModule)を`AppRoutingModule`の`imports`配列に追加し、`RouterModule.forRoot()`を呼び出してワンステップで`routes`に追加。

`src/app/app-routing.module.ts`

```typescript
imports: [ RouterModule.forRoot(routes) ],
```

<blockquote>

【解説】

<br>
<br>

アプリケーションのルートのレベルでルーターを設定しているので、このメソッドは`forRoot()`と呼ばれる。<b>このメソッドは、ルーティングに必要なサービスやプロバイダーとディレクティブを提供し、ブラウザの現在のURLをベースに最初の遷移を行う。</b>

</blockquote>

次に、`AppRoutingModule`は`RouterModule`をエクスポートして、アプリケーション全体で利用できるようにする。

`src/app/app-routing.module.ts`

```typescript
exports: [ RouterModule ]
```

## `RouterOutlet`の追加

`AppComponent`テンプレートを開いて、`<app-heroes>`要素を`<router-oulet>`に置換

`src/app/app.component.html`

```html
<h1>{{title}}</h1>
<router-outlet></router-outlet>
<app-messages></app-messages>
```

### 試す

こちらのCLIコマンドを入力

```
$ ng serve
```

ブラウザを更新するとアプリケーションのタイトルは表示されるが、ヒーローのリストは表示されない。

ブラウザのアドレスバーを見ると、URLが`/`で終了。`HeroesComponent`へのルーターのパスは`/heroes`。これを入力すればおなじみのヒーローのマスター／詳細ビューが表示されるはず。(実際には表示されていない)

## ナビゲーションのリンクを追加([`routerLink`](https://angular.jp/api/router/RouterLink))

理想的には、ルートのURLをアドレスバーに貼り付けるのではなく、ユーザがリンクをクリックして遷移できるようにする必要がある。

`<nav>`要素を追加して、その中にクリックされると`HeroesComponent`へ遷移するトリガーになるアンカー要素を追加。修正された`AppComponent`テンプレートは以下のようになる。

`src/app/app.component.html`

```typescript
<h1>{{title}}</h1>
<nav>
  <a routerLink="/heroes">Heroes</a>
</nav>
<router-outlet></router-outlet>
<app-messages></app-messages>
```

この際、`routerLink`属性は、ルーターが`HeroesComponent`へのルートとして一致する文字列である`"/heroes"`に設定される。この`routerLink`は、ユーザのクリックをルーターのナビゲーションへ変換する`RouterLink`ディレクティブのためのセレクターである。

このとき、ブラウザを更新するとアプリケーションのタイトルとヒーローのリンクは表示されるが、ヒーローのリストは表示されない。

## ダッシュボードビューを追加

