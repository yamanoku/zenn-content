---
title: "SPAのアクセシビリティを強化するNuxtのRoute Announcer機能"
emoji: "♿"
type: "tech"
topics: ["Vue", "Nuxt", "アクセシビリティ", "スクリーンリーダー"]
published: false
publication_name: "comm_vue_nuxt"
---

## SPAにおけるアクセシビリティ課題について

SPA（シングルページアプリケーション）でのアクセシビリティ課題の一つに、画面遷移時にスクリーンリーダーというWebブラウザでコンテンツを閲覧するための支援技術が、画面遷移したことを認識できないという問題があります。

具体的にどういった事象かと言うと、とあるページから別ページに遷移した時に**そのページ先に移動したこと**がスクリーンリーダーを使う人にとってはわからない、という事象です。これは視覚的に情報を取得できる人であれば気にされませんが、特定のユーザーにとっては問題があります。

そうした問題点を解消するために、JavaScriptとWAI-ARIAを使用したRoute Announcer（あるいはRoute Announcement）という手法があります[^1]。

[^1]: 実装の詳細については[令和最新Route Announcer事情](https://2023.yamanoku.net/2023-12-15/)より参照ください。

これらの手法はGatsby、Next.js、SvelteKit、Angular（Angular Material）、Astroにて実装されていますが、Nuxtでは未実装でした。

この問題点についてと、どのように解消するかについてを昨年の[Vue Fes Japan 2023のやまのく](https://vuefes.jp/2023/sessions/yamanoku)よりRoute Announcer相応のコンポーネントを作るやり方を発表しました[^2]。

[^2]: 発表資料は[画面遷移から考えるNuxtアプリケーションをアクセシブルにする方法](https://yamanoku.net/vuefes-japan-2023/ja/)を参照ください。

ですが、この度Nuxt3.12よりNuxt本体にRoute Announcer相当の機能が導入されました。

## NuxtにおけるRoute Announcerの登場

Nuxt公式のリリースにて[アクセシビリティの向上](https://nuxt.com/blog/v3-12#built-in-accessibility-improvements)の項目で`<NuxtRouteAnnouncer>`コンポーネントと`useRouteAnnouncer` composables関数が導入されました。

* [<NuxtRouteAnnouncer> · Nuxt Components](https://nuxt.com/docs/api/components/nuxt-route-announcer)
* [useRouteAnnouncer · Nuxt Composables](https://nuxt.com/docs/api/composables/use-route-announcer)

今回はそれぞれの機能についてを紹介していきます。

### `<NuxtRouteAnnouncer>`

スクリーンリーダーのために、ページタイトルの隠し要素を追加するコンポーネントです。

Route Announcerコンポーネントです。Nuxt 3.12で初期インストールすると以下のように`app.vue`に導入されるようになっています。

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

ページタイトルの変更を監視し、それに応じてアナウンサーメッセージを更新するcomposables関数です。`<NuxtRouteAnnouncer>`はこの関数を活用して組み立てられています。[Unhead](https://unhead.unjs.io/)の`dom:rendered`にフックして、ページのタイトルを読み取り、スクリーンリーダーへの通知される仕組みになっています。

:::message
3.12時点では`useRouteAnnouncer`がexportされておらず使えなかったのですが、[3.12.2](https://github.com/nuxt/nuxt/releases/tag/v3.12.2)でexportされる[修正](https://github.com/nuxt/nuxt/pull/27562)が入ったため、使用する際は3.12.2以降のNuxtを使用するようにしてください。
:::

```vue
<script setup lang="ts">
  const { message, politeness, set, polite, assertive } = useRouteAnnouncer();
</script>
```

`useRouteAnnouncer`は引数に`politeness`を受取り、`off`（通知を切る）、`polite`（すべての通知が終わった後に通知する）、`assertive`（即座に通知する）のいずれかを設定することができます。デフォルトの値は`polite`になっています。

`useRouteAnnouncer`からは以下プロパティやメソッドが利用できるようになります。

- `message` ... 読み上げられるメッセージ部分
- `politeness` ... スクリーンリーダーの読み上げ方法。`off`, `polite`, `assertive` が設定できます
- `set(message, politeness = "polite")` ... messageとpolitenessを設定するメソッド
- `polite(message)` ... politenessを`polite`に設定して`set()`を実行するメソッド
- `assertive(message)` ... politenessを`assertive`に設定して`set()`を実行するメソッド

注意点としては、これらはすでにページコンポーネントにて`useHead`の`title`が設定されている場合、`message`を上書きすることはできないようになっています。

## どのような挙動になるのか

デフォルトの状態のままだと特に変化は見られないのですが、各ページにて`useHead`で`title`を指定することでその真価が発揮されます。

<!-- 動画を収録して挿入する -->

挙動から分かるとおり、ページ（URL）が切り替わったあとにページのタイトルが読み上げられるようになっています。これまでのNuxtではこうした挙動はデフォルトでは整備されていなかったため、開発者が考慮する必要を減らしてくれるようになっています。

## nuxt/a11yの取り組み

今回導入されたNuxtRouteAnnouncerの取り組み以外にもアクセシビリティに関する機能を充実させるものが進められています。

https://github.com/nuxt/nuxt/issues/23255

ロードマップから、Nuxt内部にvue-axeといったアクセシビリティチェックツールのビルトインやナビゲーションにおけるフォーカス管理、スキップリンクの生成なども予定されています。

## まとめ

Nuxtのクライアントサイドルーティング時のアクセシビリティ課題を解消するNuxtRouteAnnouncerについての紹介になりました。

SPAでのページ遷移通知をNuxtのみで完結できるようになるため、ぜひアップデートして使ってみてください。
