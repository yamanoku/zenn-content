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

具体的な問題点についてを紹介している動画がありますので、そちらを参照ください。

https://youtu.be/FmggsMDMqh0?t=560

そうした問題点を解消するために、JavaScriptとWAI-ARIAを使用したRoute Announcer（あるいはRoute Announcement）という手法があります[^1]。

[^1]: 実装の詳細については[令和最新Route Announcer事情](https://2023.yamanoku.net/2023-12-15/)より参照ください。

これらの手法はGatsby、Next.js、SvelteKit、Angular（Angular Material）、Astroにて実装されていますが、Nuxtでは未実装でした。

ですがNuxt3.12よりRoute Announcer相当の仕組みが導入されました。

## NuxtにおけるRoute Announcerの登場

Nuxt公式のリリースにて[アクセシビリティの向上](https://nuxt.com/blog/v3-12#built-in-accessibility-improvements)として`<NuxtRouteAnnouncer>`コンポーネントと`useRouteAnnouncer`コンポーザブルが導入されました。

* [<NuxtRouteAnnouncer> · Nuxt Components](https://nuxt.com/docs/api/components/nuxt-route-announcer)
* [useRouteAnnouncer · Nuxt Composables](https://nuxt.com/docs/api/composables/use-route-announcer)

今回はそれぞれの機能についてを紹介していきます。

### `<NuxtRouteAnnouncer>`

スクリーンリーダーのために、ページタイトルの隠し要素を追加するコンポーネントです。

Route Announcerコンポーネントです。Nuxt 3.12で初期インストールすると以下のように`app.vue`に導入されるようになっています。

```html
<template>
  <div>
    <NuxtRouteAnnouncer />
    <NuxtWelcome />
  </div>
</template>
```

具体的な説明は [@splendente](https://zenn.dev/splendente) さんが紹介していますのでそちらをご参照ください。

https://zenn.dev/splendente/articles/nuxt-route-announcer-verification

### `useRouteAnnouncer`

ページタイトルの変更を監視し、それに応じてアナウンサーメッセージを更新するcomposables関数です。`<NuxtRouteAnnouncer>`はこの関数を活用して組み立てられています。

[Unhead](https://unhead.unjs.io/)の`dom:rendered`にフックして、ページのタイトルを読み取り、スクリーンリーダーへの通知されます。

:::message
3.12時点では`useRouteAnnouncer`がexportされておらず使えなかったのですが、[3.12.2](https://github.com/nuxt/nuxt/releases/tag/v3.12.2)でexportされる[修正](https://github.com/nuxt/nuxt/pull/27562)が入ったため、使用する際は3.12.2以降のNuxtを使用するようにしてください。
:::

```html
<script setup lang="ts">
  const { message, politeness, set, polite, assertive } = useRouteAnnouncer();
</script>
```

- `message` ... 読み上げられるメッセージ部分
- `politeness` ... スクリーンリーダーの読み上げ方法。`off`, `polite`, `assertive` が設定できます
- `set(message, politeness = "polite")` ... messageとpolitenessを設定するメソッド
- `polite(message)` ... politenessを`polite`に設定して`set()`を実行するメソッド
- `assertive(message)` ... politenessを`assertive`に設定して`set()`を実行するメソッド

これらはすでに`useHead`で`title`を設定されている場合、上書きすることはできないようになっているみたいです。

## どのような挙動になるのか

この状態のままだと特に変化は見られないのですが、各ページにて`useHead`にて`title`を指定することでその真価が発揮されます。

<!-- 動画を収録して挿入する -->

なんらかの変更があった際のスクリーンリーダーに通知する、という用法ではなくページのタイトルを読み上げるためのものとして活用するものになります。

## nuxt/a11yの取り組み

今回導入されたNuxtRouteAnnouncerという取り組み以外にもアクセシビリティに関する機能を充実させるものが進められています。

https://github.com/nuxt/nuxt/issues/23255

## まとめ

Nuxtのクライアントサイドルーティング時のアクセシビリティ課題を解消するNuxtRouteAnnouncerについての紹介になりました。

SPAでのページ遷移通知をNuxtのみで完結できるようになるため、ぜひアップデートして使ってみてください。
