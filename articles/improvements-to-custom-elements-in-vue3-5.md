---
title: "defineCustomElementの改善から見るVueとCustom Elementsの関係"
emoji: "⚙️"
type: "tech"
topics: ["vue", "webcomponents"]
published: false
publication_name: "comm_vue_nuxt"
---

## そもそもCustom Elementsとは？

Custom Elementsは[Web Components](https://developer.mozilla.org/ja/docs/Web/API/Web_components)を構成する一部で、独自のHTML要素を作成するためのJavaScript APIです。独自の定義のため、通常のHTML要素とは異なりダッシュが使われている名前(`kebab-case`)である必要があります。

```html
<!-- 純粋なHTML要素 -->
<progress></progress>

<!-- Custom Elements -->
<custom-progress></custom-progress>
```

Custom Elementsの利点として、UIライブラリ・フレームワークを使用していても使用していなくても活用できることです。

Custom Elementsの具体的な使用例を上げると、GitHubのリポジトリ内における日付表示部分は[Web Componentsで作られており](https://github.com/github/relative-time-element)、開発ツールで該当部分を見ると独自のHTML要素で定義されていることがわかります。

![Image from Gyazo](https://i.gyazo.com/51a182f6b9bf3c266eaa65d41434b1b2.png)

![Image from Gyazo](https://i.gyazo.com/474e1423dc198e6fc5805c9349688c20.png)

## Vue.jsでCustom Elementsを使う方法

Vue.jsでは[Custom Elements Everywhere](https://custom-elements-everywhere.com/)によるとCustom Elementsを使うために[必要なテストケースはすべて通過](https://custom-elements-everywhere.com/libraries/vue/results/results.html)しているため、Vue.jsと併用して安心して使うことができます。

ですが実際には使用にするにあたり、以下のような設定が必要になります。

```js
app.config.compilerOptions.isCustomElement = (tag) => tag.includes('-');
```

```js
// vite.config.js
import vue from '@vitejs/plugin-vue';

export default {
  plugins: [
    vue({
      template: {
        compilerOptions: {
          // ダッシュを含むすべてのタグをCustom Elementsとして扱う
          isCustomElement: (tag) => tag.includes('-');
        }
      }
    })
  ]
}
```

## VueコンポーネントをCustom Elementsとして配布する

Vue3.2からは`defineCustomElement`というAPIが追加され、VueコンポーネントをCustom Elementsとして使うことができるようになりました。

### `defineCustomElement`

拡張子を `.vue` から `.ce.vue` に変更することで、Vue.jsのコンポーネントをCustom Elementsとして使うことができます。

```ts
import { defineCustomElement } from 'vue';
import Example from './Example.ce.vue';

// Custom Elementsのコンストラクタに変換
const ExampleElement = defineCustomElement(Example);

// Custom Elementsとして登録
customElements.define('my-example', ExampleElement);
```

公式のSFCツール（vue-loader@^16.5.0、@vitejs/plugin-vue@^1.4.0）は、"Custom Elements Mode"でSFCの読み込みをサポートされており、`ce.vue`拡張子でなくとも`customElement`オプションを追加することでCustom Elementsとして呼び出すことができます。

### 活用事例

実際の活用例は以下のスライドを参照ください。

https://speakerdeck.com/piyoppi/definecustomelement-wohuo-yong-sita-sabisugong-tong-nouikonponentoraiburari

### v3.5で入った改善についての解説

そんなdefineCustomElementですが、Vue3.5のアップデートに伴い、以下のような改善が入りました。

#### custom-element: `useShadowRoot()` helper

Custom Elementsの `shadowRoot` を取得する `useShadowRoot()` ヘルパーが実装されました。

https://github.com/vuejs/core/commit/5a1a89bd6178cc2f84ba91da7d72aee4c6ec1282

#### custom-element: `useHost()` helper

Custom Elementsの `host` を取得する `useShadowRoot()` ヘルパーが実装されました。

https://github.com/vuejs/core/commit/775103af37df69d34c79f12c4c1776c47d07f0a0

#### custom-element: expose `this.$host` in Options API

Options APIでuseHost相当の機能を使うために `this.$host` が実装されました。

https://github.com/vuejs/core/commit/1ef8f46af0cfdec2fed66376772409e0aa25ad50

#### custom-element: inject child components styles to custom element shadow root

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

#### custom-element: support configurable app instance in defineCustomElement

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

#### custom-element: support css `:host` selector by applying css vars on host element

`:host` セレクタをサポートするために、`host` 要素にCSS変数を適用する実装がされました。

https://github.com/vuejs/core/pull/8830


#### custom-element: support emit with options

最初のイベント引数がオブジェクトである場合 [`CustomEvent`](https://developer.mozilla.org/ja/docs/Web/API/CustomEvent) のオプションオブジェクトとして使用されるようになりました。引数リスト全体は `CustomEvent` の `detail` プロパティを通して公開されます。

```js
emit('event', { bubbles: true });
```

https://github.com/vuejs/core/commit/e181bff6dc39d5cef92000c10291243c7d6e4d08

#### custom-element: support expose on customElement

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

#### custom-element: support `nonce` option for injected style tags

`nonce`オプションがサポートされて、Custom Elementsに挿入されるstyleタグに `nonce` 属性を追加できるようになりました。

これはコンテンツ・セキュリティ・ポリシーを満たすために `nonce` 属性を含める必要があるため追加されました。

https://github.com/vuejs/core/commit/bb4a02a70c30e739a3c705b3d96d09258d7d7ded

#### custom-element: support passing custom-element-specific options via 2nd argument of defineCustomElement

`defineCustomElement`の第2引数に `CustomElementOptions` を渡せるようになりました。

https://github.com/vuejs/core/commit/60a88a2b129714186cf6ba66f30f31d733d0311e

#### custom-element: support `shadowRoot: false` in `defineCustomElement()`

`defineCustomElement`で `shadowRoot: false` の指定をサポートするようになりました。これによりShadow DOMを使わずにCustom Elementsを作成できるようになりました。

https://github.com/vuejs/core/commit/37d2ce5d8e0fac4a00064f02b05f91f69b2d5d5e
