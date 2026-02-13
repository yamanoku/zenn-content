---
title: "Interop 2026が始まりました！"
emoji: "🎇"
type: "tech"
topics: ["ブラウザ", "Interop", "Web標準"]
published: true
---

2月13日（日本時間）、ブラウザ間でのWeb標準の相互運用性を向上するプロジェクト「Interop」の2026年プロジェクトが始まりました。

この記事では、昨年のInterop 2025の振り返りと、新たにInterop 2026でどのAPIが重点対象として選出されたのかについて触れていきます。

前提知識として、Interopそのものの取り組みについての解説については、昨年書いた[Interop 2025のプロポーザル投票が始まりました！](https://zenn.dev/yamanoku/articles/interop-2025-opens-for-proposals)をご覧ください。

## Interop 2025の振り返り

2026年のInteropプロジェクトについて触れる前に、まずは昨年のInterop 2025プロジェクトについて振り返ってみましょう。

| Stable | Experimental |
| --- | --- |
| ![2026/2/13時点のInterop2025のStable版WPTダッシュボード。重点対象全体が95点、調査対象が36点、Chromeが99点、Edgeが98点、Firefoxが99点、Safariが98点](/images/interop-2026-launched/wpt.fyi_interop-2025_stable.png) | ![2026/2/13時点のInterop2025のExperimental版WPTダッシュボード。重点対象全体が97点、調査対象が36点、Chromeが99点、Edgeが99点、Firefoxが99点、Safariが99点](/images/interop-2026-launched/wpt.fyi_interop-2025_experimental.png) |

最新の安定版ブラウザでのリリースにおいては全体で95点まで網羅されており、プレリリース版においては97点まで網羅されていました。安定版でのテスト網羅率ではFirefoxが一歩リードしている形となっています。この結果はInterop 2024でも同様の結果となっています。

スコア自体は昨年同様高い数値をキープしており、安定版においてはChromeとEdgeでは昨年よりも1点スコアが高まっています。一方で調査対象は昨年と比較して

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

ここからは、Interop 2026で新しく重点対象として追加されたもの、および重要なアップデートがあった機能について見ていきます。2026年は20の重点対象と4つの調査領域で構成され、2025年からの継続した課題も多いです。

### Container style queries

Container style queriesは、`@container`アットルールを一つ以上の`style()`関数と組み合わせて使用し、コンテナのカスタムプロパティの計算値に基づいて要素にスタイルを適用します。

- [Tests dashboard](https://wpt.fyi/results/css/css-conditional/container-queries?label=experimental&label=master&aligned&view=interop&q=label%3Ainterop-2026-container-style-queries)
- [MDN](https://developer.mozilla.org/docs/Web/CSS/Reference/At-rules/@container#container_style_queries)

### CSS anchor Positioning

CSS Anchor positioningは、アンカーと呼ばれる要素を基準として特定の要素を、常に画面の特定の位置に固定できるCSSプロパティです。

- [Tests dashboard](https://wpt.fyi/results/css/css-anchor-position?label=master&label=experimental&aligned&view=interop&q=label%3Ainterop-2025-anchor-positioning)
- [MDN](https://developer.mozilla.org/docs/Web/CSS/Guides/Anchor_positioning)

### The `attr()` CSS function

`attr()`は、HTML要素の属性値を返します。その値を特定の型や単位として返すオプションもあります。

- [Tests dashboard](https://wpt.fyi/results/css/css-values?product=chrome&product=edge&product=firefox&product=safari&aligned=&view=interop&q=label%3Ainterop-2026-attr&label=master&label=experimental)
- [MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Values/attr)

### The `contrast-color()` CSS function

`contrast-color()`は、指定された前景色または背景色に対して、コントラストが保証された色を選択します。

- [Tests dashboard](https://wpt.fyi/results/css/css-color?product=chrome&product=edge&product=firefox&product=safari&aligned=&view=interop&q=label%3Ainterop-2026-contrast-color&label=master&label=experimental)

### Custom Highlights

Custom highlightsは、DOM に余分な要素を追加することなく、任意のテキスト範囲にスタイルを設定します。

- [Tests dashboard](https://wpt.fyi/results/css/css-highlight-api?product=chrome&product=edge&product=firefox&product=safari&aligned=&view=interop&q=label%3Ainterop-2026-custom-highlights&label=master&label=experimental)

### Dialog and popover additions

`<dialog>` HTML 要素は、確認プロンプトやデータ入力用のサブウィンドウなど、モーダルまたはモードレスなダイアログボックスを表します。`popover` HTML 属性は、他のページコンテンツの上にコンテンツを表示するオーバーレイを作成します。ポップオーバーは、HTML を使用して宣言的に表示することも、`showPopover()` メソッドを使用して表示することもできます。

今年の作業は以下に焦点を当てます：

- `<dialog closedby>` 属性：ダイアログを閉じるユーザー操作を設定します。例えば、`<dialog closedby="any">` は、ダイアログの外側をクリックして閉じることを許可します。
- `:open` CSS 擬似クラス：`<dialog>` のような開いた状態を持つ要素にマッチします。
- `popover="hint"` グローバル属性：`popover="auto"` 属性を持つ他のポップオーバーに従属するポップオーバーを作成します。これを使用すると、例えば自動ポップオーバーを閉じないツールチップを作成できます。

- [Tests dashboard](https://wpt.fyi/results/?product=chrome&product=edge&product=firefox&product=safari&aligned=&view=interop&q=label%3Ainterop-2026-dialogs-and-popovers&label=master&label=experimental)

### Fetch uploads and ranges

`fetch()`メソッドは、非同期 HTTP リクエストを行います。

今年の作業は以下に焦点を当てます：

- `Body` 内の `ReadableStream`：サーバーにデータをストリーミングするため。
- リクエストとレスポンスにおける `FormData` と `mime-type` のサポート。
- `Range` ヘッダーのサポート。

- [Tests dashboard](https://wpt.fyi/results/fetch?product=chrome&product=edge&product=firefox&product=safari&aligned=&view=interop&q=label%3Ainterop-2026-fetch&label=master&label=experimental)

### IndexedDB

IndexedDB API は、ローカルストレージのトランザクション型オブジェクトデータベースです。

IndexedDB ストアまたはインデックスからレコードとその主キーを返す `IDBObjectStore` および `IDBIndex` の `getAllRecords()` メソッドに焦点を当てます。レコードはバッチ処理や逆順で読み取ることができます。`getAllRecords()` メソッドは、大規模なデータセットでの読み取り操作を高速化します。

```js
const records = await objectStore.getAllRecords({
  query: IDBKeyRange.bound('A', 'M'),
  count: 100,
  direction: 'prev'
});
```

- [Tests dashboard](https://wpt.fyi/results/IndexedDB?product=chrome&product=edge&product=firefox&product=safari&aligned=&view=interop&q=label%3Ainterop-2026-indexeddb&label=master&label=experimental)

### JSPI for WASM

Wasm（WebAssembly）は、ポータブルなバイナリ命令形式です。JavaScript Promise Integration API (JSPI) を使用すると、外部機能への同期アクセスを前提として記述された Wasm アプリケーションが、その機能が実際には非同期である環境でもスムーズに動作できるようになります。

- [Tests dashboard](https://wpt.fyi/results/wasm/jsapi/jspi?product=chrome&product=edge&product=firefox&product=safari&aligned=&view=interop&q=label%3Ainterop-2026-jspi-for-wasm&label=master&label=experimental)

### Media pseudo-classes

`:playing`、`:paused`、`:seeking`、`:buffering`、`:stalled`、`:muted`、および `:volume-locked` CSS 擬似クラスは、その状態に基づいて `<audio>` および `<video>` 要素にマッチします。

- `:playing` — media is currently playing
- `:paused` — media is paused
- `:seeking` — user is seeking to a new position
- `:buffering` — media is buffering
- `:stalled` — playback has stalled
- `:muted` — audio is muted
- `:volume-locked` — volume cannot be changed

- [Tests dashboard](https://wpt.fyi/results/css/selectors/media?product=chrome&product=edge&product=firefox&product=safari&aligned=&view=interop&q=label%3Ainterop-2026-media-pseudo-classes&label=master&label=experimental)

### Navigation API

Navigation API は、ブラウザのナビゲーション操作を開始、傍受、または変更します。

今年は、Navigation API の相互運用性の継続的な向上と、ハンドラが解決されるまでコミットを遅延させる `MapsEvent.intercept()` の `precommitHandler` オプションに焦点を当てます。

```js
navigation.addEventListener('navigate', (e) => {
  e.intercept({
    async precommitHandler() {
      // Load critical resources before commit
      await loadCriticalData();
    },
    async handler() {
      // Render the new view
      renderPage();
    }
  });
});
```

- [Tests dashboard](https://wpt.fyi/results/navigation-api?product=chrome&product=edge&product=firefox&product=safari&aligned=&view=interop&q=label%3Ainterop-2026-navigation&label=master&label=experimental)

### Scoped custom element registries

`CustomElementRegistry()` コンストラクタは、グローバルな `window.customElements` レジストリとは別の、新しいカスタム要素レジストリを作成します。複数のレジストリを作成することは、同じタグ名を持つ複数のカスタム要素を共存させる場合に便利です。

```js
const registry = new CustomElementRegistry();
registry.define('my-button', MyButtonV2);
shadowRoot.registry = registry;
```

- [Tests dashboard](https://wpt.fyi/results/custom-elements/registries?product=chrome&product=edge&product=firefox&product=safari&aligned=&view=interop&q=label%3Ainterop-2026-scoped-custom-element-registries&label=master&label=experimental)

### Scroll-driven Animations

スクロール量に応じてアニメーションを制御する機能です。`animation-timeline`、`scroll-timeline`、および `view-timeline` CSS プロパティは、ユーザーのスクロール位置に基づいてアニメーションを進行させます。

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

- [Tests dashboard](https://wpt.fyi/results/scroll-animations?product=chrome&product=edge&product=firefox&product=safari&aligned=&view=interop&q=label%3Ainterop-2026-scroll-driven-animations&label=master&label=experimental)
- [MDN](https://developer.mozilla.org/docs/Web/CSS/CSS_scroll-driven_animations)

### Scroll snap

Scroll snapは、スクロールコンテナ内のパンとスクロールの動作を制御します。

```css
.carousel {
  scroll-snap-type: x mandatory;
  overflow-x: scroll;
}
.carousel > * {
  scroll-snap-align: center;
}
```

### The `shape()` CSS function

CSS の`shape()` 関数は、line（線）、move（移動）、curve（曲線）などの一連のコマンドを使用して形状を作成します。これは `clip-path` や `shape-outside` と共に使用できます。

```css
.element {
  clip-path: shape(
    from 0% 0%,
    line to 100% 0%,
    line to 100% 100%,
    curve to 0% 100% via 50% 150%,
    close
  );
}
```

### View Transitions API

View Transition APIを使用すると、ドキュメントの異なる状態間、またはマルチページアプリケーションの異なるドキュメント間で、アニメーション化された視覚的な遷移を作成できます。

今年の作業は以下に焦点を当てます：

- 同一ドキュメント（same-document）のビュートランジションの相互運用性の継続的な向上。
- `<link>`、`<script>`、`<style>` 要素の `blocking="render"` 属性：外部スクリプトやスタイルシートが読み込まれるまで、または特定の要素が DOM に存在するまでレンダリングをブロックします。
- `<link rel="expect">` 属性：href 値が参照する要素がドキュメントに接続され、完全に解析されるまでレンダリングをブロックするようブラウザにヒントを与えます。
- `:active-view-transition-type()` CSS 擬似クラス：指定されたタイプでアクティブなビュートランジションが開始された場合にのみマッチします。
- クロスドキュメント（cross-document）ビュートランジション

* [MDN: View Transition API](https://developer.mozilla.org/docs/Web/API/View_Transition_API)

### Web Compat

Web Compatは特定の技術ではなく、すでにリリースされているAPIであるもののバグやWeb標準からの逸脱によって問題が起きているものたちのグループになります。今年は以下の互換性の問題に焦点を当てます。

* ESM module loading（[プロポーザル](https://github.com/web-platform-tests/interop/issues/1105)）
* Timing of scroll events relative to animation events（[プロポーザル](https://github.com/web-platform-tests/interop/issues/1126)）
* `user-select`（[プロポーザル](https://github.com/web-platform-tests/interop/issues/1000)）

### WebRTC

WebRTC API は、ブラウザ間で直接リアルタイム通信チャネルを確立します。ビデオ会議アプリケーションで一般的に使用されます。

今年の作業は以下に焦点を当てます：

- WebRTC の相互運用性の継続的な向上。
- WebRTC Interop 2025 重点分野からの残りの失敗テストの修正。

### WebTransport

WebTransport API は、HTTP/3 プロトコルを使用してクライアントとサーバー間でデータを送信します。

### The `zoom` CSS property

要素のサイズを拡大縮小する CSS プロパティ`zoom`の相互運用性の向上を2025年から引き続き対応します。`transform`プロパティとは異なり、ズームされた要素はページのレイアウトに影響を与えます。

- [Tests dashboard](https://wpt.fyi/results/?label=master&label=experimental&aligned&view=interop&q=label%3Ainterop-2026-zoom)

## Interop 2026での調査対象について

Interopでは重点対象だけでなく、テスト合格率では進捗を測定できない分野の長期的な相互運用性向上を目的とした調査も実施されています。新しいテスト機能の追加や、プラットフォーム機能のテストカバレッジの拡大などを検討されています。今年は以下が調査対象となります。

* Accessibility testing
* JPEG XL
* Mobile testing
* WebVTT

## 参加企業によるInterop 2026プロジェクト開始にまつわる記事

### Apple
https://webkit.org/blog/17818/announcing-interop-2026/

### Google
https://web.dev/blog/interop-2026?hl=en

### Igalia
https://www.igalia.com/news/interop-2026.html

### Microsoft
https://blogs.windows.com/msedgedev/2026/02/12/microsoft-edge-and-interop-2026/

### Mozilla
https://hacks.mozilla.org/2026/02/launching-interop-2026/

## おわりに

Interop 2025ではNavigation APIやCSS Anchor Positioningといった「アプリのようなWeb」を作るための基盤が固まりました。

Interop 2026では、それらをさらに発展させた**MPA View Transitions**や**Scroll-driven Animations**など、ユーザー体験を直接リッチにする機能が多く選出されています。また、`field-sizing` のような「長年欲しかった小さな機能」が含まれているのも嬉しいポイントです。

個人的には、昨年SPAで実現されたView Transitionが、今年は普通のWebサイト（MPA）でも使えることへの大きな期待を寄せています。これにより、ReactやVueなどのフレームワークを使わないシンプルなサイトでも、ネイティブアプリのような遷移表現が可能となるでしょう。

今年もWeb標準の進化から目が離せません。ぜひ皆さんもWPTダッシュボードをチェックしてみてください。
