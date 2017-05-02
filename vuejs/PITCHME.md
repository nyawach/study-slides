<!-- $theme: gaia -->
<!-- $size: 16:9 -->

# Vue.js

#### :princess:

---

- Vue.jsとは？
  - 歴史
  - 何が良いか
  - 記事: 私たちはなぜReactではなくVue.jsを選んだのか
- デモ
- Component
- 書き方
- 便利系ツール
- 周辺ライブラリ

---

## Vue.jsとは？

---

### [Vue.js](https://jp.vuejs.org/)

MVVM(Model View ViewModel)に影響を受けたプログレッシブフレームワーク

データの双方向バインディング
→リアクティブ

[Webアプリケーション開発者から見た、MVCとMVP、そしてMVVMの違い](http://qiita.com/shinkuFencer/items/f2651073fb71416b6cd7)

---

```
<div id="app">
  {{ message }}
</div>
```

```
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})
```

![Hello!](images/hello.png)

---

## 歴史

- 2013年後半開発開始
- 2015年10月末v1.0ローンチ(コードネーム: Evangelion)
- 2016年10月 v2.0ローンチ(コードネーム: Ghost in the Shell)

**1.xと2.xで変更点がある(特にfilterまわり)** ので、どちらのバージョンかは予め確認を！

(今回のスライドは2.xベース)

移行についての公式ガイド: [Vue 1.x からの移行](https://jp.vuejs.org/v2/guide/migration.html)

---

## 何が良いか

- 各種プリプロセッサ(Sass/Babel/Pugなど)を比較的容易に使える
- 明示的変数宣言
- **JSXで書かなくて良い**
- TypeScriptの型定義ファイル
- **公式の日本語ドキュメント**: https://jp.vuejs.org/v2/guide/
- 動作がわりと速い

---

### 各種プリプロセッサ(Sass/Babel/Pugなど)を比較的容易に使える

Webpack(or Browserify)を通していろんなトランスパイラが使える

- Pug
- Sass
- Stylus
- PostCSS
- Babel

などなど

---

### 明示的変数宣言

各コンポーネントに属するプロパティは明示的に宣言しないと使えない

```js

data() {
  return {
    // fetch前のデータとして必要
    jsonData: {},
  };
},
computed {
  fetchData() {
    fetch("http://example.com")
      .then(data=>data.json())
      .then(json=>{ this.jsonData=json });
  },
}
```

---

### JSXで書かなくて良い

React(JSX): 条件分岐などが入ると初学者にはDOM構造が読みづらい

Vue: ディレクティブでの条件分岐=>DOM構造が読みやすい(間によくわからない`{{}}`が出現しない)
※Pugの構文などは別の話

---

### [動作がわりと速い](http://stefankrause.net/js-frameworks-benchmark4/webdriver-ts/table.html)

![Vue](https://cdn-images-1.medium.com/max/1600/1*Lu6OJiraJYShl4aBppoh3w.png)

---

### 私たちはなぜReactではなくVue.jsを選んだのか

[記事](http://postd.cc/why-we-chose-vuejs-over-react/)

- チーム規模に合ってない保守性の高いコード
- JSX読みづらい
- React+Reduxは一日中タイピングしなければならない
- 必要な道具の多さ
- Angular2は完璧さを求めてるようにみえる
- 顧客向けのフロントエンドインターフェイスのUIとかもかいている

---

### VueJSのデメリット

- エラーがわかりにくい
- 未成熟コミュニティ
- 中国語
- 個人プロジェクト(スポンサーがいる)

---

## デモ

知見山(仮)のリポジトリ見ながら環境の解説+文法の解説する予定

---

### 知見山(仮)の環境

- gulp+webpack
- .vueファイルをpug/ES2015/PostCSSで記述
- browser-sync
- gulp-watch

---

### Component

---

#### .vueファイル

`script.js`

```js
import Vue from "vue";
import App from "./lib/App";

new Vue({
  el: "#app",
  components: { app: App },
});
```
---

`App.vue`

```jsx:app.vue
<template lang="pug">
ul
  todo(v-for="todo, i in todoList" v-bind:no="todo.no" v-bind:title="todo.title" v-if="i >= 3")
</template>
<style lang="postcss">
</style>
```

---

`App.vue`

```
<script lang="babel">
import Todo from "./Todo";
export default {
  data: () => {
    return {
      todoList: [ { no: "1", title: "title1" }, ],
    };
  },
  methods: {
    awesomeMethod() {},
  },
  components: {
    Todo,
  },
}
</script>
```

---

`Todo.vue`

```jsx:Todo.vue
<template lang="pug">
.todo {{ no }} : {{ title }}
</template>

<script lang="babel">
export default {
  props: ["no", "title"]
}
</script>
<style lang="postcss" scoped>
</style>
```

---

## 書き方

---

### scoped

自動でUID付きのdata属性をつけてくれる
scopedに対応してないものは[data-v-xxxxxxxx]内にCSSタグが付与される仕組み？

```
<ul data-v-425e9a8f="" class="ticket__list">
</ul>
```

---

ex. 親要素からデータを受け取る時

```js
props: ["inheritedData"],
data() {
  return {
    // ...
  };
}
computed: {
  // ...
  showData() {
    console.log(this.inheritedData);
  },
}

```

---

### ディレクティブ

タグに記述する
AngularJS使ったことある方は馴染みあるかも？(`ng-xxx`) => `v-xxx`

- [ディレクティブ](https://jp.vuejs.org/v2/api/#ディレクティブ)
- Qiita: [体で覚えるVue.js - ディレクティブ編 〜 JSおくのほそ道 #023](http://qiita.com/hosomichi/items/25041c1d46452de84aa6)

---

#### v-on

イベントハンドリング

`evt.preventDefault` や `evt.stopPropagation` など用の修飾子がある

[イベントハンドリング](https://jp.vuejs.org/v2/guide/events.html)

```html
<button v-on:click="alertData"></button>
<button @click="alertData"></button>
<button @click.prevent="alertData"></button>
```
---

#### v-bind

属性 or コンポーネントで定義したプロパティと式を動的に束縛。

```html
<user v-bind:data="userData[id]"></user>
<user :data="userData[id]"></user><!-- 省略記法 -->
```

---

#### v-repeat

繰り返し

```
<ul>
  <li v-repeat="user in users">
    {{user.name}} {{user.email}}
  </li>
</ul>
```

---

#### v-model

双方向バインディング
`input` `select` `textarea` のみ

```html
<input type="text" v-model="inputText" />
```

`v-model`と`v-bind`

---

### トランジション効果
[トランジション効果](https://jp.vuejs.org/v2/guide/transitions.html): CSS/JSアニメーションを記述可能に

![トランジションの状態遷移図](https://jp.vuejs.org/images/transition.png)

---

`script.js`
```
new Vue({
  el: '#example-1',
  data: {
    show: true
  }
})
```
`index.html`
```
<div id="example-1">
  <button @click="show = !show">Toggle render</button>
  <!-- [name]に付けた値がclass名のsuffixになる -->
  <transition name="slide-fade">
    <p v-if="show">hello</p>
  </transition>
</div>
```

---

`style.css`
```
/* [name]-[enter/leave]-[active/to] */
.slide-fade-enter-active {
  transition: all .3s ease;
}
.slide-fade-leave-active {
  transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
}
.slide-fade-enter, .slide-fade-leave-to
/* .slide-fade-leave-active for <2.1.8 */ {
  transform: translateX(10px);
  opacity: 0;
}
```
---

## 使った所感

Riotの書き方で、Reactのライフサイクル・機能があって、Angularライクなディレクティブが使える感じ(ざっくり)

◯ HTML(or Template)/CSS/JSを分割できる
◯ 各言語もwebpack+loaderで比較的簡単にメタ言語が使える

```
<template src="view.pug" lang="pug"></template>
<script src="view.js" lang="babel"></script>
<style src="view.css" lang="postcss"></style>
```

△ ディレクティブ覚えるのが若干面倒

× Arrow FunctionやBabelでの書き方がきもい

---

## 便利系

---
![デモ](https://raw.githubusercontent.com/vuejs/vue-devtools/master/media/demo.gif)

---

### vue-cli

Vueの開発環境をいい感じに作ってくれる
公式提供: [vue-cli を発表](https://jp.vuejs.org/2015/12/28/vue-cli/)

```
npm install -g vue-cli
vue init webpack my-project
# プロンプトへ回答
cd my-project
npm install
npm run dev # ドジャーン!
```

**ドジャーン！**

---


## 周辺ライブラリ

---

### Vuex

[vuejs/vuex: Centralized State Management for Vue.js.](https://github.com/vuejs/vuex)

Vue用に最適化されたFluxライクライブラリ

開発元が同じ(Reduxはサードパーティ)

[状態管理](https://jp.vuejs.org/v2/guide/state-management.html)

---

#### Vuexストアフロー

![Vuexストアフロー](https://raw.githubusercontent.com/vuejs/vuex/dev/docs/en/images/vuex.png)

---

### Vue-Router

[vuejs/vue-router: The official router for Vue.js.](https://github.com/vuejs/vue-router)

ComponentのRouting設定ができる

こちらも開発元が同じ

[ルーティング](https://jp.vuejs.org/v2/guide/routing.html)

---

### NUXT.js

ユニバーサル Vue アプリケーションを作成するための非常に合理的な開発エクスペリエンスを提供(強そう)

- Vue 2
- Vue-Router
- Vuex (included only when using the store option)
- Vue-Meta

[Nuxt.js - Universal Vue.js Applications](https://nuxtjs.org/)

[Vue.js向けSSRフレームワークnuxt.jsを触ってみた](http://qiita.com/YukiYonekura/items/17820d46a0b68a0c2fe8)

---

### weex

ネイティブアプリ作れるやーつ
APIドキュメントなどもまだ書かれていないので触るのはまだ早そう
[weex](https://weex-project.io/)

![flow](https://weex-project.io/images/flow.png)

---

# :princess:

---

## References:

日本人コアエンジニアのスライド2つ
[Vue.js Recent Trends by kazupon](https://speakerdeck.com/kazupon/vue-dot-js-recent-trends)
[Vue.js 2.0 Server Side Rendering by kazupon](https://speakerdeck.com/kazupon/vue-dot-js-2-dot-0-server-side-rendering)

メリットとか選定基準とか
[💓 Vue.js](https://nakajmg.github.io/s/161119-vue/)


[ReactとAngularのいいとこ取り？ 2017年こそ学びたいVue.jsの始め方](https://www.webprofessional.jp/up-and-running-vue-js-2-0/)


[Why we chose Vue.js over React](http://pixeljets.com/blog/why-we-chose-vuejs-over-react/)

公式のわりと公正そうな比較
[Comparison with Other Frameworks — Vue.js](https://vuejs.org/v2/guide/comparison.html)