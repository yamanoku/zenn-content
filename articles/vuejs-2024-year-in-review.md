---
title: "Vue.jsと周辺エコシステムで振り返る2024年"
emoji: "📅"
type: "tech"
topics: ["vue"]
published: false
publication_name: "comm_vue_nuxt"
---

[Vue Advent Calendar 2024](https://qiita.com/advent-calendar/2024/vue)の9日目の記事です。

この記事では2024年のVue.js、そしてそれらにまつわる周辺エコシステムについてを振り返っていきます。関連する大きな出来事や変更があったものについてを中心にまとめています（この内容も取り上げてほしい！のがあった際はGitHubで編集の提案をください）。

同日の[Nuxt / UnJS Advent Calendar 2024](https://qiita.com/advent-calendar/2024/nuxt-and-unjs)ではNuxtと周辺のエコシステムについての2024年を振り返っています。併せてご覧ください。

## Vue.js

### Vue.js 公開 10 周年

https://x.com/vuejs/status/1753678155444101385

2024年はVue.jsがHackerNewsで公開されてから10周年目を迎える年でした。Vue.jsの進化の歴史については以下の通りになります。

- 2015年10月: Vue1リリース
- 2016年10月: Vue2リリース
- 2018年9月: Vue3の開発が開始
- 2020年9月: Vue3リリース
- 2022年1月: Vue3がデフォルトになる、ドキュメントもVue3がメインに
- 2023年12月: Vue2がEoLを迎える

また、今年の9月の時点でnpmからの総ダウンロード数が10億を突破しました。

https://x.com/vuejs/status/1840300340383756759

### Vue 3.5

https://blog.vuejs.org/posts/vue-3-5

9月にVue 3.5がリリースされました。コードネームは「Tengen Toppa Gurren Lagann」です。このリリースでは以下の変更が含まれています。

- **リアクティブシステムの最適化**: パフォーマンスが向上し、メモリ使用量が大幅に削減されました。
- **リアクティブプロップのデストラクチャリング**: `defineProps`からデストラクチャリングされた変数がリアクティブになりました。
- **SSRの改善**: 遅延ハイドレーションや`useId()`などの新機能が追加されました。
- **カスタムエレメントの改善**: `defineCustomElement` APIに新しい機能が追加されました。
- **新しいAPI追加**
  - `useTemplateRef()`
  - `Deferred Teleport`
  - `onWatcherCleanup()`

### Vapor Mode

## Vue Router

https://router.vuejs.org/

v4.4.0よりRouteNamedMapによる型付きルートのネイティブ・サポートが導入されました。 unplugin-vue-routerなしでマップを定義し、型を自動的に推論させることができるようになりました。

v4.5.0よりRouterLinkに`view-transition` Propが追加されました。

## Vitepress

https://blog.vuejs.org/posts/vitepress-1.0

VitePressは、Vite製の静的サイトジェネレータ（SSG）です。今年よりv1.0がリリースされました。

https://x.com/sanxiaozhizi/status/1857155901532320202

現在、先ほど紹介したVapor ModeをVitePressに組み込む取り組みも進められています。

https://github.com/sxzz/vitepress-vapor

## Pinia

https://pinia.vuejs.org/

[12 月 4 日にリリースされた v2.3.0](https://github.com/vuejs/pinia/blob/v2/packages/pinia/CHANGELOG.md#230-2024-12-04)によるとVue 2.7が必要になるのと、2025年1月に登場するPinia 3.0では、Vue 2のサポートを終了する予定とのことです。

## Vue DevTools

https://x.com/vuejs/status/1850796839295176969

Chrome拡張機能であるVue Devtoolsのv7からはVue3のみをサポートするようになりました。そのためVue2でデバッグする際はv5やv6を別途使用する必要があります。

## eslint-plugin-vue

https://eslint.vuejs.org/

Vue.jsのESLint pluginであるeslint-plugin-vueでは、以下の新たなルールが追加されました。

- [`vue/enforce-style-attribute`](https://eslint.vuejs.org/rules/no-deprecated-delete-set.html)
- [`vue/max-props`](https://eslint.vuejs.org/rules/max-props.html)
- [`vue/require-default-export`](https://eslint.vuejs.org/rules/require-default-export.html)
- [`vue/max-template-depth`](https://eslint.vuejs.org/rules/max-template-depth.html)
- [`vue/no-deprecated-delete-set`](https://eslint.vuejs.org/rules/enforce-style-attribute.html)

そのほかVue3.4からの`v-bind` same-name shorthandと`defineModel`のサポートが追加されたり、Flat ConfigサポートやESLint v9をpeer dependencyとして追加するなどの変更も行われました。

## Volar（Vue Language Tools）

https://volarjs.dev/

Volar（いわゆるVue Language Toolsと呼ばれるもの）の[v2.0.0](https://gist.github.com/johnsoncodehk/62580d04cb86e576e0e8d6bf1cb44e73)が今年リリースされました。Takeover Modeは廃止され、Vueファイルに対するすべてのTS機能を引き継ぐHybrid Modeが導入され、vue-tscを書き換えてメモリ使用量を大幅に削減するなどの変更が加えられました。

## VueUse

https://vueuse.org/

今年はv11、v12のメジャーバージョンアップがありました。[v12](https://github.com/vueuse/vueuse/releases/tag/v12.0.0)よりVue2のサポートを終了し、Vue3のみをサポートするようになりました。

## vue-i18n

https://github.com/intlify/vue-i18n/releases/tag/v10.0.0

Vue.jsでのi18nプラグインであるvue-i18nでのv10.0.0では軽量版のpetite-vue-i18n、JITコンパイルの有効化がデフォルトに、設定されたLocaleに合わせた型生成、レガシー APIモードの`$t`と`t`のオーバーロードシグネチャの変更などがされました。

次期バージョンのv11からはLegacy API modeの非推奨化や、v10から非推奨としていた`$tc`と`tc`のAPI廃止が計画されています。

## UI フレームワーク

Vue.jsにまつわるUIフレームワークやコンポーネントライブラリで大きな変更があったものについてを取り上げていきます。

### PrimeVue

https://x.com/primevue/status/1833504714908176650

PrimeVueは2024年9月時点でnpmでの月間100万ダウンロードを突破しました。

今年はフォームを構築できる[Form ライブラリ](https://primevue.org/forms/)、[Figma UI Kit](https://primevue.org/uikit/)、コピーペーストで使用できるUI群の[Prime Blocks](https://primeblocks.org/)のVue.js版が公開されました。

### Vuetify

https://vuetifyjs.com/en/blog/state-of-the-union-2024/

認証と状態管理のためのツールである[Vuetify One](https://one.vuetifyjs.com/)、コード共有ツールである[Vuetify Bin](https://bin.vuetifyjs.com/)、プレグラウンドツールである[Vuetify Playground](https://play.vuetifyjs.com/)、Vuetifyを使ったUIスニペット集を公開するサイトである[Vuetify Snips](https://snips.vuetifyjs.com/)といったVuetifyにまつわるエコシステムツールやサイトが開発・公開されました。

https://x.com/vuetifyjs/status/1859672959822201020

11月にはVuetifyのテーマエディターでもある[Vuetify Studio](https://studio.vuetifyjs.com/)がリリースされました。

### Oku UI

https://oku-ui.com/

Vue.js、Nuxt利用できるUIコンポーネントライブラリである[Oku Primitives](https://primitives.oku-ui.com/)を公開しているOku UIでモーションにまつわるライブラリの[Oku Motion](https://motion.oku-ui.com/)、NuxtとNitro Kitでフルスタックアプリケーション開発ができるようになる[Pergel](https://pergel.oku-ui.com/)が今年公開されました。

### Unovue

https://unovue.com/

Vueのコンポーネントやユーティリティ集であるUnovueが公開されました。現在、ヘッドレスコンポーネントのRadix Vueをリブランドした[Reka UI](https://reka-ui.com/)、再利用可能なVueコンポーネント群の[Inspira UI](https://inspira-ui.com/)、shadcn/uiの非公式、コミュニティ主導のVue移植版の[shadcn-vue](https://www.shadcn-vue.com/)がまとめられています。

## GovUK Vue

https://govukvue.org/

イギリス政府のデザインシステムであるGOV.UK Design SystemにVueコンポーネントを提供するコミュニティ・プロジェクトが公開されました。

## Storybook

https://storybook.js.org/blog/first-class-vue-support-storybook-8/

Storybook 8はVue公式のLanguage ToolsであるVolarを活用してStorybookとVue.jsの連携が強化されました。docgenでは`vue-component-meta`を使用するようになりました。Reactをpeer dependencyとしてインストールする必要がなくなりました。

## v0 for Vue

https://x.com/v0/status/1834036748932055175

Vercelが提供するAI駆動型のUI生成ツール「V0」がVue.jsでのアプリケーション生成に対応しました。

## The State of Vue.js 2025 Survey

[The State of Vue.js Report 2025 - Developer Survey](https://docs.google.com/forms/d/e/1FAIpQLSc6QQQ14ZuE4akaTZAkqOAMUSGjXVjKS-KuVu2eZ448jzK1Nw/viewform)

Vue.jsの開発者向けアンケート調査が公開されました。Vue.jsやNuxtの利用状況や課題についての項目についてを回答できます。この結果を元に来年には「The State of Vue.js Report 2025」としてレポートが公開される予定です。
