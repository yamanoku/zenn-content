---
title: "Nuxt設定ファイルにあるcompatibilityDateとは何か？"
emoji: "⚙"
type: "tech"
topics: ["Nuxt", "Nitro"]
published: true
publication_name: "comm_vue_nuxt"
---

皆さんはNuxtで新しいプロジェクトを作成するとき、設定ファイルに `compatibilityDate` という項目を見かけたことはありませんか？

```ts:nuxt.config.ts
// https://nuxt.com/docs/api/configuration/nuxt-config
export default defineNuxtConfig({
  compatibilityDate: '2025-07-15',
  devtools: { enabled: true }
})
```

この記事では、`compatibilityDate` が果たす役割や、どのようにNuxtに影響しているのかを詳しく解説します。

## `compatibilityDate` は何に使われているの？

Nuxtの公式ドキュメントを見てみると以下のように記載されています。

> ## compatibilityDate
> Specify a compatibility date for your app.
> 
> This is used to control the behavior of presets in Nitro, Nuxt Image and other modules that may change behavior without a major version bump. We plan to improve the tooling around this feature in the future.
> 
> [Nuxt Configuration v4](https://nuxt.com/docs/4.x/api/nuxt-config#compatibilitydate)

意訳すると「`compatibilityDate` は、アプリの互換性を保つための日付を指定するものであり、将来的にはこの機能に関するツールを改善する予定です」と書いてあります。

Nuxtアプリの互換性を保つためのもの…というのは名前からもわかるのですが、ドキュメントだけでは具体的な用途が少し分かりにくい印象です。

そもそも、この `compatibilityDate` はNuxt内部のどこで使われているのでしょうか？次に、Nuxtのソースコードを確認してみましょう。

https://github.com/nuxt/nuxt/blob/911ce76cb06beb8850aac033fe2b4addadbd7146/packages/nuxt/src/core/nuxt.ts#L143

https://github.com/nuxt/nuxt/blob/911ce76cb06beb8850aac033fe2b4addadbd7146/packages/nuxt/src/core/nuxt.ts#L170-L180

上記のコードはNuxtのコア部分で、`compatibilityDate` の値を設定している箇所です。ここでは、もし `compatibilityDate` が設定されていなければ、デフォルトで `2025-07-15` を使うようにしています（2025年9月現在）。

https://github.com/nuxt/nuxt/blob/412786a1d8283aaa2360a855d44ebd3ac21924f1/packages/nuxt/src/core/nitro.ts#L509-L513

次にNuxtのサーバーサイド相当の挙動を担うNitroの部分です。ここに先ほどの `nuxt.options.compatibilityDate` が `createNitro` の引数に渡されています。

このことから、`compatibilityDate` はNitroのために使われていることがわかります。

## Nitroでの `compatibilityDate` の使われ方

では次にNitroのドキュメントを見てみましょう。

> ## Compatibility date
> Deployment providers regularly update their runtime behavior. Nitro presets are updated to support these new features.
> 
> To prevent breaking existing deployments, Nitro uses compatibility dates. These dates let you lock in behavior at the project creation time. You can also opt in to future updates when ready.
> 
> When you create a new project, the compatibilityDate is set to the current date. This setting is saved in your project's configuration.
> 
> You should update the compatibility date periodically. Always test your deployment thoroughly after updating. Below is a list of key dates and their effects.
> 
> [Overview - Nitro](https://nitro.build/deploy#compatibility-date)

意訳すると「`compatibilityDate` は、Nitroプリセットの挙動を更新する際に、既存のデプロイが壊れないようにするためのものであり、新しいプロジェクトを作成するときに現在の日付に設定されます」と書かれています。

ここで言うNitroプリセットとは何を指すのでしょうか？ **プリセットとは、Nitroにおいて特定のデプロイ環境やランタイム向けに最適化された設定**のことを指します。

プリセットには以下のランタイムが含まれています。

- AWS
- Azure
- Cloudflare
- Deno
- Firebase
- Netlify
- Vercel等

つまり、 **`compatibilityDate` はこれらのランタイムの挙動が更新されたときに、既存のNuxtアプリケーションの挙動が壊れないようにする後方互換性を保つための設定**、ということになります。

`compatibilityDate` は `2025-07-15` のように日付指定をすることで、2025年7月15日時点までのランタイムの挙動を維持する、という意味になります。これより先に出た新機能を使う場合は、`compatibilityDate` を更新する必要があります。

nuxt.config.tsからこの設定を制御できるため、Nuxtのコアな機能に影響するのではないかと思われそうですが、 **`compatibilityDate` によってNuxtの基本的な挙動には影響しません**。影響するのは、Nitroや `@nuxt/image` といった「デプロイ環境により影響を受けるパッケージ」のみです。

https://image.nuxt.com/get-started/providers

## `compatibilityDate` が指定できる値

以上のように `compatibilityDate` は日付（`YYYY-MM-DD`）形式で指定できるというのが分かりました。一方で日付以外にも指定できる値があります。それは `latest` です。

Nuxtの `src/core/nuxt.ts` にあった変数 `fallbackCompatibilityDate` は `DateString` 型をキャストしています。この `DateString` 型は以下のように定義されています。

https://github.com/unjs/compatx/blob/4d7c1e140af76d1514c937c6101eda1c637907ec/src/date.ts#L116-L127

```ts:nuxt.config.ts
export default defineNuxtConfig({
  compatibilityDate: 'latest',
})
```

`latest` を指定すると、常にデプロイ環境の最新の挙動が適用されます。常に新機能を取り入れたい場合に便利ですが、予期せぬ変更が起こる可能性もあるため、本番環境での使用は慎重に行う必要があります。

また、`compatibilityDate` ではプリセットを個別指定ができます。

```ts:nuxt.config.ts
export default defineNuxtConfig({
  compatibilityDate: {
    default: '2025-07-15',
    netlify: '2025-05-01'
  }
})
```

このようにプリセットごとに異なる日付を指定することで、特定のデプロイ環境だけを新しい挙動に切り替えることができます。たとえば、Netlifyでは2025年5月1日時点の挙動を適用しつつ、他の環境ではそれ以降の変更を反映させないようにできます。

指定できるプリセットのサービス一覧は以下の通りです。

https://github.com/unjs/compatx/blob/4d7c1e140af76d1514c937c6101eda1c637907ec/src/platforms.ts#L4-L12


## まとめ

- `compatibilityDate` はデプロイ環境の挙動が更新されたときに、既存のNuxtアプリケーションが壊れないようにするための設定
- `compatibilityDate` の設定はNuxtのコア機能には直接影響しない
- `compatibilityDate` は日付または `latest` を指定したり、プリセットごとに個別に指定したりできる

`compatibilityDate` を変更する際は、対象のデプロイ環境における挙動の変化も十分に確認したうえで、慎重に設定を更新することが重要です。

今回の記事の内容は、Alexander Lichter氏の動画でも解説されています。併せてご覧になってみてください。

https://www.youtube.com/watch?v=RqOPfhAWSl0

## 謝辞

本記事は、DeepWikiによるリポジトリ内要約、Copilot in Edgeによる動画要約を協力してもらい作成されました。感謝申し上げます。
