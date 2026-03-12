---
title: "動的な変更をスクリーンリーダーに伝えるNuxtAnnouncerの紹介"
emoji: "♿"
type: "tech"
topics: ["Vue", "Nuxt", "アクセシビリティ", "スクリーンリーダー"]
published: true
published_at: 2026-03-13 12:00
publication_name: "comm_vue_nuxt"
---

## 動的な変更をスクリーンリーダーに伝える技術

以前の記事で、Nuxt 3.12から導入された `<NuxtRouteAnnouncer>` と `useRouteAnnouncer` について紹介しました。

https://zenn.dev/comm_vue_nuxt/articles/feat-nuxt-route-announcer

これらはSPAにおけるページ遷移時にスクリーンリーダーへページタイトルを自動通知するコンポーネントとコンポーザブルで、クライアントサイドルーティングのアクセシビリティ課題を解消するものでした。

しかしページ遷移以外にも、スクリーンリーダーに伝えたい動的な変更があります。たとえば、フォームバリデーションのエラーメッセージ、トースト通知、ローディング状態の変化などです。

Nuxt 4.4[^1]より、これらのユースケースに対応する `<NuxtAnnouncer>` コンポーネントと `useAnnouncer` コンポーザブルが導入されました。

https://github.com/nuxt/nuxt/pull/34318

[^1]: Nuxt 3系の場合、Nuxt 3.17に同梱されています。

- [<NuxtAnnouncer> · Nuxt Components v4](https://nuxt.com/docs/4.x/api/components/nuxt-announcer)
- [useAnnouncer · Nuxt Composables v4](https://nuxt.com/docs/4.x/api/composables/use-announcer)

今回はこのコンポーネントとコンポーザブルについて紹介します。

## 基本的な使い方

### コンポーネントの設置

`app.vue` またはレイアウトファイルに `<NuxtAnnouncer />` を設置します。

```vue
<template>
  <div>
    <NuxtRouteAnnouncer />
    <NuxtAnnouncer />
    <NuxtPage />
  </div>
</template>
```

`<NuxtRouteAnnouncer>` と併用する場合は上記のように両方を設置します。

### `useAnnouncer` で通知する

ページコンポーネントやその他のコンポーネントで `useAnnouncer` コンポーザブルを使い、スクリーンリーダーへメッセージを通知します。

```vue
<script setup lang="ts">
import { useAnnouncer } from '#app';

const { polite, assertive } = useAnnouncer();

function onSubmit() {
  // フォーム送信処理...
  polite('メッセージが送信されました');
};
</script>
```

## `<NuxtAnnouncer>` のProps

`<NuxtAnnouncer>` は以下のPropsを受け取ります。


```vue
<template>
  <NuxtAnnouncer :atomic="true" politeness="polite" />
</template>
```

### `atomic`

- 型: `boolean`
- デフォルト: `true`

スクリーンリーダーが変更部分のみを読み上げるか、要素全体を読み上げるかを制御します。`true` の場合は要素全体を、`false` の場合は変更された部分のみを読み上げます。

:::message

全体を読み上げるとはどういうことかというと、例えば以下のような要素があったとします。

```html
<div id="clock" role="timer" aria-live="polite" aria-atomic="true">
  <span id="hours">12</span>:
  <span id="minutes">00</span>
</div>
```

この要素内のテキストで `minutes` が動的に切り替わって `01` となったとき、`aria-atomic="true"` では `12:00` から `12:01` に変わったと読み上げられますが、`aria-atomic="false"` では `00` から `01` に変更されたと読み上げられます。

:::

### `politeness`

- 型: `'off' | 'polite' | 'assertive'`
- デフォルト: `'polite'`

スクリーンリーダーの読み上げの優先度を設定します。

- `off` ... 通知を無効にする
- `polite` ... 他の読み上げがすべて終わった後に通知する
- `assertive` ... 他の読み上げを中断して即座に通知する

## `useAnnouncer` コンポーザブルの詳細

`useAnnouncer` からは以下のメソッドが利用できます。

```vue
<script setup lang="ts">
import { useAnnouncer } from '#app';
const { set, polite, assertive } = useAnnouncer();
</script>
```

### `set(message, politeness)`

メッセージと `politeness` を指定して通知します。

```ts
set('5件の結果が見つかりました', 'polite');
```

### `polite(message)`

`politeness` を `polite` に設定して通知します。他の読み上げが終わるのを待ってから通知されるため、補足的な情報の通知に適しています。

```ts
polite('データの読み込みが完了しました');
```

### `assertive(message)`

`politeness` を `assertive` に設定して通知します。他の読み上げを中断して即座に通知されるため、緊急性の高い情報に適しています。

```ts
assertive('入力内容にエラーがあります');
```

### `polite` と `assertive` の使い分け

基本的には `polite` を使用し、ユーザーの即座の注意が必要な場合にのみ `assertive` を使用するようにしてください。`assertive` は他の読み上げを中断するため、多用するとユーザー体験を損なう可能性があります。

- `polite` が適切な場面 ... 検索結果の件数、データの読み込み完了、操作の成功通知
- `assertive` が適切な場面 ... フォームバリデーションエラー、セッション期限切れの警告、重要なエラー通知

## `<NuxtRouteAnnouncer>` と `<NuxtAnnouncer>` の違い

| | `<NuxtRouteAnnouncer>` | `<NuxtAnnouncer>` |
|---|---|---|
| 通知タイミング | ルート遷移時に自動 | 開発者が任意に制御 |
| 通知内容 | ページタイトル（`useHead` の `title`） | 任意のメッセージ |
| コンポーザブル | `useRouteAnnouncer` | `useAnnouncer` |
| 主な用途 | ページ遷移の通知 | フォーム・トースト・ローディング等 |
| `atomic` のデフォルト値 | `false` | `true` |

`<NuxtRouteAnnouncer>` はルート変更時にページタイトルを**自動的に**通知するのに対し、`<NuxtAnnouncer>` は開発者が**任意のタイミングで任意のメッセージ**をスクリーンリーダーに通知できます。この性質の違いによって、それぞれの通知同士が競合することはありません。

## 具体的なユースケース

以下は、`useAnnouncer` を利用した具体的なユースケースのサンプルです。

### フォームバリデーションエラーの通知

フォームの入力にエラーがある場合、`assertive` で即座にエラー内容を伝えます。

```vue
<script setup lang="ts">
import { useAnnouncer, useId } from '#app';
import { ref } from 'vue';

const { assertive } = useAnnouncer();
const email = ref('');
const errorMessage = ref('');
const formId = useId();

function validateForm() {
  if (!email.value) {
    errorMessage.value = 'メールアドレスは必須です';
    assertive(errorMessage.value);
    return false;
  }
  return true;
};
</script>

<template>
  <form @submit.prevent="validateForm">
    <label :for="formId">メールアドレス</label>
    <input :id="formId" v-model="email" type="email" />
    <p v-if="errorMessage" role="alert">{{ errorMessage }}</p>
    <button type="submit">送信</button>
  </form>
</template>
```

### ローディング状態のフィードバック

データ取得中の状態変化を `polite` で通知します。

```vue
<script setup lang="ts">
import { useAnnouncer, useFetch } from '#app';
import { watch } from 'vue';

const { polite } = useAnnouncer();
const { data, status } = useFetch('/api/items');

watch(status, (newStatus) => {
  if (newStatus === 'pending') {
    polite('データを読み込んでいます');
  } else if (newStatus === 'success') {
    polite('データの読み込みが完了しました');
  } else if (newStatus === 'error') {
    polite('データの読み込みに失敗しました');
  }
});
</script>
```

### 検索結果の件数通知

検索後に結果の件数をスクリーンリーダーに伝えます。

```vue
<script setup lang="ts">
import { useAnnouncer } from '#app';
import { ref } from 'vue';

const { polite } = useAnnouncer();
const query = ref('');
const results = ref([]);

async function search() {
  results.value = await $fetch('/api/search', {
    params: { q: query.value },
  })  ;
  polite(`${results.value.length}件の検索結果が見つかりました`);
}
</script>

<template>
  <label>
    検索 <input v-model="query" type="search" @input="search" />
  </label>
  <ul>
    <li v-for="item in results" :key="item.id">{{ item.title }}</li>
  </ul>
</template>
```

## 注意点

ユースケースでお伝えした通り、あくまでも動的な変更をスクリーンリーダーに伝えるため設計されているため、何でも `useAnnouncer` で通知するようにするのはおすすめしません。たいていの操作上では、コンテンツにフォーカスで移動させたり、ネイティブの `<form>` 検証を使用するだけでも情報は伝わります。主にフォーカスしない要素の通知のときに `useAnnouncer` の使用を検討してみてください。

## 余談：VueUseへのコンポーザブル追加要望

私はこのNuxtから呼び出されるコンポーザブルを見たとき、Vue.js単体でも使えたらいいなと思いました。

そこで、[VueUse](https://github.com/vueuse/)という便利なコンポーザブル集にもこの `useAnnouncer` 相当のものを実装したいと思って[Issueにて要望を出した](https://github.com/vueuse/vueuse/issues/5314)ところ、別の方がすでに実装していただいています。

https://github.com/vueuse/vueuse/pull/5315

この記事を書いている現時点ではマージはされていませんが、これが本体に適用されるのを楽しみにしています。

## まとめ

この記事ではNuxt 3.12から `<NuxtRouteAnnouncer>` が導入されSPAのページ遷移の通知が実現し、Nuxt4.4からは `<NuxtAnnouncer>` が追加されたことでページ内の動的な変更の通知もフレームワークレベルでサポートされたことについて紹介しました。

これにより、開発者がアクセシビリティ対応のために自前で `aria-live` の実装をする負担が軽減されます。ぜひ `<NuxtAnnouncer>`、`useAnnouncer` を活用して、より多くのユーザーにとって使いやすいNuxtアプリケーションを構築してみてください。

ちなみに今回の対応は、Nuxtのアクセシビリティに関するロードマップの一部です。

https://github.com/nuxt/nuxt/issues/23255

DevTools上でアクセシビリティチェックが可能になる [nuxt-a11y](https://github.com/nuxt/a11y) モジュールの開発も現在進行中で、Nuxt上でアクセシビリティを考慮した開発をより容易にする方向性で進んでいます。今後もこのロードマップには期待を寄せています。

