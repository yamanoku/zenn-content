---
title: "SPAのアクセシビリティを強化する`<NuxtRouteAnnouncer>`について"
emoji: "♿"
type: "tech"
topics: ["Vue", "アクセシビリティ", "スクリーンリーダー"]
published: false
publication_name: "comm_vue_nuxt"
---

## SPAにおけるアクセシビリティ課題について

SPA（シングルページアプリケーション）では

クライアントサイドルーティングにおける画面遷移したことを支援技術に伝えるためにJavaScriptとWAI-ARIAを使用したRoute Announcer（あるいはRoute Announcement）という手法があります。

Gatsby、Next.js、SvelteKit、Angular（Angular Material）、Astroにて実装されています。

<!-- 具体的な問題については以下YouTubeをご参考ください。

https://youtu.be/FmggsMDMqh0?t=560

また、Route Announcerに関する実装についての詳細は[令和最新Route Announcer事情](https://2023.yamanoku.net/2023-12-15/)より参照ください。 -->

## NuxtにおけるRoute Announcerの登場

Nuxt3.12よりRoute Announcer相当の仕組みが導入されています。

* [<NuxtRouteAnnouncer> · Nuxt Components](https://nuxt.com/docs/api/components/nuxt-route-announcer)
* [useRouteAnnouncer · Nuxt Composables](https://nuxt.com/docs/api/composables/use-route-announcer)

今回はそれぞれの機能についてを紹介していきます。

### `<NuxtRouteAnnouncer>`

Route Announcerコンポーネントです。Nuxt 3.12で初期インストールすると以下のように`app.vue`に導入されるようになっています。

```html
<template>
  <div>
    <NuxtRouteAnnouncer />
    <NuxtWelcome />
  </div>
</template>
```

この状態のままだと特に変化はないのですが、各ページにて`useHead`にて`title`を指定することでその真価が発揮されます。

### `useRouteAnnouncer`

Route Annnouncerのcomposables関数です。`<NuxtRouteAnnouncer>`ではこの関数を活用して組み立てられています。

```html
<script setup lang="ts">
  const { message, politeness, set, polite, assertive } = useRouteAnnouncer();
</script>
```

:::message
実は3.12時点では`useRouteAnnouncer`がexportされておらず使えなかったのですが、3.12.2でexportされる修正が入ったので、使用する際は3.12.2以降のNuxtを使用するようにしてください。
:::

## nuxt/a11yの取り組み

NuxtRouteAnnouncerという取り組み以外にもアクセシビリティに関する機能を充実させるものが進められています

## まとめ

Nuxtのクライアントサイドルーティング時のアクセシビリティ課題を解消するNuxtRouteAnnouncerについての紹介になりました。
ぜひアップデートして使ってみてください。
