# Angular

Angularでヒーローを表示するアプリ(CRUDモデルの練習)を開発。(チュートリアル)

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

ルーティングは、複数のビューがある場合に更に意味を持つ。CLIを使って`DashBoardComponent`を追加する。

```
$ ng generate component dashboard
```

CLIは、`DashBoardComponent`のためのファイルを生成し、`AppModule`の中でそれを宣言する。これら3つのファイルのデフォルト内容を以下のように書き換える。

`src/app/dashboard/dashboard.component.html`

```html
<h2>Top Heroes</h2>
<div class="heroes-menu">
  <a *ngFor="let hero of heroes">
    {{hero.name}}
  </a>
</div>
```

`src/app/dashboard/dashboard.component.ts`

```typescript
import { Component, OnInit } from '@angular/core';
import { Hero } from '../hero';
import { HeroService } from '../hero.service';

@Component({
  selector: 'app-dashboard',
  templateUrl: './dashboard.component.html',
  styleUrls: [ './dashboard.component.css' ]
})
export class DashboardComponent implements OnInit {
  heroes: Hero[] = [];

  constructor(private heroService: HeroService) { }

  ngOnInit(): void {
    this.getHeroes();
  }

  getHeroes(): void {
    this.heroService.getHeroes()
      .subscribe(heroes => this.heroes = heroes.slice(1, 5));
  }
}
```

`src/app/dashboard/dashboard.component.css`

```css
/* DashboardComponent's private CSS styles */

h2 {
  text-align: center;
}

.heroes-menu {
  padding: 0;
  margin: auto;
  max-width: 1000px;

  /* flexbox */
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: space-around;
  align-content: flex-start;
  align-items: flex-start;
}

a {
  background-color: #3f525c;
  border-radius: 2px;
  padding: 1rem;
  font-size: 1.2rem;
  text-decoration: none;
  display: inline-block;
  color: #fff;
  text-align: center;
  width: 100%;
  min-width: 70px;
  margin: .5rem auto;
  box-sizing: border-box;

  /* flexbox */
  order: 0;
  flex: 0 1 auto;
  align-self: auto;
}

@media (min-width: 600px) {
  a {
    width: 18%;
    box-sizing: content-box;
  }
}

a:hover {
  background-color: #000;
}
```

### ダッシュボードのルートを追加

ダッシュボードに遷移するには、ルーターに適切なルートが必要である。`app-routiung.module.ts`で`DashboardComponent`をインポート。

`src/app/app-routing.module.ts`

```typescript
import { DashboardComponent } from './dashboard/dashboard.component';
```

`routes`配列に、`DashboardComponent`へのパスにマッチするルートを追加。

`src/app/app-routing.module.ts`

```typescript
{ path: 'dashboard', component: DashboardComponent },
```

### デフォルトルートの追加

アプリケーションを起動すると、ブラウザのアドレスバーはWebサイトのルートを指す。これは既存のルートと一致しないので、ルーターはどこにも移動しない。`<router-outlet>`の下のスペースが空白になっているからだ。

アプリケーションをダッシュボードに自動的に遷移するには、以下のルートを`routes`配列に追加。

`src/app/app-routing.module.ts`

```typescript
{ path: '', redirectTo: '/dashboard', pathMatch: 'full' },
```

このルートは、空のパスと完全に一致するURLを、パスが`/dashboard`であるルートにリダイレクトする。

ブラウザが更新されると、ルーターは`DashboardComponent`をロードし、ブラウザのアドレスバーには`/dashboard`のURLが表示される。

### ダッシュボードのリンクをシェルに追加

ユーザはページのトップにあるナビゲーション領域のリンクをクリックすることで、`DashboardComponent`と`HeroesComponent`のあいだを行き来することができます。

Heroesリンクの上、`AppComponent`シェルテンプレートにダッシュボードのナビゲーションリンクを追加する。

`src/app/app.component.html`

```html
<h1>{{title}}</h1>
<nav>
  <a routerLink="/dashboard">Dashboard</a>
  <a routerLink="/heroes">Heroes</a>
</nav>
<router-outlet></router-outlet>
<app-messages></app-messages>
```

ブラウザが更新されると、リンクをクリックすることで二つのビューの間を自由に遷移できるようになる。

## ヒーローの詳細ページを表示する

`HeroDetailComponent`は選択されたヒーローの詳細を表示する。

1. ダッシュボードのヒーローをクリックする
2. ヒーローリストのヒーローをクリックする
3. 表示するヒーローを識別するブラウザのアドレスバーにディープリンクURLを貼り付ける

### `HeroesComponent`からヒーローの詳細を削除する

ユーザが`HeroesComponent`で一つのヒーローをクリックすると、アプリは`HeroDetailComponent`に遷移する必要があり、ヒーローリストビューをヒーロー詳細ビューに置換する。

`HeroesComponent`テンプレート(`heroes/heroes.component.html`)を開いて、`<app-hero-detail>`要素を一番下から削除する

### ヒーローの詳細を表示するルートを設定

`app-routing.module.ts`を開いて、`HeroDetailComponent`をインポートする。

`src/app/app-routing.module.ts`

```typescript
import { HeroDetailComponent } from './hero-detail/hero-detail.component';
```

次に、ヒーロー詳細ビューへのパスのパターンと一致するパラメータ付きルートを`routes`配列に追加する。

`src/app/app-routing.module.ts`

```typescript
{ path: 'detail/:id', component: HeroDetailComponent },
```

▲上記のプログラムと同じファイル

```typescript
const routes: Routes = [
  { path: '', redirectTo: '/dashboard', pathMatch: 'full' },
  { path: 'dashboard', component: DashboardComponent },
  { path: 'detail/:id', component: HeroDetailComponent },
  { path: 'heroes', component: HeroesComponent }
];
```

### `DashboardComponent`ヒーローへのリンク

現時点では`DashboardComponent`ヒーローへのリンクは何もしない。

ルーターは`HeroDetailComponent`へのルートを持っているので、ダッシュボードのリンクを修正してパラメータ付きダッシュボードのルート経由で遷移する。

`src/app/dashboard/dashboard.component.html`

```html
<a *ngFor="let hero of heroes"
  routerLink="/detail/{{hero.id}}">
  {{hero.name}}
</a>
```

`*ngFor`リピーター内でAngularの[補間バインディング](https://angular.jp/guide/interpolation)を活用し、現在の繰り返しの`hero.id`を個々の[`routerLink`](https://angular.jp/tutorial/toh-pt5#routerlink)に挿入する。


### `HeroesComponent`ヒーローへのリンク

`HeroesComponent`のヒーローのアイテムは、コンポーネントの`onSelect()`メソッドにバインディングされたクリックイベントを持つ`<li>`要素。

`src/app/heroes/heroes.component.html`

```html
<ul class="heroes">
  <li *ngFor="let hero of heroes"
    [class.selected]="hero === selectedHero"
    (click)="onSelect(hero)">
    <span class="badge">{{hero.id}}</span> {{hero.name}}
  </li>
</ul>
```

こちらの`<li>`要素を`*ngFor`だけを持つように戻して、アンカー要素(`<a>`)でバッジと名前を囲み、ダッシュボードのテンプレートと同じようにアンカーに`routerLink`要素を追加。

`src/app/heroes/heroes.component.html`

```html
<ul class="heroes">
  <li *ngFor="let hero of heroes">
    <a routerLink="/detail/{{hero.id}}">
      <span class="badge">{{hero.id}}</span> {{hero.name}}
    </a>
  </li>
</ul>
```

プライベートなスタイルシート(`heroes.component.css`)を修正して、これまでと同じようにリストが見えるようにする。

### 不要なコードを削除

`HeroesComponent`クラスはまだ動作するが、`onSelect()`メソッドと`selectedHero`プロパティはもはや使われない。

不要なコードを削除する。(**必要最低限の機能で実装するため**)

`src/app/heroes/heroes.component.ts`

```typescript
export class HeroesComponent implements OnInit {
  heroes: Hero[] = [];

  constructor(private heroService: HeroService) { }

  ngOnInit(): void {
    this.getHeroes();
  }

  getHeroes(): void {
    this.heroService.getHeroes()
    .subscribe(heroes => this.heroes = heroes);
  }
}
```

## ルーティング可能な`HeroDetailComponent`

`HeroesComponent`は表示するヒーローを取得するための新しい方法が必要。

* それを作成したルートを取得
* ルートから`id`を抽出
* `HeroService`を経由してサーバからその`id`でヒーローを取得する

`src/app/hero-detail/hero-detail.component.ts`

```typescript
import { ActivatedRoute } from '@angular/router';
import { Location } from '@angular/common';

import { HeroService } from '../hero.service';
```

[`ActivatedRoute`](https://angular.jp/api/router/ActivatedRoute)、`HeroService`、[`Location`](https://angular.jp/api/common/Location)サービスをコンストラクタに入れて、それらの値をプライベートフィールドに保存。

`src/app/hero-detail/hero-detail.component.ts`

```typescript
import { Component, OnInit, Input } from '@angular/core';
import { Hero } from '../hero';

import { ActivatedRoute } from '@angular/router';
import { Location } from '@angular/common';

import { HeroService } from '../hero.service';

@Component({
  selector: 'app-hero-detail',
  templateUrl: './hero-detail.component.html',
  styleUrls: ['./hero-detail.component.css']
})
export class HeroDetailComponent implements OnInit {
  @Input() hero?: Hero;

  // 追加
  constructor(
    private route: ActivatedRoute, // HeroDetailComponentのインスタンスのルートに関する情報を保持
    private heroService: HeroService, // リモートサーバからヒーローのデータを取得し、このコンポーネントはそれを使用して表示するヒーローを取得
    private location: Location // ブラウザと対話するためのAngularサービス。
  ) { }

  ngOnInit(): void {
  }

}

```

### `id`ルートパラメータを抽出

`ngOnInit()`ライフサイクルハックで、`getHero()`を呼び出して以下のように定義する。

`src/app/hero-detail/hero-detail.component.ts`

```typescript
ngOnInit(): void {
  this.getHero();
}

getHero(): void {
  const id = Number(this.route.snapshot.paramMap.get('id'));
  this.heroService.getHero(id)
    .subscribe(hero => this.hero = hero);
}
```

`route.snapshot`は、コンポーネントが作成された直後のルート情報の静的イメージ。`paramMap`は、URLから抽出されたルートパラメータ値の辞書。`id`キーは、fetchするヒーローの`id`を返す。

ルートパラメータは常に文字列。JavaScriptの`Number`関数は文字列を数値に変換。

しかし、上記のプログラムではプライベート変数`heroService`に`getHero()`メソッドが定義されていないので、コンパイルエラーが表示される。

### `HeroService.getHero()`の追加

`HeroService`を開いて、`getHeroes()`メソッドの後に`id`とともに次の`getHero()`メソッドを追加する。

`src/app/hero.service.ts`

```typescript
getHero(id: number): Observable<Hero> {
  // 現時点では、このプログラムではHeroとidの型が一致しない型エラーが発生する。対処法は後述
  const hero = HEROES.find(h => h.id === id)!;
  this.messageService.add(`HeroService: fetched hero id=${id}`);
  return of(hero);
}
```

**このとき、`id`を埋め込むためのJavaScriptの[テンプレートリテラル](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/template_strings)を定義するバッククォートに注意すること。**

上記のプログラムでは、`getHero()`を呼び出す`HeroDetailComponent`を変更することなく、実際の`Http`リクエストとして`getHero()`を再実装できる。

### 戻る方法を探す

ブラウザの戻るボタンをクリックすると、詳細ビューに来た時の経路によって、ヒーローリストまたはダッシュボード画面に戻れる。

`HeroDetail`ビュー上にそのような挙動を実装できるボタンを表示。コンポーネントのテンプレートの最後に戻るボタンを追加して、コンポーネントの`goBack()`メソッドにバインド。

`src/app/hero-detail/hero-detail.component.html`

```html
<button (click)="goBack()">go back</button>
```

前述の通り導入した[`Location`](https://angular.jp/api/common/Location)サービスで、ブラウザの履歴の一つ前にナビゲートする`goBack()`メソッドをコンポーネントのクラスに追加。

`src/app/hero-detail/hero-detail.component.ts`

```typescript
goBack(): void {
  this.location.back();
}
```

最後に、独自のCSSスタイルを`hero-detail.component.css`に追加すると、詳細がより美しく表示される

`src/app/hero-detail/hero-detail.component.css`

```css
/* HeroDetailComponent's private CSS styles */
label {
  color: #435960;
  font-weight: bold;
}
input {
  font-size: 1em;
  padding: .5rem;
}
button {
  margin-top: 20px;
  background-color: #eee;
  padding: 1rem;
  border-radius: 4px;
  font-size: 1rem;
}
button:hover {
  background-color: #cfd8dc;
}
button:disabled {
  background-color: #eee;
  color: #ccc;
  cursor: auto;
}
```

## サーバからデータの取得

Angularの`HttpClient`を使ってCRUDモデルを構築する

* `HeroService`はHTTPリクエストを介してヒーローデータを取得
* ユーザはヒーロー情報を追加、編集、削除でき、その変更をHTTP経由で伝達できる。
* ユーザは名前でヒーロー情報を検索できる

## HTTPサービスの有効化

`HttpClient`はHTTPを通してリモートサーバと通信するための仕組み。

`HttpClient`をインポートしてルートの`AppModule`に追加。

`src/app/app.module.ts`

```ts
import { HttpClientModule } from '@angular/common/http';
```

また、`AppModule`で`HttpClientModule`を`imports`配列に追加する。

`src/app/app.module.ts`

```ts
import { HttpClientModule } from '@angular/common/http';
...
@NgModule({
  imports: [
    HttpClientModule,
  ],
})
```

## データサーバをシミュレートする

[In-memory Web API](https://github.com/angular/angular/tree/master/packages/misc/angular-in-memory-web-api)でリモートサーバとの通信を再現する。

このモジュールをインストールすると、アプリケーションはインメモリWeb APIがリクエストをインターセプトして、そのリクエストをインメモリデータストアに適用してシミュレートされたレスポンスを返えさずに`HttpClient`でリクエストを送信、レスポンスを受信できます。

以下のコマンドを使って、npmからAPIをインストール。

```
npm install angular-in-memory-web-api --save
```

`AppModule`で、`HttpClientInMemoryWebApiModule`と、これからすぐに作成する`InMemoryDataService`クラスをインポート。

`src/app/app.module.ts`

```ts
import { HttpClientInMemoryWebApiModule } from 'angular-in-memory-web-api';
import { InMemoryDataService } from './in-memory-data.service';
```

`HttpClientModule`の後に、`HttpClientInMemoryWebApiModule`を`AppModule`の`imports`配列に追加し、`InMemoryDataService`で設定。

```ts
HttpClientModule,


HttpClientInMemoryWebApiModule.forRoot(
  InMemoryDataService, { dataEncapsulation: false }
)
```

`forRoot()`設定メソッドは、インメモリデータベースを準備する`InMemoryDataService`クラスを取る。

以下のコマンドで`src/app/in-memory-data.service.ts`クラスを生成

```
ng generate service InMemoryData
```

`in-memory-data.service.ts`のデフォルトの内容を以下のものに置き換える。

`src/app/in-memory-data.service.ts`

```ts
import { Injectable } from '@angular/core';
import { InMemoryDbService } from 'angular-in-memory-web-api';
import { Hero } from './hero';

@Injectable({
  providedIn: 'root',
})
export class InMemoryDataService implements InMemoryDbService {
  createDb() {
    const heroes = [
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
    return {heroes};
  }

  // Overrides the genId method to ensure that a hero always has an id.
  // If the heroes array is empty,
  // the method below returns the initial number (11).
  // if the heroes array is not empty, the method below returns the highest
  // hero id + 1.
  genId(heroes: Hero[]): number {
    return heroes.length > 0 ? Math.max(...heroes.map(hero => hero.id)) + 1 : 11;
  }
}
```

この際、`in-memory-data.service.ts`ファイルは`mock-heroes.ts`の機能を継承。しかし、まだ`mock-heroes.ts`は削除しない。

サーバが用意できたら、アプリケーションのリクエストはサーバに送信される。