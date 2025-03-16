---
title: "リアクティブの進化からalien-signalsを知ろう"
emoji: "👾"
type: "tech"
topics: ["Signals", "JavaScript", "Vue", "tc39"]
published: false
publication_name: "comm_vue_nuxt"
---

現代のWebアプリケーションを開発する上で、**あらゆるデバイスにおける操作に応じて画面がスムーズに変化できるのは、ユーザー体験を考慮する上で非常に重要なものです。この「スムーズな変化」を実現するための仕組みの1つが「リアクティブ」です。

今回は、このリアクティブにまつわる歴史を辿りつつ、2025年の1月にメジャーバージョンとなった注目のライブラリである**alien-signals**について解説していきます。

## リアクティブとは？

そもそもリアクティブとは何かについてを説明します。

リアクティブを日本語にすると「即応性」と訳されます。リアクティブシステムは **「データが変更されたときに、それに関連する処理や画面表示を自動的に更新する仕組み」** です。これはGoFのデザインパターンにおける「オブザーバーパターン」に基づいており、データの変更を監視し必要な部分に通知することで、アプリケーション全体の状態を一貫して保つことができます。

フロントエンド開発の歴史においてはKnockout observablesやMeteor Tracker、RxJSのようなライブラリが存在し、リアクティブなプログラミングの基礎を築いていました。状態管理ライブラリであるMobXも、背後では同様の原理に基づいてオブジェクトのプロパティの変化を監視し、関連する部分を更新していました[^1]。

[^1]: [The fundamental principles behind MobX | HackerNoon](https://hackernoon.com/the-fundamental-principles-behind-mobx-7a725f71f3e8)

## なぜリアクティブプログラミングが重要なのか？

このリアクティブプログラミングという概念はなぜ重要となっているのでしょうか？その理由は以下の通りです。

### 複雑化するUIの効率的な管理

現代のWebアプリケーションは、以前に比べて遥かに複雑なユーザーインターフェース（UI）を持つようになっています。

データ量の増加やインタラクティブ性の向上に伴い、アプリケーションの状態（データ）とUIの同期を手動で管理することは、開発者の負担が大きく、エラーも発生しやすくなります。

リアクティブプログラミングは、データが変更された際に、その変更をUIに自動的に反映させる仕組みを提供することで、この課題を解決します。

### 宣言的なプログラミング

リアクティブプログラミングは「何を表示するか」を宣言的に記述するスタイルを促進します。

開発者は、状態の変化に応じてUIをどのように更新するかという手続き的なコードを書く代わりに、データの流れとそれにまつわるUIの関連性を定義するだけで済みます。これはいわゆる「宣言的UI」の考え方に基づいているものです。

### 状態管理の簡略化

アプリケーションの状態管理は、規模が大きくなるにつれて複雑になります。リアクティブプログラミングは、状態の変化を監視し、それに応じて関連するUIや他の状態を自動的に更新する機能を提供することで、状態管理の複雑さを軽減します。

## Push型、Pull型、Push-Pull型

リアクティブシステムは、状態の変化を依存している部分にどのように伝えるかによって、主にPush型、Pull型、Push-Pull型に分類できます。次はそれぞれの特徴を見ていきます。

### Push型

* このモデルでは、**状態が変更されると、その状態に依存しているすべてのシステムに自動的に変更を通知（push）** される
  * 依存している側は、通知を受け取るたびに再計算や副作用が実行される
* RxJSがこのPush型のモデルを採用しており、状態の変更をストリームとして扱うことができる
* 初期のリアクティブプログラミングではこのPush型のモデルが一般的だったが、**不要な再計算やUIの早期更新の問題**が指摘されていた

### Pull型

* Pull型のモデルでは、**状態が変更されても、依存している側は直ちに通知を受けない**
* 代わりに、**依存している側が必要になったタイミング** (例えば、UIのレンダリング時や値が実際に要求された時) に、**最新の値を状態から「引き出す (pull)」** 必要がある
* これにより、状態が頻繁に変化する場合でも、実際に必要になるまで計算を遅延させることができて**パフォーマンスの最適化**に繋がる
* ReactやVue.jsのような仮想DOMを採用するものや、ElmではPull型のアルゴリズムが採用されている

### Push-Pull型

* Push-Pull型のモデルは、**Push型とPull型の利点を組み合わせたハイブリッドなアプローチ**
* 状態が変更されると、依存している部分に対して **「dirty(ダーティ)」** であるというフラグを立てるなどの軽い通知 (Push) を行う
* その後、実際にその依存している部分の値が必要となった際に最新の値を計算する (Pull) という流れを取り込む

近年では**Pull型**や**Push-Pull型**のアプローチが、パフォーマンスと効率性の観点から多くのリアクティブプログラミングで採用される傾向にあります。これにより、不要な計算を避け、必要な時だけ最新の状態を反映させることが可能になり、よりスムーズなアプリケーション体験を提供できます。

## Fine-Grained ReactivityとSignals

リアクティブプログラミングにおいて近年注目を集めているのが、 **Fine-Grained Reactivity（きめ細かいリアクティビティ）** [^2]という考え方です。これはReactのようにアプリケーション全体を再レンダリングするのではなく[^3]、**本当に変更があったデータに関連するごく一部だけを更新する**というものです。

[^2]: [A Hands-on Introduction to Fine-Grained Reactivity - DEV Community](https://dev.to/ryansolid/a-hands-on-introduction-to-fine-grained-reactivity-3ndf)

[^3]: ReactではReact.memoやuseMemo、useCallbackを使うことでレンダリングのパフォーマンスを悪化させないようにしたり、新たなコンパイル戦略でもある「React Compiler」によって仮想DOMのレンダリングコストを抑えようとしています。

このFine-Grained Reactivityを実現するための重要なプリミティブ（基本的な構成要素）となるAPIが **「Signals（シグナル）」** です。Signalsは、**リアクティブプログラミング**を実現するための重要なプリミティブであり、データの変更に応じて自動的に影響を受ける部分を更新する仕組みの中心となります。

Signalsの基本的な機能は以下のようになります。

* **値の保持 (State/Writable Signal)**
* **値へのアクセス (Getter)**
* **値の更新 (Setter)**
* **算出された値 (Computed/Derived Signals)**
* **副作用 (Effects/Watchers)**

それぞれのAPIは異なりますが、[Solid](https://www.solidjs.com/docs/latest/api#createsignal)をはじめ、[Angular](https://angular.dev/guide/signals)、[Preact](https://preactjs.com/guide/v10/signals/)、[Qwik](https://qwik.dev/docs/components/state/#usesignal)など、多くのモダンなフレームワークがSignalsを採用しています。

## alien-signalsとは？

![alien-signalsのロゴ](https://github.com/stackblitz/alien-signals/raw/master/assets/logo.png =300x)

このようなSignalsの潮流の中で登場したのが **alien-signals** です。alien-signalsは、**非常に軽量なリアクティブライブラリ**であることを特徴としています。

https://github.com/stackblitz/alien-signals

このライブラリが生まれた背景として、作者であるJohnson氏が[Vue 3.4のリアクティビティ・システムの最適化](https://github.com/vuejs/core/pull/5912)に関わったことが挙げられます。その後、[Vue 3.5がPreactに似たPull型のアルゴリズムに切り替わった](https://github.com/vuejs/core/pull/10397)ことを受け、Vue.jsとは別のリアクティビティライブラリを研究しはじめたことが、alien-signalsの開発のきっかけとなりました。

このライブラリではPush-Pull型を採用しています。さらに[Vue 3.6からはalien-signalsの実装がリアクティブプログラミングの基盤として取り入れられる](https://github.com/vuejs/core/pull/12349)ことになりました。

### alien-signalsの特徴

値を保持する `signal`、算出された値として計算する `computed`、リアクティブな処理を定義する `effect`、そして `effect` の有効範囲を管理する `effectScope` など、Signalsの基本的なAPIを提供しています。

```javascript
import { signal, computed, effect } from 'alien-signals';

const count = signal(1);
const doubleCount = computed(() => count() * 2);

effect(() => {
  console.log(`Count is: ${count()}`);
}); // Console: Count is: 1

console.log(doubleCount()); // 2

count(2); // Console: Count is: 2

console.log(doubleCount()); // 4
```

`signal` でリアクティブな値 `count` を作成し、`effect` 内でその値を監視しています。`count` の値が変更されると、`effect` 内の処理が自動的に再実行されます。`effectScope` を使うことで、`effect` のライフサイクルを管理できます。

内部の処理では、再帰呼び出しを避けるための最適化 (`propagate`, `checkDirty` 関数) が施されており、効率的な更新を実現しています。

また、`createReactiveSystem()` を使うことで、alien-signalsのコアアルゴリズムを再利用して、独自のSignals APIの構築も可能です。

```javascript
const system = createReactiveSystem({
  updateComputed(computed: Computed) {
    return computed.update();
  },
  notifyEffect(watcher: subtle.Watcher) {
    if (watcher.flags & alien.SubscriberFlags.Dirty) {
      watcher.run();
      return true;
    }
    return false;
  },
});
```

### 派生プロジェクト(2025/3/15時点)

alien-signalsは、現在さまざまなフレームワークや言語で派生して実装が進められています。以下はその派生プロジェクトの一部です。

- [YanqingXu/alien-signals-in-lua](https://github.com/YanqingXu/alien-signals-in-lua)
- [medz/alien-signals-dart](https://github.com/medz/alien-signals-dart)
- [delaneyj/alien-signals-go](https://github.com/delaneyj/alien-signals-go)
- [Rajaniraiyn/react-alien-signals](https://github.com/Rajaniraiyn/react-alien-signals)
- [hunghg255/reactjs-signal](https://github.com/hunghg255/reactjs-signal)
- [CCherry07/alien-deepsignals](https://github.com/CCherry07/alien-deepsignals)
- [gn8-ai/universe-alien-signals](https://github.com/gn8-ai/universe-alien-signals)

## TC39への標準化提案

Signalsは、現在Ecma Internationalの技術委員会39（通称TC39）にてECMAScriptでの標準化提案が進められています。現在はStage 1（Proposal）になります。

https://github.com/tc39/proposal-signals

Signalsの仕様提案の推進者には、BloombergのDaniel Ehrenberg氏、Emberの開発者であるYehuda Katz氏、Google社内で使用されるWizフレームワークのメンテナのJatin Ramanathan氏、SvelteメンテナのDominic Gannaway氏などが名を連ねています。

alien-signalsのようなライブラリや各フレームワークによるSignalsの実装がより浸透していくと、この標準化の動きをさらに加速させる可能性があります。将来的には、JavaScriptの標準APIとしてSignalsが利用できるようになれば、アプリケーション側はSignalsを採用するために変更を加える必要はなくなります。また、複数のマイクロフロントエンドで共通のデータ層を共有する場合などに役立つとされています。

## まとめ

- リアクティブプログラミングは、現代のフロントエンドフレームワークにおける重要な概念であり、状態の変化に応じてUIを効率的に更新するための基盤となる
- 複数の主要なフレームワークが、それぞれ独自の方法でリアクティビティを実現しているが、近年「Signals」というパターンに収束する傾向が見られる
- alien-signalsは、軽量なリアクティブプログラミングのライブラリであり、Vue 3.6から採用されている
- TC39におけるSignalsの標準化の動きは、JavaScriptエコシステム全体に大きな影響を与える可能性があり、フレームワーク間の相互運用性や開発の共通理解を促進することが期待される

## 宣伝: Vue.js v-tokyo Meetup #22

Vue.js日本ユーザーグループが主催する「Vue.js v-tokyo Meetup #22」では、alien-signalsのメジャーリリースを記念して、Webフロントエンドフレームワークのリアクティビィティについてキャッチアップできるイベントが3/28に開催されます。

https://vuejs-meetup.connpass.com/event/343338/

alien-signalsやVue.jsについてはもちろん、PreactやAngular、Svelteなどのフレームワークにおけるリアクティブについても学ぶことができる貴重な機会です。ぜひご参加ください！

## 参考情報

* [Reactive programming - Wikipedia](https://en.wikipedia.org/wiki/Reactive_programming)
* [The Reactive Manifesto](https://www.reactivemanifesto.org/)
* [pzuraq | blog | What Is Reactivity?](https://www.pzuraq.com/blog/what-is-reactivity)
* [pzuraq | blog | What Makes a Good Reactive System?](https://www.pzuraq.com/blog/what-makes-a-good-reactive-system#observables-streams-and-rxjs)
* [EmberConf 2024 - Standardizing Autotracking Via TC39 Signals with Daniel Ehrenberg](https://youtu.be/ji7zSHCX6d8?list=TLGGE0qWPG3HOW4xNTAzMjAyNQ)
* [The Evolution of Signals in JavaScript - DEV Community](https://dev.to/this-is-learning/the-evolution-of-signals-in-javascript-8ob)
* [リアクティビティーの探求 | Vue.js](https://vuejs.org/guide/extras/reactivity-in-depth.html)
* [ついにやってくるSignals！Angularはどう変わるのか？【ng-japan OnAir #63】 - YouTube](https://www.youtube.com/watch?v=gCKw5OyRPiA)
