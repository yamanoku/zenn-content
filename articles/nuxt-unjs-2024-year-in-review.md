---
title: "Nuxt/UnJSと周辺エコシステムで振り返る2024年"
emoji: "📅"
type: "tech"
topics: ["nuxt", "unjs"]
published: true
published_at: 2024-12-09 12:00
publication_name: "comm_vue_nuxt"
---

[Nuxt / UnJS Advent Calendar 2024](https://qiita.com/advent-calendar/2024/nuxt-and-unjs)の9日目の記事です。

この記事では2024年のNuxtとUnJS、そしてそれらにまつわる周辺エコシステムについてを振り返っていきます。関連する大きな出来事や変更があったものについてを中心にまとめています（この内容も取り上げてほしい！のがあった際GitHubで編集の提案をください）。

同日の[Vue Advent Calendar 2024](https://qiita.com/advent-calendar/2024/vue)ではVue.jsと周辺のエコシステムについての2024年を振り返っています。併せてご覧ください。

## Nuxt

https://x.com/nuxt_js/status/1850123359297614324

Nuxtは今年でリリースされてから8年が経ちました。

### Nuxt2 EoL

https://x.com/Atinux/status/1856402125418574056

Nuxtのダウンロード数はNuxt 3が約75%を占めており、多くの開発者がNuxt 3への移行を進めています。

https://nuxt.com/blog/nuxt2-eol

一方で前バージョンのNuxt 2は今年の6月30日にEOLを迎えました。引き続き使う場合はHeroDevsでの有償サポートを受けることができます。

### Nuxt 3.10

https://nuxt.com/blog/v3-10

- **共有されたasyncDataのprerendering**：prerendering時にデータの再取得を防ぐ機能が追加されました
- **SSRセーフなユニークIDの生成**：SSRセーフなユニークIDを生成するための`useId`コンポーザブルが追加されました
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
- **プレビューモード**: [`usePreviewMode`](https://nuxt.com/docs/api/composables/use-preview-mode)を使用して、プレビューモードが利用できるようになりました
- **Cache-busting payloads**: デプロイ後に古いデータが残らないように自動的なキャッシュバスティングが行われるようになりました
- **Middleware `routeRules`**: Nuxtアプリケーション内のページパスに対してミドルウェアを定義できます
- **新しいデータフェッチユーティリティ`clear` **: `useAsyncData`と`useFetch`が`clear`ユーティリティを公開しました
- **新しい`#teleports`ターゲット**: サーバーサイドテレポートをサポートする新しい`<div id="teleports"></div>`要素が追加されました
- **ローディングインジケーターとトランジションコントロール**: ローディングインジケータを非表示にし、必要に応じて`finish()`メソッドを強制的に実行するためのカスタムタイミングを設定できるようになり、View Transitions APIにフックする`page:view-transition:start`フックが登場しました
- **サーバーおよびクライアント専用ページ**: `.server.vue`または`.client.vue`サフィックスを使用して、サーバーまたはクライアント専用ページを作成できます
- **サーバーコンポーネントの改善**
- **パフォーマンスの向上**
- **パブリックアセットの取り扱いの変更**
- **JSチャンクのデフォルト命名パターンの変更**
- **型の修正**

### Nuxt 3.12

https://nuxt.com/blog/v3-12

- **Nuxt 4の変更テスト**: Nuxt 4の変更箇所をテストするために設定にて`compatibilityVersion`オプションが追加されました。
- **Nuxt Scriptsの自動インストール**
- **レイヤーの自動登録とバグ修正**: プロジェクト内の`~/layers`が自動的に登録されるようになり、依存関係の読み込みが改善されました
- **アクセシビリティの向上**: [`<NuxtRouteAnnouncer>`](https://nuxt.com/docs/api/components/nuxt-route-announcer)と[`useRouteAnnouncer`](https://nuxt.com/docs/api/composables/use-route-announcer)が追加され、SPAでのルート遷移時の状態をスクリーンリーダーに通知できるようになりました
- **マルチアプリサポート**: 複数のNuxtアプリを並行して実行できるようになるための変更が行われました
- **機能の安定化**: いくつかの実験的オプションが安定化に伴い、experimentalから削除されました
- **モジュール作者向けの改善**: モジュールオプションの型サポートや、ランタイム設定のアクセスと更新が可能になりました
- **パフォーマンスの改善**
- **開発者体験の向上**
- **型の改善**

### Nuxt 3.13

https://nuxt.com/blog/v3-13

- **Route Groups**: ディレクトリ名に括弧や角括弧を使用してルートを整理できるようになりました。これにより、URL構造に影響を与えずにルートをグループ化できます
- **IslandとHeadメタデータ**: サーバーコンポーネントから`head`を操作し、SEOメタデータを追加できるようになりました
- **カスタムプリフェッチトリガー**: NuxtLinkで`prefetch-on`トリガーをサポート。ホバーやフォーカス時、または可視化されたときにプリフェッチを実行できます
- **サーバーソースマップの改善**: サーバービルドのソースマップが元のソースファイルを参照するようになりました
- **モジュール作者向けの新機能**: モジュール作者向けに新しいユーティリティや型推論の改善が追加されました
- **開発時の警告の改善**: ミドルウェアでのデータフェッチングコンポーザブルの使用に関する警告が削除され、ユーザーコンポーネント名が「Lazy」で始まる場合に警告が表示されるようになりました
- **Vue TypeScriptの変更**: Vueの型拡張に関する変更があり、ライブラリのコード更新が必要になりました

### Nuxt 3.14

https://nuxt.com/blog/v3-14

- **Jitiによる高速起動**: Nuxtの設定ファイルやモジュールの読み込みがjiti v2によって高速化されました
- **`shared/`フォルダ**: クライアントとサーバーで共有するコードや型のための新しい`shared/`フォルダが追加されました
- **Rspackビルダー**: 新しいNuxtのバンドラーとしてrspackが実験的に導入されました
- **新しいコンポーザブル**: `useResponseHeader`と`useRuntimeHook`という新しいコンポーザブル関数が追加されました
- **新しいモジュールユーティリティ**: Nitroランタイムルート内でアクセス可能な仮想ファイルを追加するための`addServerTemplate`ユーティリティが追加されました
- **v4への変更**: `compatibilityVersion: 4`を設定することで、いくつかの変更を早期に適用できるようになります

### Codemod for Nuxt 4 migration

https://codemod.com/blog/nuxt-announcement

CodemodではNuxtチームと連携し、Nuxt 3から4への移行用のオープンソースのツールを公開しました。

## Nuxt Certification

https://certificates.dev/nuxt

Nuxtの公式認定試験である「Nuxt Certification」が今年から開始されました。コーディング課題に取り組み、Nuxt開発者としてのスキルをテストできます。

## NuxtHub

https://hub.nuxt.com/blog/beta

今年発表されたNuxtHubは、Nuxtフレームワークの拡張機能で、Cloudflare上でフルスタックアプリケーションをZero configで構築できるようになるソフトウェアです。

実際に活用している事例としては以下記事をご参照ください。

https://zenn.dev/comm_vue_nuxt/articles/nuxt-hub-cloudflare-2025-coedo-org

https://zenn.dev/yamanoku/articles/vuefes-japan-speakers

## Nuxt Studio

https://nuxt.studio/blog/v2

Nuxt StudioはNuxt Contentを活用したウェブサイト用のGitベースのCMSです。今年はv2がリリースされ、以下の変更が行われました。

- インターフェースの再設計・刷新
- Google認証の追加
- ライブプレビュー設定の簡素化

## Nuxt UI

https://x.com/benjamincanac/status/1841456682620072231

NuxtアプリケーションのためのUIライブラリであるNuxt UIは今年より次期バージョンであるv3に向けてアルファ、ベータ版の開発が進められています。主にReka UI（Radix Vueの後継UIライブラリ）、Tailwind CSS v4、Tailwind Variantsを活用したもので構築されています。

## nuxt-bridge

https://x.com/nuxt_js/status/1745823076988686538

Nuxt2からNuxt3へのマイグレーションを支援するツールであるNuxt Bridgeの安定版がリリースされました。

## Nuxt Modules

Nuxtアプリケーションで利用できるモジュール群であるNuxt Modulesで大きな変更があったものについてを取り上げます。

### Nuxt Content

https://content.nuxt.com/

Nuxtディレクトリの`content/`配下のファイルを活用したブログ・ドキュメントなどのコンテンツ管理ができるNuxt Contentは現在v3の開発が進められています。

https://x.com/nuxtstudio/status/1861448306338992295

https://x.com/nuxtstudio/status/1865046985880785191

前述したNuxt Studioとのサイトドメインの共通化、Nuxt Contentの内容をNuxt Studioにて直接編集が可能になるなど、連携が進められています。

### Nuxt Fonts

https://github.com/nuxt/fonts

Nuxt FontsはNuxtアプリでお気に入りのフォントソースを使用した際に最適化できるモジュールです。

https://x.com/nuxt_js/status/1834350374100287596

`font-family`の宣言だけでカスタムフォントの追加と最適化を実施してくれます。

### Nuxt Icon

https://nuxt.com/blog/nuxt-icon-v1-0

Nuxt IconはIconifyをベースにしたNuxtアプリケーション用のアイコンライブラリです。今年v1.0がリリースされました。

### Nuxt Leaflet

https://leaflet.nuxtjs.org/

Nuxt Leafletは[Leaflet](https://leafletjs.com/)をラップしたリアクティブな地図を作成できるモジュールです。今年より公式のNuxtモジュールのリポジトリに追加されました。

### Nuxt Scripts

https://nuxt.com/blog/nuxt-scripts

Nuxt Scriptsはサードパーティ製スクリプトをNuxtアプリケーションで組み込む際にプライバシー・セキュリティに配慮し最適化するモジュールです。Daniel Roe氏によるRFCを経て今年の5月にパブリックプレビューとなりました。

具体的な内容・使い方については以下記事をご参照ください。

https://zenn.dev/comm_vue_nuxt/articles/what-is-nuxt-scripts

### Nuxt SEO

https://nuxtseo.com/announcement

Nuxt SEOはNuxtアプリケーションやサイトのオーガニックトラフィックを増やすための技術的な側面を処理してくれるモジュールです。v2.0.0が11月25日にリリースされました。これまで個別でインストールが必要だったモジュールが`@nuxtjs/seo`に統一されています。今後はNuxt SEO Proの有料版も公開される予定です。

## learn.nuxt.com

https://learn-nuxt.vuejs-jp.org/

learn.nuxt.comはNuxt公式で提供されているチュートリアルサイトで、すべてWeb上で完結できるものです。今年のVue Fes Japan 2024では日本語版サイトと[リソース](https://github.com/vuejs-jp/learn.nuxt.com)が公開されました。

https://note.com/ubugeeei/n/n2ac2b02043da

## Nx Plugin

[Introducing @nx/nuxt: Enhanced Nuxt.js Support in Nx | by Katerina Skroumpelou | Nx Devtools](https://blog.nrwl.io/introducing-nx-nuxt-enhanced-nuxt-js-support-in-nx-01eac78034fc)

モノレポ管理ツールであるNxにNuxtプロジェクトをサポートするためのプラグイン`@nx/nuxt`が公開されました。

## UnJS

https://x.com/unjsio/status/1743239935946072575

UnJSはあらゆるJavaScriptフレームワーク上で統一的に動作するユーティリティーツール・ライブラリです。今年の1月には公式サイトがリニューアルされました。

https://unjs.io/blog/2024-02-07-introducing-unjs-relations

UnJSパッケージやnpmパッケージの関係をリアルタイムで視覚化できる[UnJS Relations](https://unjs.io/relations)が公開されました。

### Nitro

NitroはNuxtで採用されているUnJSパッケージを活用したサーバーエンジンです。

今年のVue.js Amsterdamにて[v2.9.0](https://github.com/nitrojs/nitro/releases/tag/v2.9.0)がライブリリースされました。

- ドキュメントの刷新
- WebSockets API
- SQL database layer
- Nitro Tasks
- unwasmの採用

[v2.10.0](https://github.com/nitrojs/nitro/releases/tag/v2.10.0)ではNitro v3に向けての内部リファクタリングを行い、以下変更が含まれております。

- Compatibility date
- Environment-specific handlers
- Route groups
- OpenAPIのプロダクションサポート
- Cloudflare, Netlify, VercelらのPresetの更新

### 2024 年リリースされた UnJS 公式パッケージ、UnJS プラグイン

今年リリースされたパッケージやプラグインについてをアルファベット順で列挙しています。

https://github.com/unjs/automd

https://github.com/unjs/codeup

https://github.com/unjs/compatx

https://github.com/unjs/confbox

https://github.com/unjs/errx

https://github.com/unjs/glob-native

https://github.com/unjs/impound

https://github.com/unjs/mdbox

https://github.com/unjs/srvx

https://github.com/unjs/undio

https://github.com/unjs/undocs

https://github.com/unjs/unifont

https://github.com/unjs/unrouting

https://github.com/unplugin/unplugin-inline-enum

https://github.com/unplugin/unplugin-isolated-decl

https://github.com/unplugin/unplugin-replace

https://github.com/unplugin/unplugin-unused

https://github.com/unplugin/unplugin-vue-fervid

https://github.com/unplugin/unplugin-vue-jsx-vapor
