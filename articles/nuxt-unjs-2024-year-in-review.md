---
title: "Nuxt/UnJSと周辺エコシステムから振り返る2024年"
emoji: "📬"
type: "tech"
topics: ["nuxt", "unjs"]
published: false
---

この記事は[Nuxt / UnJS Advent Calendar 2024](https://qiita.com/advent-calendar/2024/nuxt-and-unjs)の9日目の記事です。

同日の[Vue Advent Calendar 2024](https://qiita.com/advent-calendar/2024/vue)ではVue.jsと周辺のエコシステムについての2024年を振り返っています。併せてご覧ください。

## Nuxt

### Nuxt 3.10

https://nuxt.com/blog/v3-10

Nuxt 3.10のリリースノートの主なポイントを以下にまとめました：

- **実験的な共有asyncDataのprerendering**：prerendering時にデータの再取得を防ぐ機能が追加されました
- **SSRセーフなユニークID生成**：SSRセーフなユニークIDを生成するための`useId`コンポーザブルが追加されました
- **app/router.optionsの拡張**：モジュール作成者が独自のrouter.optionsファイルを注入できるようになりました
- **クライアントサイドのNode.jsサポート**：クライアントサイドでNode.jsのビルトインモジュールをポリフィルする実験的サポートが追加されました
- **`useCookie`のリアクティビティ向上**：`CookieStore`を使用してクッキーの値をリアクティブに更新するオプションが追加されました
- **潜在的なバグやパフォーマンス問題の検出機能の追加**
- **ページごとにView Transitionsのサポートが可能**
- **ビルド時にルートメタデータにアクセスが可能**
- **TypeScriptのバンドラー解決によるバンドラーモジュール解決**

### Nuxt 3.11

https://nuxt.com/blog/v3-11

- **ログ機能の改善**: サーバーサイドレンダリング中にブラウザコンソールにログが表示されるようになりました
- **プレビューモード**: `usePreviewMode`コンポーザブルを使用して、プレビューモードが利用できるようになりました
- **キャッシュバスティングペイロード**: デプロイ後に古いデータが残らないように自動的にキャッシュバスティングが行われます
- **ミドルウェアルートルール**: Vueアプリケーション内のページパスに対してミドルウェアを定義できます
- **新しいデータフェッチユーティリティ**: `useAsyncData`と`useFetch`が`clear`ユーティリティを公開しました
- **新しい`#teleports`ターゲット**: サーバーサイドテレポートをサポートする新しい`<div id="teleports"></div>`要素が追加されました
- **ローディングインジケーターとトランジションコントロール**: ローディングインジケーターのカスタムタイミング設定が可能になりました
- **サーバーおよびクライアント専用ページ**: `.server.vue`または`.client.vue`サフィックスを使用して、サーバーまたはクライアント専用ページを作成できます
- **サーバーコンポーネントの改善**: サーバーコンポーネントの使用がさらに便利になりました
- **パフォーマンスの向上**: 仮想テンプレートの更新やプリレンダリングキャッシュの改善など、多くのパフォーマンス向上が行われました
- **パブリックアセットの取り扱い**: Viteのパブリックアセット取り扱いの再実装が行われました
- **チャンク命名**: JSチャンクのデフォルト命名パターンが変更されました
- **型の修正**

### Nuxt 3.12

https://nuxt.com/blog/v3-12

- **Nuxt 4のテスト**: Nuxt 4の変更点をテストするためのオプションが追加されました。
- **Nuxt Scriptsの自動インストール**: Nuxt Scriptsが公開プレビュー中で、使用時に自動インストールを促すスタブが追加されました。
- **レイヤーの自動登録とバグ修正**: プロジェクト内のレイヤーが自動的に登録されるようになり、依存関係の読み込みが改善されました。
- **アクセシビリティの向上**: 新しいコンポーネントとコンポーザブルが追加され、アクセシビリティが向上しました。
- **パフォーマンスの改善**: モジュールの重複排除やViteの開発サーバーの起動時間短縮など、多くのパフォーマンス改善が行われました。
- **マルチアプリサポート**: 複数のNuxtアプリを並行して実行できるようになるための変更が行われました。
- **開発者体験の向上**: 開発サーバーログの改善や、ブラウザでのエラーメッセージの改善が行われました。
- **機能の安定化**: いくつかの実験的オプションが安定化され、デフォルトで有効になりました。
- **型の改善**: サーバーコンポーネントのスロットやtsconfig.jsonのデフォルト設定の改善が行われました。
- **モジュール作者向けの改善**: モジュールオプションの型サポートや、ランタイム設定のアクセスと更新が可能になりました。

### Nuxt2 EoL

https://nuxt.com/blog/nuxt2-eol

Nuxt2が2024年6月30日にEOLを迎えました。引き続き使う場合はHeroDevsでの有償サポートを受けることができます。

### Nuxt 3.13

https://nuxt.com/blog/v3-13

- **ルートグループ**: ディレクトリ名に括弧や角括弧を使用してルートを整理できるようになりました。これにより、URL構造に影響を与えずにルートをグループ化できます。
- **アイランドとヘッドメタデータ**: サーバーコンポーネントアイランドがヘッドを操作し、SEOメタデータを追加できるようになりました。
- **カスタムプリフェッチトリガー**: NuxtLinkのカスタムプリフェッチトリガーをサポート。ホバーやフォーカス時、または可視化されたときにプリフェッチを実行できます。
- **サーバーソースマップの改善**: サーバービルドのソースマップが元のソースファイルを参照するようになりました。
- **モジュール作成者向けの新機能**: モジュール作成者向けに新しいユーティリティや型推論の改善が追加されました。
- **開発時の警告の改善**: ミドルウェアでのデータフェッチングコンポーザブルの使用に関する警告が削除され、ユーザーコンポーネント名が「Lazy」で始まる場合に警告が表示されるようになりました。
- **Vue TypeScriptの変更**: Vueの型拡張に関する変更があり、ライブラリのコード更新が必要です。

### Nuxt 3.14

https://nuxt.com/blog/v3-14

- **Jitiによる高速起動**: Nuxtの設定ファイルやモジュールの読み込みがjiti v2によって高速化されました。
- **共有フォルダ**: クライアントとサーバーで共有するコードや型のための新しい`shared/`フォルダが追加されました。
- **Rspackビルダー**: 新しいNuxtビルダーとしてrspackが導入されました。まだ実験的ですが、内部の仮想ファイルシステムがunpluginを使用してリファクタリングされています。
- **新しいコンポーザブル**: `useResponseHeader`と`useRuntimeHook`という新しいコンポーザブルが追加されました。
- **新しいモジュールユーティリティ**: Nitroランタイムルート内でアクセス可能な仮想ファイルを追加するための`addServerTemplate`ユーティリティが追加されました。
- **v4への変更**: `compatibilityVersion: 4`を設定することで、いくつかの変更を早期に適用できます。
- **v3.15へのロードマップ**: 次のマイナーリリースでは、モジュールからのディレクティブの自動インポートや遅延ハイドレーションなどが予定されています。

### Nuxt: Looking forward
https://nuxt.com/blog/looking-forward-2024

## NuxtHub
https://hub.nuxt.com/blog/beta

## Nuxt UI

Nuxt UI simplifies the creation of stunning and responsive web applications with its comprehensive collection of fully styled and customizable UI components designed for Nuxt.

## @nuxt/test-utils

## Nuxt DevTools

## nuxt-bridge

## Nuxt Modules

### Nuxt Content

Nuxt Content reads the content/ directory in your project, parses .md, .yml, .csv and .json files to create a powerful data layer for your application. Additionally, you can use Vue components in Markdown with the MDC syntax.


### Nuxt Scripts

https://nuxt.com/blog/nuxt-scripts

### Nuxt Fonts

### NuxtIcon

### Nuxt Image

Plug-and-play image optimization for Nuxt apps. Resize and transform your images using built-in optimizer or your favorite images CDN.

### Nuxt Tailwind

The Tailwind CSS module for Nuxt enables you to set up Tailwind CSS in your Nuxt application in seconds, with many goodies.

### Nuxt SEO

Nuxt SEO is both a collection of modules and a module itself. The Nuxt SEO module is all the SEO modules combined into one that is guaranteed to work well together.

https://nuxtseo.com/announcement

## learn.nuxt.com

## Nuxt Studio
https://nuxt.studio/blog/v2

## Nx Plugin
https://blog.nrwl.io/introducing-nx-nuxt-enhanced-nuxt-js-support-in-nx-01eac78034fc

## Elk

## UnJS

具体的なツールの紹介については[ykoizumi0903](https://zenn.dev/ytr0903)さんの記事をご覧ください。

https://zenn.dev/ytr0903/articles/c6c42147ed29be

https://zenn.dev/ytr0903/articles/6b50bf790c340b
