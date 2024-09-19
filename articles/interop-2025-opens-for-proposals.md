---
title: "Interop2025のプロポーザル投票が始まりました！"
emoji: "🗳️"
type: "tech"
topics: ["ブラウザ", "Interop", "Web標準"]
published: true
---

ブラウザ間でのWeb標準の相互運用性を向上するプロジェクトの「Interop」で、来年の2025年に重点対象とするAPIへのプロポーザル投票がはじまりました。

この記事ではそもそものInteropとについての解説、プロポーザルについて、どのようなものが重点対象になっていたか、今後の選考スケジュールについてを触れていきたいと思います。

## Interopとは何か？

はじめにそもそもInteropとはどういう取り組みなのか？についてを簡単に触れておきます。

Interopプロジェクトは、Web Platform Tests（以下、WPT）プロジェクトの一部であり、異なるウェブブラウザ（Google Chrome、Microsoft Edge、Mozilla Firefox、Safari）間でウェブサイトやウェブアプリが同じように表示され、動作することをにするための取り組みです。

このプロジェクトはブラウザのレンダリングエンジンに多大な貢献をしているブラウザベンダ（Apple、Google、Microsoft、Mozilla）とコンサルティング企業（Bocoup、Igalia）の代表者チームによって進められています。

どれだけ相互互換性を維持できるようになっているかについてはWPTのダッシュボードにて確認できるようになっています。Experimental（実験版）とStable（安定版）のそれぞれのブラウザでのテスト進捗結果が見れます。

## プロポーザルの投票について

期待していた新たなAPIがブラウザに実装されたときに「早くクロスブラウザで安定的に使えるようになったら…」とやきもきしている方もいるかもしれません。そんな相互運用性を高めてほしいと思ったときにInteropのIssueよりプロポーザルとして投票できます。

![InteropのIssue選択画面。Focus Area Proposal、Investigation Effort Proposal、Other、Test Change Proposalの4項目がある](/images/interop-2025-opens-for-proposals/interop-issue-templates.png)

Interop 2025に向けたIssueとして以下の２つから投票できます。

- Focus Area Proposal
- Investigation Effort Proposal

「**Focus Area Proposal**」は仕様が定められていて適切なテストケースが存在するもので「**Investigation Effort Proposal**」は逆に仕様やテストケースなどで不足しているものがある場合のプロポーザルとして使われます。

どの種類のプロポーザルを投票すればよいかわからない場合は「Investigation Effort Proposal」の方を選ぶと良いとされています。

## プロポーザルの採用について

プロポーザルとして投票されたもののすべてが対象となるわけではなく、[いくつかの選考フロー](https://github.com/web-platform-tests/interop/blob/main/2025/selection-process.md)を経て最終的にどのAPIに注力するかを決定されます。

注力するに値する優れたプロポーザルの観点としては以下が挙げられます。

- **具体的**: 具体的な相互運用性の問題を特定できるか
- **インパクトがあるか**: 明確な使用例や事例を挙げられるか
- **テスト済みであるか**: WPTにてすでにテストを通過しているものか
- **安定的か**: Web標準として広く受け入れられているものか
- **価値があるか**: 開発者およびユーザーが相互運用性の向上で恩恵を受けられるか

さらに「Standards Positionsとしてポジティブであるか」「一部のブラウザでは動いていない」「APIがすでに広く実装されているか」「公開されているバグトラッカーですでに認知されているか」「アクセシビリティ・国際化・プライバシー・セキュリティの面で影響があるか」といった観点についても加点要素となります。

また、開発者向けサーベイの「[State of HTML](https://stateofhtml.com/en-US)」「[State of CSS](https://stateofcss.com/en-US)」「[State of JavaScript](https://stateofjs.com/en-US)」などの結果によっても今後のInteropにおいて参考とされるため、積極的に参加して回答していくと良さそうです。

## これまでのInteropについて振り返る

これまでのInterop重点対象となったプロポーザルにはどのようなものがあったか、についてを振り返ってみます。

### [2021](https://wpt.fyi/interop-2021)

| Stable | Experimental |
| --- | --- |
| ![Interop2021のStable版WPTダッシュボード。Chromium系が97点、Firefoxが94点、Safariが92点](/images/interop-2025-opens-for-proposals/wpt.fyi_interop-2021_stable.png) | ![Interop2021のExperimental版WPTダッシュボード。Chromium系が98点、Firefoxが94点、Safariが94点](/images/interop-2025-opens-for-proposals/wpt.fyi_interop-2021.png) |

- Aspect Ratio
- Flexbox
- Grid
- `position: sticky`
- Transforms

2021年の重点対象は計5件になります。今や問題なく使えるようになったAspect Ratioや `position: sticky` を見ると懐かしく感じられます。

### [2022](https://wpt.fyi/interop-2022)

| Stable | Experimental |
| --- | --- |
| ![Interop2022のStable版WPTダッシュボード。重点対象全体が82点、調査対象が80点、Chromium系が87点、Firefoxが90点、Safariが96点](/images/interop-2025-opens-for-proposals/wpt.fyi_interop-2022_stable.png) | ![Interop2022のExperimental版WPTダッシュボード。重点対象全体が85点、調査対象が80点、Chromium系が92点、Firefoxが91点、Safariが96点](/images/interop-2025-opens-for-proposals/wpt.fyi_interop-2022.png) |

- Aspect Ratio
- Cascade Layers
- Color Spaces and Functions
- Containment
- Dialog Element
- Flexbox
- Forms
- Grid
- Scrolling
- Sticky Positioning
- Subgrid
- Transforms
- Typography and Encodings
- Viewport Units
- Web Compat

2022年の重点対象は計15件になります。2021年にあった重点対象も継続しつつ、この年から「Focus Area」（重点対象）と「Investigations」（調査対象）の項目が分かれています。

「Web Compat」の項目は特定の技術ではなく、すでにリリースされているAPIであるもののバグや標準からの逸脱によって問題が起きているものたちのグループになります。

2022年においてはViewport Unitsが唯一の100%テストカバレッジを達成しています。

### [2023](https://wpt.fyi/interop-2023)

| Stable | Experimental |
| --- | --- |
| ![Interop2023のStable版WPTダッシュボード。重点対象全体が83点、調査対象が85点、Chromium系が97点、Firefoxが86点、Safariが95点](/images/interop-2025-opens-for-proposals/wpt.fyi_interop-2023_stable.png) | ![Interop2022のExperimental版WPTダッシュボード。重点対象全体が95点、調査対象が85点、Chromium系が99点、Firefoxが98点、Safariが97点](/images/interop-2025-opens-for-proposals/wpt.fyi_interop-2023.png) |

- Border Image
- Color Spaces and Functions
- Container Queries
- Containment
- CSS Math Functions
- CSS Pseudo-classes
- Custom Properties
- Flexbox
- Font Feature Detection and Palettes
- Forms
- Grid
- `:has()`
- Inert
- Masking
- Media Queries 4
- Modules
- Motion Path
- Offscreen Canvas
- Pointer and Mouse Events
- Scrolling
- Subgrid
- Transforms
- URL
- Web Codecs (video)
- Web Compat 2023
- Web Components

2023年からの重点対象は合計26件になります。

2022年から継続しているもの（計7件）も含まれていますが、歴代の中では最多となる採択数になりました（[プロポーザルは87件も集まったそうです](https://github.com/orgs/web-platform-tests/projects/2)）。そのためInteropチーム内で少なくとも２つの参加組織からの支持と反対がないことを示すコンセンサスによって、採用するプロポーザルを絞っていったそうです。

2023年においてはMedia Queries 4、Inert、`:has()`、Font Feature Detection and Palettesが100%テストカバレッジを達成しています。

### [2024](https://wpt.fyi/interop-2024)

| Stable | Experimental |
| --- | --- |
| ![Interop2024のStable版WPTダッシュボード。重点対象全体が74点、調査対象が12点、Chromeが90点、Edgeが89点、Firefoxが87点、Safariが83点](/images/interop-2025-opens-for-proposals/wpt.fyi_interop-2024_stable.png) | ![Interop2024のExperimental版WPTダッシュボード。重点対象全体が89点、調査対象が12点、Chromeが98点、Edgeが93点、Firefoxが93点、Safariが97点](/images/interop-2025-opens-for-proposals/wpt.fyi_interop-2024.png) |

※ 2024のスクリーンショットは2024/9/19時点のものです。

- Accessibility
- CSS Nesting
- Custom Properties
- Declarative Shadow DOM
- `font-size-adjust`
- HTTPS URLs for WebSocket
- IndexedDB
- Layout
- Pointer and Mouse Events
- Popover
- Relative Color Syntax
- `requestVideoFrameCallback`
- Scrollbar Styling
- `@starting-style` & `transition-behavior`
- Text Directionality
- `text-wrap: balance`
- URL

2024年からの重点対象は合計17件になります。こちらは現在も対応が行われております。今年のInvestigationsには[WebAssemblyのテスト](https://github.com/web-platform-tests/interop-2024-wasm)も対象となっているようです。

## 今後のスケジュール

以下がInterop 2025プロポーザル選考プロセスのスケジュールです（変更される可能性があります）。

- プロポーザルの投票期間（～3週間）: 2024/9/17 ～ 2024/10/9
- 提案されたプロポーザル選定: 2024/12/19以前
- Interop 2025プロジェクト公開：2025/2前半

## 参加企業によるInterop 2025のお知らせ記事

### Apple
https://webkit.org/blog/15942/get-ready-for-interop-2025-your-chance-to-shape-the-web/

### Bocoup
https://www.bocoup.com/blog/interop-2025-call-for-proposals

### Google
https://web.dev/blog/interop2025-proposals

### Igalia
https://www.igalia.com/2024/09/17/Interop-2025-Opens-for-Proposals.html

### Microsoft
https://blogs.windows.com/msedgedev/2024/09/17/interop-2025-call-for-proposals/

## おわりに

この記事ではInterop 2025にまつわる紹介をしました。現時点ですでに[いくつかのAPIに関してプロポーザルが投票されています](https://github.com/web-platform-tests/interop/labels/focus-area-proposal)。

リアクションやコメントで応援したり、ブログやSNSなどを通じて広めることでより多くの人が知るきっかけになるかもしれません。私の推しAPIでもある「[Navigation API](https://developer.mozilla.org/en-US/docs/Web/API/Navigation_API)」もそろそろ採用されてくれればいいなを願っています（[該当Issue](https://github.com/web-platform-tests/interop/issues/709)で `:+1:` をしてきてささやかな応援をしています）。

これらのプロポーザルの中からどれが重点対象となりクロスブラウザで活用できるのか、今後の選考結果が楽しみです。
