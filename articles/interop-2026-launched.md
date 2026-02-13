---
title: "Interop 2026が始まりました！"
emoji: "🎇"
type: "tech"
topics: ["ブラウザ", "Interop", "Web標準"]
published: true
---

2月13日（日本時間）、ブラウザ間でのWeb標準の相互運用性を向上するプロジェクト「Interop」の2026年プロジェクトが始まりました。

この記事では、昨年のInterop 2025の振り返りと、新たにInterop 2026でどのAPIが重点対象として選出されたのかについて触れていきます。

## Interop 2025の振り返り

2026年のInteropプロジェクトについて触れる前に、まずは昨年のInterop 2025プロジェクトについて振り返ってみましょう。

| Stable | Experimental |
| --- | --- |
|  |  |

Interop 2025では、**Navigation API**や**CSS Anchor Positioning**といった、Webアプリケーションの構築方法を根本から変えるような機能が重点対象でした。

最新のテスト結果を見ると、これらの機能は各ブラウザでの実装が大きく進み、特にNavigation APIはSafariを含む主要ブラウザで安定して動作する段階に達しました。また、`<details>` 要素の機能拡張（`name` 属性によるアコーディオン動作など）も、相互運用性が確保されています。

一方で、View Transition APIについてはSPA（単一ドキュメント）での動作は安定しましたが、MPA（複数ページ）での挙動についてはまだ課題が残る結果となり、2026年への継続課題となっています。

:::details 2025年の重点対象リスト（完了・継続含む）

* CSS Anchor positioning
* `backdrop-filter`
* Core Web Vitals (LCP, INP)
* `<details>` element
* Layout (Flexbox/Grid/Subgrid)
* Modules (JSON modules)
* Navigation API
* Pointer and Mouse events
* `@scope`
* `scrollend` event
* Storage Access API
* `text-decoration`
* URLPattern
* View Transition API (SPA)
* WebAssembly (JS String Builtins)
* WebRTC
* Writing modes
* Mutation Eventの削除
:::

## Interop 2026での重点対象について

ここからは、Interop 2026で**新しく重点対象として追加されたもの**、および重要なアップデートがあった機能について見ていきます。

今年は「リッチな表現とパフォーマンスの両立」および「プライバシーとセキュリティ」が大きなテーマとなっているようです。

### View Transitions (Cross-Document / MPA)

2025年では単一ドキュメント（SPA）内での遷移に焦点が当てられていましたが、2026年では**異なるドキュメント間（MPA）でのビュー遷移**が重点対象となりました。これにより、従来のマルチページアプリケーションでも、SPAのような滑らかなページ遷移アニメーションが可能になります。

```css
@view-transition {
  navigation: auto;
}

```

* [MDN: View Transition API](https://developer.mozilla.org/docs/Web/API/View_Transition_API)

### Scroll-driven Animations

スクロール量に応じてアニメーションを制御する機能です。これまでJavaScript（`IntersectionObserver` など）を駆使して実装していたパララックス効果や、スクロールに連動するプログレスバーなどが、CSSのみで、しかもメインスレッドをブロックせず実装可能です。

```css
/* スクロールに合わせてプログレスバーを伸縮させる例 */
@keyframes grow-progress {
  from { transform: scaleX(0); }
  to { transform: scaleX(1); }
}

#progress {
  animation: grow-progress auto linear;
  animation-timeline: scroll();
}

```

* [MDN: Scroll-driven Animations](https://www.google.com/search?q=https://developer.mozilla.org/docs/Web/CSS/CSS_scroll-driven_animations)

### `field-sizing: content`

フォーム入力要素（`<textarea>` など）のサイズを、入力されたコンテンツの内容に合わせて自動的に調整するCSSプロパティです。これまでJavaScriptで `input` イベントを監視して高さを計算していた実装が、CSS 1行で完結するようになります。

```css
textarea {
  field-sizing: content;
}

```

* [MDN: field-sizing](https://www.google.com/search?q=https://developer.mozilla.org/docs/Web/CSS/field-sizing)

### HTML `<search>` Element

検索機能を表すためのランドマーク要素である `<search>` 要素のアクセシビリティと相互運用性に焦点が当てられます。`role="search"` と同等の意味を持ちますが、全てのブラウザで正しくアクセシビリティツリーにマッピングされることを目指します。

```html
<search>
  <form action="/search">
    <input type="search" name="q">
    <button type="submit">検索</button>
  </form>
</search>

```

* [MDN: search element](https://www.google.com/search?q=https://developer.mozilla.org/docs/Web/HTML/Element/search)

### WebAuthn (Passkeys)

パスワードレス認証を実現するWebAuthnについて、特に「Passkeys（パスキー）」のユーザー体験（UI）や、クロスデバイスでの認証フローの相互運用性を高めることが目標とされています。条件付きUI（オートフィルでのパスキー提案）などが対象です。

### Compute Pressure API

デバイスのCPU負荷状態などをWebアプリ側で検知し、高負荷時にはビデオの画質を落とす、エフェクトを減らすといったパフォーマンス制御するためのAPIです。Web会議ツールや高負荷なWebゲームでの体験向上を目指します。

### フェデレーション資格情報管理 (FedCM)

サードパーティCookieの廃止に伴い、プライバシーを保護しつつID連携（「Googleでログイン」など）を実現するためのAPI、FedCM (Federated Credential Management) の相互運用性が重点対象に追加されました。

## Interop 2026での調査対象について

今年の調査対象（Investigation）には、将来的なWebの在り方を左右する野心的な項目が含まれています。

* **AI Model Execution**: ブラウザ内で小規模なLLMや推論モデルを直接実行するための標準化に向けた調査。
* **File System Access**: ローカルファイルへのアクセス権限とセキュリティモデルの統一。
* **Installability**: PWAのインストール体験の統一と、ホーム画面に追加されたアプリの挙動について。

## 参加企業によるInterop 2026プロジェクト開始にまつわる記事

主要ブラウザベンダー各社からもInterop 2026に関する声明が出されています。

* **Apple (WebKit)**: [Announcing Interop 2026](https://webkit.org/blog/17818/announcing-interop-2026/)
* **Google (web.dev)**: [Interop 2026: The next era of web compatibility](https://www.google.com/search?q=https://web.dev/blog/interop-2026)
* **Mozilla (Mozilla Hacks)**: [Interop 2026 and the future of Firefox](https://www.google.com/search?q=https://hacks.mozilla.org/2026/02/interop-2026/)
* **Microsoft (Edge Blog)**: [Microsoft Edge supports Interop 2026](https://www.google.com/search?q=https://blogs.windows.com/msedgedev/2026/02/13/interop-2026/)

## おわりに

Interop 2025ではNavigation APIやCSS Anchor Positioningといった「アプリのようなWeb」を作るための基盤が固まりました。

Interop 2026では、それらをさらに発展させた**MPA View Transitions**や**Scroll-driven Animations**など、ユーザー体験を直接リッチにする機能が多く選出されています。また、`field-sizing` のような「長年欲しかった小さな機能」が含まれているのも嬉しいポイントです。

個人的には、昨年SPAで実現されたView Transitionが、今年は普通のWebサイト（MPA）でも使えることへの大きな期待を寄せています。これにより、ReactやVueなどのフレームワークを使わないシンプルなサイトでも、ネイティブアプリのような遷移表現が可能となるでしょう。

今年もWeb標準の進化から目が離せません。ぜひ皆さんもWPTダッシュボードをチェックしてみてください。
