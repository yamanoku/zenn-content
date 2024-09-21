---
title: "Vue3.5の改善から振り返るdefineCustomElementについて"
emoji: "⚙️"
type: "tech"
topics: ["vue", "webcomponents"]
published: false
publication_name: "comm_vue_nuxt"
---

Vue3.5の更新において `defineCustomElement` に関する改善が含まれています。今回の記事ではその `defineCustomElement` に関連する事項についてを紹介・解説していきます。

## Custom Elementsとは？

`defineCustomElement`についてを紹介する前に、まずは根底の知識となる「Custom Elements」についてを解説します。

Custom Elementsは[Web Components](https://developer.mozilla.org/ja/docs/Web/API/Web_components)を構成する一部で、独自のHTML要素を作成するためのJavaScript APIです。

独自の定義のため、通常のHTML要素とは異なりダッシュが使われている名前(`kebab-case`)である必要があります。

```html
<!-- 純粋なHTML要素 -->
<progress></progress>

<!-- Custom Elements -->
<custom-progress></custom-progress>
```

Custom Elementsの利点としては、UIライブラリ・フレームワークを使用しているかに関わらずに利用できることが挙げられます。例えばフロントエンドの技術スタックが異なるアプリケーション間で共通のUIコンポーネントを使用する場合に有用です。

Vue.js内でCustom Elementsを使用するにあたり一部設定[^1]を追加する必要がありますが、[必要なテストケースはすべて通過しているため](https://custom-elements-everywhere.com/libraries/vue/results/results.html)、安心して使用できます。

[^1]: [コンポーネント解決のスキップ](https://ja.vuejs.org/guide/extras/web-components.html#skipping-component-resolution)の設定を参考ください

Web標準技術の相互運用向上プロジェクトである[Interop](https://wpt.fyi/interop-2024)では[Web Componentsの改善が行われており](https://wpt.fyi/results/?label=experimental&label=master&product=chrome&product=firefox&product=safari&aligned&view=interop&q=label%3Ainterop-2023-webcomponents)、Custom Elementsはほぼほぼテストケースを通過しているためクロスブラウザでもある程度安定して使用できると言えます。

Custom Elementsの具体的な使用例を上げると、GitHubのリポジトリ内での日付表示部分は[Web Componentsで作られており](https://github.com/github/relative-time-element)、開発ツールで該当部分を見ると独自のHTML要素で定義されていることがわかります。

![relative-timeというCustom Elementsによって「last week」と表示されている](https://i.gyazo.com/51a182f6b9bf3c266eaa65d41434b1b2.png)

![実際に開発者ツール上で表示されるrelative-timeとして使用されているコード](https://i.gyazo.com/474e1423dc198e6fc5805c9349688c20.png)

Zennでもウィジェットの表示においてCustom Elementsが使用されています。

https://zenn.dev/steelydylan/articles/zenn-web-components

## VueコンポーネントをCustom Elementsとして配布する

Vue3.2から[`defineCustomElement`](https://ja.vuejs.org/api/custom-elements.html#definecustomelement)というAPIが追加され、VueコンポーネントをCustom Elementsとして使用できるようになりました。

### `defineCustomElement`の使い方

使い方はシンプルで、コンポーネントの拡張子を `.vue` から `.ce.vue` に変更して `defineCustomElement` で呼び出すことで、Vue.jsのコンポーネントをCustom Elementsとして使用できます。

```ts
import { defineCustomElement } from 'vue';
import Example from './Example.ce.vue';

// Custom Elementsのコンストラクタに変換
const ExampleElement = defineCustomElement(Example);

// Custom Elementsとして登録
customElements.define('vue-app-element', ExampleElement);
```

```html
<!--　バンドルされたJSを読み込むと以下Custom　Elementsが使用できます -->
<vue-app-element></vue-app-element>
```

公式のSFCツール（vue-loader@^16.5.0、@vitejs/plugin-vue@^1.4.0）では、`"Custom Elements Mode"`をサポートしており、`ce.vue`拡張子でなくとも `customElement` オプションを追加することでCustom Elementsで呼び出せます。

```js:vite.config.js
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [vue({
    customElement: true, // Custom Elements Mode
  })],
})
```

### `defineCustomElement`のサンプルコードとデモ

`defineCustomElement`のデモとして以下リポジトリを用意しています。

https://github.com/yamanoku-playground/2024-09-21-vue3_5-defineCustomElement-demo

Viteを起動すると、描画されているVueコンポーネント部分がCustom Elementsとして表示されることが確認できます。内部のカウントアップボタンをクリックするとカウントが増えることも確認できます。

![ViteのVueアプリサンプルがvue-app-elementというCustom Elementsによって表示されている。カウントアップボタンが正常に動作してカウントが5まで増えている。](/images/improvements-to-custom-elements-in-vue3-5/9220af571d03548cfcdd007c06358245.gif)

### 実際の使用事例

Vue.jsやNuxtアプリケーション内でVueコンポーネントをわざわざCustom Elementsに変換して使用しないと思います。実際にどのように使用されているかについては、GMOペパボさんの以下記事の「Vue 3系におけるWeb Comopnent Build」やスライドにて紹介されている事例が参考になると思います。

https://tech.pepabo.com/2022/10/31/colorme-vue3-migration/

https://speakerdeck.com/piyoppi/definecustomelement-wohuo-yong-sita-sabisugong-tong-nouikonponentoraiburari?slide=5

## v3.5で入った改善についての紹介

そんな `defineCustomElement` ですがVue3.5のアップデートに伴い、様々な改善が入りました。以下はそれらに含まれた改善についてを紹介しています。

### custom-element: `useShadowRoot()` helper

Custom Elementsの `shadowRoot` を取得する `useShadowRoot()` ヘルパーが実装されました。

https://github.com/vuejs/core/commit/5a1a89bd6178cc2f84ba91da7d72aee4c6ec1282

### custom-element: `useHost()` helper

Custom Elementsの `host` を取得する `useHost()` ヘルパーが実装されました。

https://github.com/vuejs/core/commit/775103af37df69d34c79f12c4c1776c47d07f0a0

### custom-element: expose `this.$host` in Options API

Options APIでカスタム要素のホスト要素を公開できる `this.$host` 属性が実装されました。

https://github.com/vuejs/core/commit/1ef8f46af0cfdec2fed66376772409e0aa25ad50

### custom-element: inject child components styles to custom element shadow root

子コンポーネントのスタイルをCustom Elementsの `shadowRoot` に挿入する機能が実装されました。

```js
import { defineCustomElement } from 'vue';
import Root from './Root.ce.vue';

customElements.define('root-element', defineCustomElement(Root));
```

```vue
<!-- Root.ce.vue -->
<script setup>
import Child from './Child.ce.vue';
</script>
```

```vue
<!-- Child.ce.vue -->
<style>
div { color: red }; /* Root.ce.vue コンポーネントに挿入される style */
</style>
```

https://github.com/vuejs/core/pull/11517

https://github.com/vuejs/core/commit/56c76a8b05c45f782ed3a16ec77c6292b71a17f1

### custom-element: support configurable app instance in defineCustomElement

`defineCustomElement` でCustom Elementsを登録する際に `configureApp` でVueアプリのインスタンスを設定できるようになりました。これによりdevtoolsでも `defineCustomElement` で作成されたコンポーネントを認識できるようになりました。

```js
defineCustomElement({
  // ...
}, {
  configureApp(app) {
    // ...
  }
})
```

https://github.com/vuejs/core/commit/6758c3cd0427f97394d95168c655dae3b7fa62cd

### custom-element: support css `:host` selector by applying css vars on host element

`:host` セレクタをサポートするために、`host` 要素にCSS変数を適用する実装がされました。

https://github.com/vuejs/core/pull/8830


### custom-element: support emit with options

最初のイベント引数がオブジェクトである場合 [`CustomEvent`](https://developer.mozilla.org/ja/docs/Web/API/CustomEvent) のオプションオブジェクトとして使用されるようになりました。引数リスト全体は `CustomEvent` の `detail` プロパティを通して公開されます。

```js
emit('event', { bubbles: true });
```

https://github.com/vuejs/core/commit/e181bff6dc39d5cef92000c10291243c7d6e4d08

### custom-element: support expose on customElement

defineCustomElementで `expose` をサポートされるようになり、Custom Elementsから参照できるようになりました。

```ts
const HelloExposeElement = defineCustomElement({
  props: {
    value: String,
  },
  setup(props, { expose }) {
    expose({
      value: 'hello',
    })

    return () => h('div', null, [props.value])
  },

customElements.define('hello-expose-el', HelloExposeElement);
```

```html
<!-- 以下のCustom Elementsにてvalueプロパティはすでに公開されている旨の警告が出る -->
<hello-expose-el value="world"></hello-expose-el>
```

https://github.com/vuejs/core/commit/af838c1b5ec23552e52e64ffa7db0eb0246c3624

### custom-element: support `nonce` option for injected style tags

`nonce`オプションがサポートされて、Custom Elementsに挿入されるstyleタグに `nonce` 属性を追加できるようになりました。

これはコンテンツ・セキュリティ・ポリシーを満たすために `nonce` 属性を含める必要があるため、追加されました。

https://github.com/vuejs/core/commit/bb4a02a70c30e739a3c705b3d96d09258d7d7ded

### custom-element: support passing custom-element-specific options via 2nd argument of defineCustomElement

`defineCustomElement`の第2引数に `CustomElementOptions` を渡せるようになりました。

https://github.com/vuejs/core/commit/60a88a2b129714186cf6ba66f30f31d733d0311e

### custom-element: support `shadowRoot: false` in `defineCustomElement()`

`defineCustomElement`で `shadowRoot: false` の指定をサポートするようになりました。これによりShadow DOMを使わずにCustom Elementsを作成できるようになりました。

https://github.com/vuejs/core/commit/37d2ce5d8e0fac4a00064f02b05f91f69b2d5d5e

## おわりに

この記事では `defineCustomElement` についてと、Vue3.5からの改善内容についてを紹介しました。今回の改善にでは2〜3年前から起票されていた問題を解消するものも含まれており、突然改善された理由は明らかになっておりません。いずれにせよ、Vue.jsからのCustom Elementsの利用がより安定的になり、より多くの開発者で利用しやすくなることが期待されます。
