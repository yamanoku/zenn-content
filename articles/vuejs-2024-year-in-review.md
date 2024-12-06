---
title: "Vue.jsと周辺エコシステムから振り返る2024年"
emoji: "📬"
type: "tech"
topics: ["vue"]
published: false
---

この記事は[Vue Advent Calendar 2024](https://qiita.com/advent-calendar/2024/vue)の9日目の記事です。

同日の[Nuxt / UnJS Advent Calendar 2024](https://qiita.com/advent-calendar/2024/nuxt-and-unjs)ではNuxtと周辺のエコシステムについての2024年を振り返っています。併せてご覧ください。

## Vue.js

### Vue.js公開10周年

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

## Vitepress

https://blog.vuejs.org/posts/vitepress-1.0

VitePressのv1.0がリリースされました。

先ほど紹介したVapor ModeをVitePressに組み込む取り組みも進められています。

https://x.com/sanxiaozhizi/status/1857155901532320202

## Pinia

### Pinia Colada

https://pinia-colada.esm.dev/

## Vue DevTools

## eslint-plugin-vue

## Vue Language Tools

### Volar

https://gist.github.com/johnsoncodehk/62580d04cb86e576e0e8d6bf1cb44e73



## VueUse

v12よりVue2のサポートを終了し、Vue3のみをサポートするようになりました。

## Vue Macros

## FormKit

## TresJS

## vue-i18n

## UIフレームワーク

### PrimeVue

### Vuetify

### Quasar Components

### Buefy

### BootstrapVue

### Element Plus

## Unovue
https://unovue.com/

### Reka UI
https://reka-ui.com/

### Inspira UI
https://inspira-ui.com/

### Shadcn Vue
https://www.shadcn-vue.com/

## GovUK Vue
https://govukvue.org/

## Storybook
https://storybook.js.org/blog/first-class-vue-support-storybook-8/?source=weeklyVueNews&campaign=137

## v0 for Vue

## The State of Vue.js Report 2025

[The State of Vue.js Report 2025 - Developer Survey](https://docs.google.com/forms/d/e/1FAIpQLSc6QQQ14ZuE4akaTZAkqOAMUSGjXVjKS-KuVu2eZ448jzK1Nw/viewform)
