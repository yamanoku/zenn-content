---
title: "SPA のアクセシビリティを強化する Nuxt の Route Announcer 機能"
emoji: "♿"
type: "tech"
topics: ["Vue", "Nuxt", "アクセシビリティ", "スクリーンリーダー"]
published: false
publication_name: "comm_vue_nuxt"
---

## SPAにおけるアクセシビリティ課題について

SPA（シングルページアプリケーション）は、１つの HTML ファイルに対して、JavaScript と WebAPI を活用してスムーズなユーザー体験を実現するアプリケーションの一形態です。

ですが SPA でのアクセシビリティ課題の１つに、画面遷移時にスクリーンリーダーという Web ブラウザでコンテンツを閲覧するための支援技術が、画面遷移したことを認識できないという問題があります。

これはクライアントサイドルーティングによって起こってしまう事象です。具体的にどういった事象かと言うと、とあるページから別ページに遷移した際、**そのページ先に移動したこと**がスクリーンリーダーを使う人にとってはわからない、という事象です。

これは視覚的に情報を取得できる人であれば気にされませんが、支援技術を使うユーザーにとっては何が変わったかが分からない問題があります。

そうした問題点を解消するために、JavaScript と WAI-ARIA を使用した Route Announcer（あるいは Route Announcement）という手法があります[^1]。

[^1]: 実装の詳細については[令和最新Route Announcer事情](https://2023.yamanoku.net/2023-12-15/)より参照ください。

これらの手法は Gatsby、Next.js、SvelteKit、Angular（Angular Material）、Astro にて実装されていますが、Nuxt では未実装でした。

この問題点を Nuxt にてどのように解消するかについてを昨年の[Vue Fes Japan 2023のやまのく](https://vuefes.jp/2023/sessions/yamanoku)より Route Announcer 相応のコンポーネントを作るやり方を発表しました[^2]。

[^2]: 発表資料は[画面遷移から考えるNuxtアプリケーションをアクセシブルにする方法](https://yamanoku.net/vuefes-japan-2023/ja/)を参照ください。

ですが、この度 Nuxt 3.12 より Nuxt 本体に Route Announcer 相当の機能が導入されました。

## Nuxt における Route Announcer の登場

Nuxt 公式のリリースにて[アクセシビリティの向上](https://nuxt.com/blog/v3-12#built-in-accessibility-improvements)の項目で `<NuxtRouteAnnouncer>` コンポーネントと `useRouteAnnouncer` composables 関数が導入されました。

* [<NuxtRouteAnnouncer> · Nuxt Components](https://nuxt.com/docs/api/components/nuxt-route-announcer)
* [useRouteAnnouncer · Nuxt Composables](https://nuxt.com/docs/api/composables/use-route-announcer)

### どのような挙動をしてくれるのか

初期インストールされたデフォルトの状態のままだと特に変化は見られないのですが、各ページにて `useHead` で `title` を指定することでその真価が発揮されます。

挙動から分かるとおり、ページ（URL）が切り替わったあとにページのタイトルが読み上げられるようになっています。

これまでの Nuxt ではこうした挙動はデフォルトで整備されていなかったため、開発者の考慮する必要を減らしてくれるようになりました。

次にコンポーネントと composables 関数のそれぞれについてを紹介していきます。

### `<NuxtRouteAnnouncer>`

スクリーンリーダーのために、ページタイトル要素を取得して通知するためのコンポーネントです。

Route Announcer コンポーネントです。Nuxt 3.12 で初期インストールすると以下のように `app.vue` に導入されます。

```vue
<template>
  <div>
    <NuxtRouteAnnouncer />
    <NuxtWelcome />
  </div>
</template>
```

全体のレイアウトコンポーネントか、ページコンポーネントに設置して使います。

具体的な説明は [@splendente](https://zenn.dev/splendente) さんが紹介していますのでそちらをご参照ください。

https://zenn.dev/splendente/articles/nuxt-route-announcer-verification

### `useRouteAnnouncer`

ページタイトルの変更を監視し、それに応じてアナウンサーメッセージを更新する composables 関数です。`<NuxtRouteAnnouncer>`はこの関数を活用して組み立てられています。[Unhead](https://unhead.unjs.io/)の `dom:rendered` にフックして、ページのタイトルを読み取り、スクリーンリーダーへの通知される仕組みになっています。

:::message
3.12 時点では `useRouteAnnouncer` が export されておらず使えなかったのですが、[3.12.2](https://github.com/nuxt/nuxt/releases/tag/v3.12.2)で export される[修正](https://github.com/nuxt/nuxt/pull/27562)が入ったため、使用する際は 3.12.2 以降の Nuxt を使用するようにしてください。
:::

```vue
<script setup lang="ts">
  const { message, politeness, set, polite, assertive } = useRouteAnnouncer();
</script>
```

`useRouteAnnouncer` は引数に `politeness` を受取り、`off`（通知を切る）、`polite`（すべての通知が終わった後に通知する）、`assertive`（即座に通知する）のいずれかを設定できます。デフォルトの値は `polite` になっています。

`useRouteAnnouncer` からは以下プロパティやメソッドが利用できるようになります。

- `message` ... 読み上げられるメッセージ部分
- `politeness` ... スクリーンリーダーの読み上げ方法。`off`, `polite`, `assertive` が設定できます
- `set(message, politeness = "polite")` ... message と politeness を設定するメソッド
- `polite(message)` ... politeness を `polite` に設定して `set()` を実行するメソッド
- `assertive(message)` ... politeness を `assertive` に設定して `set()` を実行するメソッド

:::message alert
すでにページコンポーネントにて `useHead` の `title` が設定されている場合、`set` メソッドや `message` 自体で上書きできないようになっているため、注意が必要です。動的な変更をする場合は `useHead` の `title` 側から変更してください。
:::

## nuxt/a11yの取り組み

今回導入された `<NuxtRouteAnnouncer>` の取り組み以外にもアクセシビリティに関する機能を充実させるものが進められています。

https://github.com/nuxt/nuxt/issues/23255

ロードマップから、Nuxt 内部に vue-axe といったアクセシビリティチェックツールのビルトインやナビゲーションにおけるフォーカス管理、スキップリンクの生成なども予定されています。

## まとめ

Nuxt のクライアントサイドルーティング時のアクセシビリティ課題を解消する `<NuxtRouteAnnouncer>` についての紹介になりました。

SPA でのページ遷移通知を Nuxt のみで完結できるようになるため、ぜひ 3.12.2 以降にアップデートして使ってみてください。
