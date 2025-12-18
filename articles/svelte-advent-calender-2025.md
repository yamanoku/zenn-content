---
title: "SvelteKitのクライアントサイドルーティングはどういう仕組みなの？"
emoji: "🔀"
type: "tech"
topics: ["svelte", "sveltekit"]
published: true
---

:::message
この記事は[Svelte Advent Calendar 2025](https://qiita.com/advent-calendar/2025/svelte)の18日目の記事です。
:::

こんにちは、[yamanoku](https://x.com/yamanoku)です。私は現在[Navigation API](https://developer.mozilla.org/en-US/docs/Web/API/Navigation_API)とそれに関連するAPIやエコシステムを調べるアドベントカレンダーの「[ひとりNavigation API Advent Calendar](https://qiita.com/advent-calendar/2025/navigation-api)」を実施しています。

その中でSPAにおけるクライアントサイドルーティングがどのような仕組みになっているかを調べており、[SvelteKit](https://svelte.dev/docs/kit/introduction)の内部を調査しています。今日はその実装内部で興味をもった部分があったので、それをまとめて記事にしてみました。

# Sveltekitのクライアントサイドルーティングの仕組み

SvelteKitのクライアントサイドルーティングは、他のルーターライブラリと同様にHistory APIとLocation APIを中心に構築されています。主にHistory APIの `pushState` や `replaceState` を用いて、ルーティング処理を行っています。

SvelteKitではクライアントサイドルーティング遷移をコンポーネント上で表現するにあたり、特定のルーター用のコンポーネント（`<Link>` や `<NuxtLink>` のようなもの）を呼び出すことはありません。代わりにHTMLの `<a>` 要素を使って表現できます。これは他メタフレームワークと比較して個人的に面白いなと思っている部分です。

しかし、ただの `<a>` 要素であればHTMLでのリンクとしてそのまま遷移してしまいます。それを防ぐためにSvelteKitでは通常のリンク遷移のインターセプト（中断）処理を実装して制御しています。今回はその内容を紹介していきます。

# インターセプト処理を見る

クライアントサイドルーティング処理は `kit/packages/kit/src/runtime/client/client.js` 上で実装されています。その中にインターセプト処理も含まれております。

https://github.com/sveltejs/kit/blob/85a57a03160ab68dc45da6acd57ac8670d7b1d26/packages/kit/src/runtime/client/client.js#L2461-L2579

まず最初のコメントを見ると[Page.js](https://github.com/visionmedia/page.js)についてが書かれています。Page.jsはExpressのルーターを参考にした小さなクライアントサイドルーティングのライブラリです。このライブラリの内容を一部参考にした部分があるようです。

https://github.com/sveltejs/kit/blob/85a57a03160ab68dc45da6acd57ac8670d7b1d26/packages/kit/src/runtime/client/client.js#L2463-L2464

<!-- textlint-disable -->
::: details 参考：Page.jsのクリックハンドラ部分

https://github.com/visionmedia/page.js/blob/master/index.js#L345-L426

:::
<!-- textlint-enable -->

その次に書かれているのは、以下の操作時に処理が無効となります。

- 左クリック以外でクリックされたとき [^1]
- Ctrl、Meta、Shift、Altキーが押されているとき
- `preventDefault` が既に呼ばれているとき

[^1]: `event.which !== 1` によって左クリック以外はfalseとなるのですが、[`which` プロパティは現在非推奨](https://developer.mozilla.org/ja/docs/Web/API/UIEvent/which)となっています

https://github.com/sveltejs/kit/blob/85a57a03160ab68dc45da6acd57ac8670d7b1d26/packages/kit/src/runtime/client/client.js#L2465-L2467

次はクリックされた要素から、親要素を辿ってアンカー（`<a>`）要素を探します。`find_anchor` 関数は、クリックイベントのターゲットから開始して、href属性を持つ `<a>` 要素が見つかるまで親要素を辿ります。存在しない場合は処理を無視します。

https://github.com/sveltejs/kit/blob/85a57a03160ab68dc45da6acd57ac8670d7b1d26/packages/kit/src/runtime/client/client.js#L2469-L2470


次に `get_link_info` 関数でリンクの詳細情報を取得しています。`target` 属性が `_parent`、`_top`、または `_self` 以外の値である場合、ブラウザのデフォルト動作となるようにしています。

https://github.com/sveltejs/kit/blob/85a57a03160ab68dc45da6acd57ac8670d7b1d26/packages/kit/src/runtime/client/client.js#L2472-L2480

同様に `download` 属性が設定されている場合もデフォルト動作となるようにしています。

https://github.com/sveltejs/kit/blob/85a57a03160ab68dc45da6acd57ac8670d7b1d26/packages/kit/src/runtime/client/client.js#L2500

次に `http:` または `https:` 以外のプロトコル（`mailto:`、`tel:` など）のリンクは、インターセプトせずにブラウザのデフォルト動作に任せます。

https://github.com/sveltejs/kit/blob/85a57a03160ab68dc45da6acd57ac8670d7b1d26/packages/kit/src/runtime/client/client.js#L2493-L2498

外部リンクまたは `data-sveltekit-reload` 属性が設定されている場合、`beforeNavigate` コールバックを実行した後、ブラウザのデフォルト動作になります。

https://github.com/sveltejs/kit/blob/85a57a03160ab68dc45da6acd57ac8670d7b1d26/packages/kit/src/runtime/client/client.js#L2506-L2516


同じpathnameでハッシュのみが異なる場合は、特別な処理が行われます。既に同じハッシュにいる場合は、要素へのスクロールのみを実行します。

https://github.com/sveltejs/kit/blob/85a57a03160ab68dc45da6acd57ac8670d7b1d26/packages/kit/src/runtime/client/client.js#L2521-L2544


最終的に `event.preventDefault()` が呼ばれ、ブラウザのデフォルトのページ遷移が防止されます。

https://github.com/sveltejs/kit/blob/85a57a03160ab68dc45da6acd57ac8670d7b1d26/packages/kit/src/runtime/client/client.js#L2559

その後、`requestAnimationFrame` と `setTimeout` を使用してブラウザの再描画を待ってから、SvelteKitの `navigate` 関数が呼び出されてクライアントサイドナビゲーションが実行されます。
`requestAnimationFrame` と `setTimeout` の組み合わせは、Core Web VitalsのInteraction to Next Paint（INP）スコアのペナルティを防ぐために、ブラウザが再描画する前にナビゲーションを開始することを保証するための処理です。

https://github.com/sveltejs/kit/blob/85a57a03160ab68dc45da6acd57ac8670d7b1d26/packages/kit/src/runtime/client/client.js#L2563-L2578

# Navigation APIのインターセプト処理について

SvelteKit内でのリンクのインターセプト処理を見ましたが、この処理部分はNavigation APIのインターセプト処理を使うことで簡略化できそうです。

Naviation APIについてを簡単に紹介すると、Histroy API処理をより現代のフロントエンド開発に適合させた後継のWeb APIです。ChromeとEdgeはすでにサポートされており、Safariは26.2よりサポート、Firefoxは来年1月以降にサポートが予定されております。

Navigation APIの `NavigateEvent` に `intecept()` というメソッドがあり、これを使用してインターセプト処理が実装できます。以下はHTML Living Standardにある[サンプルコード](https://html.spec.whatwg.org/multipage/nav-history-apis.html#dom-navigateevent-intercept-dev:~:text=The%20NavigateEvent%27s%20intercept()%20method%20allows%20intercepting%20a%20navigation%20and%20converting%20it%20into%20a%20same%2Ddocument%20navigation%3A)です。

```js
navigation.addEventListener("navigate", e => {
  // 一部のナビゲーション（例: クロスオリジン遷移）はインターセプトできない。
  // その場合はブラウザに通常どおり処理（遷移）させる。
  if (!e.canIntercept) {
    return;
  }

  // 同様に、フラグメント遷移やダウンロードも中断しない。
  if (e.hashChange || e.downloadRequest !== null) {
    return;
  }

  const url = new URL(event.destination.url);

  if (url.pathname.startsWith("/articles/")) {
    e.intercept({
      async handler() {
        // URLはすでに変更されているため、
        // 新しいコンテンツを取得する間はプレースホルダー（スピナーUIなど）を表示する。
        renderArticlePagePlaceholder();

        // 新しいコンテンツを取得し、準備ができたら表示する。
        // signalは読み取り専用のAbortSignalで、遷移キャンセル時にfetch処理を中断できる
        const articleContent = await getArticleContent(url.pathname, { signal: e.signal });
        renderArticlePage(articleContent);
      }
    });
  }
});
```

リンククリック処理をインターセプトせずとも、Navigation APIの `intecept` メソッドよりSvelteKitの `navigate` 関数を渡すことで簡略化できるかもしれません（詳細な内部実装までは追えてないのであくまで想像です）。

参考までにHistory APIで実装したクライアントサイドルーティングとNavigation APIのクライアントサイドルーティングのサンプルページがあるので実装内容を比較してみてください。

- [History APIで実装したクライアントサイドルーティング](https://codepen.io/yamanoku/pen/JoXepMb)
- [Navigation APIで実装したクライアントサイドルーティング](https://codepen.io/yamanoku/pen/dPMEvOV)

来年より本格的にクロスブラウザ対応となるNavigation APIですが、SvelteKit以外でも様々なルーティングライブラリに影響を与えてくれると思っています。来年以降でのNavigation APIの活用が広がっていくのが楽しみです。

Navigation APIについての仕様や動向は引き続き[ひとりNavigation API Advent Calendar](https://qiita.com/advent-calendar/2025/navigation-api)で追っていきたいと思っております。興味ある方は是非ご覧になってみてください。

## 謝辞

本記事は、DeepWikiによるリポジトリ内要約を協力してもらい作成されました。感謝申し上げます。
